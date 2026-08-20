---
schema_version: "1.0.0"
document_id: "78202efd01f752a6b38126d56588124e2a7c5bda67f493e7d559b2ff9c80d8d5"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/how-simulations-reduce-guesswork"
published_at: "2026-02-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:6c0b6abf59a240e3ddd43926354b03abc86d9f6644f3a58f42a9c90fea2903cc"
---

# How simulations reduce the guesswork in our infrastructure decisions

At Depot we use AWS for our CI builds for customers. To provide the fastest experience we create a standby pool of machines that are warmed and ready to take jobs. See Jacob’s[blog](https://depot.dev/blog/faster-ec2-boot-time) about it. When a customer has a new CI job we take a machine out of the standby pool and make it active.


## The problem


The lifecycle of a machine is: standby boot, standby ready, customer job arrives, active boot, active running job, terminated.


If the standby pool runs out of capacity, the customer job needs to wait and therefore job latency increases. The issue is that standby machines cost money when idle.


The arrival rate of jobs can be complicated. Jobs can arrive in huge bursts (like when dependabot passes through). We need extra capacity to handle a large influx of jobs, but we also need to balance the cost of running the system.


How do you right-size standby pools when too few means latency spikes and too many means paying for idle instances?


Over time we have built up a standby pool capacity algorithm that has over a dozen parameters. We developed it as we gained an understanding of typical customer job demands. However, tuning it and understanding each parameter's impact is tricky.


Instead of guessing how to improve, we built a simulator using real customer data. Next, we used hyperparameter optimization to find better parameter values. Finally, we could make changes to the core mechanics of our algorithm with confidence.


## Simulation


Before doing any optimization I like to build the smallest possible simulator that still captures the idea I care about.


Here is a toy example using[SimPy](https://simpy.readthedocs.io/en/latest/contents.html) :


```text
import   simpy


def   job  (env, name, machine_pool):
arrive   =   env.now
print  (  f  "  {  arrive  :5.1f  }    {  name  }   arrives"  )


with   machine_pool.request()   as   req:
yield   req
start   =   env.now
wait   =   start   -   arrive
print  (  f  "  {  start  :5.1f  }    {  name  }   starts after waiting   {  wait  :.1f  }  s"  )


job_run_time   =   5
yield   env.timeout(job_run_time)


done   =   env.now
print  (  f  "  {  done  :5.1f  }    {  name  }   finishes"  )


def   arrival_process  (env, machine_pool):
i   =   0
while   True  :
interarrival   =   random.expovariate(  1   /   3.0  )
yield   env.timeout(interarrival)


i   +=   1
env.process(job(env,   f  "job-  {  i  }  "  , machine_pool))


env   =   simpy.Environment()


# three warm machines in pool.
machine_pool   =   simpy.Resource(env,   capacity  =  3  )


env.process(arrival_process(env, machine_pool))
env.run(  until  =  40  )
```


This model represents a simple queueing system. CI jobs arrive over time and the job runs on a pool of already-booted machines. However, if all machines in the pool are busy, the jobs wait; in other words, if jobs have to wait then they have a high latency.


The` simpy.Environment` is the event loop that drives the whole simulation. Simulated time only moves forward when processes yield something.


The` job` function models a single unit of work and it tries to acquire a machine from the pool:


```text
with   machine_pool.request()   as   req:
yield   req
```


` machine_pool` is a` simpy.Resource` with a capacity of three. If all three machines are busy, the` yield req` pauses the job until one becomes free. This simulates queuing.


Once the machine is acquired, the job "runs" for five simulated seconds by yielding:


```text
yield   env.timeout(job_run_time)
```


Nothing is sleeping in real time here. We are just advancing simulated time.


When the timeout finishes, the job prints and exits. Also, releasing the machine happens automatically when the` with` block ends.


The` arrival_process` is what keeps injecting jobs into the system. On each iteration of its loop it waits for the next job and then enqueues it. For a real simulation you would use real data, but for the sake of brevity we use an exponential distribution for the next arrival.


After waiting, it spawns a new job process using the machine pool:


```text
env.process(job(env,   f  "job-  {  i  }  "  , machine_pool))
```


At the bottom we wire everything together. We create a pool with three warm machines and start the arrival process. Then we run for forty simulated seconds.


This is a fairly simple model but it shows the core simpy components. Sometimes a simple model is all that is needed. Certainly you can keep adding to the model's mechanics to get a more precise description. However, it is always important to relate a model back to the real world. For that we need to calibrate the model.


## Calibration


Instead of feeding a simulation a random distribution of jobs, you can give it real data. For our modeling we used real customer time-series metrics we measure about job arrival and job duration. CI job arrival time is very difficult to predict and jobs can come in bursts of thousands. Job duration can be very short or very long. As a result, using a predetermined distribution function does not necessarily give confidence in the simulation's results.


So, we instead fed real job data into the simulation and compared the results against our real-world latency and cost. The calibration's goal is to determine how close your simulation test code is to a production system.


Ideally, if your code's algorithms are separable enough from the rest of your code base, you could just use it as the simulator directly. Otherwise, you'll need to create a close enough approximation. Ultimately, doing so is a qualitative exercise. The overriding question is, does the simulation produce results that are close enough to your historical data to derive some meaningful conclusions.


Overall, calibration takes time because you continue revisiting the simulation to either update it to be closer to real code **or** simplify it to make getting the results faster.


Think of this as your most biased step. Track what is different and perhaps justify if need be.


Once we can reproduce a system in a simulation then we can use it to predict what would have happened with changes to parameters.


## Optimization


We have a simulation we feel mimics reality fairly well, now it's time to run some experiments. Code-wise, make sure to move some of the simulation parameters into variables you can change. The goal is to create a bunch of "what if?" scenarios. For example, "what happens if I increase the SLA?" or "what happens if I speed up the boot time of the standby instances?"


Once we can change parameters, we can use a technique from machine learning called hyperparameter optimization. Hyperparameter optimization is a trial-and-error technique that's typically used to adjust machine learning model parameters to get better results. The techniques vary from searching every combination (grid search) to bayesian probabilities to find likely better parameters.


I'm using the optimizer to play "what if?" with the simulator to search for the parameters that best reach the goal. In my case I wanted to search for parameters that decrease job start latency but keep costs the same or ideally better.


I wrapped the simulator in an optimization loop by exposing simulator parameters as CLI options. For each parameter set, the optimizer executes the simulator and then gets the result metric outputs like p99 latency, throughput, SLA violations, and cost. Those outputs are combined into an objective function capturing several competing constraints. The optimizer then tries new configurations and steers the simulation to parameters that produce better trade-offs.


To implement the hyperparameter optimization, I used the[optuna](https://optuna.org/) package. It's super, super easy to use out of the box but has many options to try if you so choose. Here's a simplified example showing how to use it:


```text
import   optuna


def   objective  (trial):
standby_boot_s   =   trial.suggest_int(  "standby_boot_s"  ,   10  ,   180  ,   step  =  10  )
pool_size        =   trial.suggest_int(  "pool_size"  ,   1000  ,   5000  ,   step  =  100  )


report   =   simulate(standby_boot_s, pool_size)


p99    =   report.latency_p99
cost   =   report.total_cost


# minimize both p99 and cost.
return   p99, cost


study   =   optuna.create_study(  directions  =  [  "minimize"  ,   "minimize"  ])
study.optimize(objective,   n_trials  =  50  )


for   t   in   study.best_trials:
print  (t.values, t.params)
```


The example has two parameters, standby boot seconds and pool size. It doesn't matter much what they do to the simulation, only that optuna will suggest a value between a range and by step. We tell optuna that we want to minimize the two return values from the objective, p99 and cost.


Note that this study returns a list of *best trials* , because I'm optimizing two competing metrics at once and there is no single winner. When you optimize for competing metrics, optuna returns the Pareto frontier. That's a fancy way of saying it returns all the configurations where you can't make the p99 lower without increasing cost.


## Results


The optimization of a simulation run took about 10 minutes each and we tested several thousand simulated parameter combinations. Thankfully, the optuna framework can run these in parallel. When studies surface the Pareto frontier, which highlights the tradeoffs that we can't optimize away, it makes it easier to reason about what's important and have those conversations as a team.


Our final best optimization decreased our p99 latency by two seconds and decreased the cost of the standby pool by 2%, so both faster and cheaper. Ultimately, the change was straightforward; we keep more in the standby pool in day and fewer in the night.


Building a simulation like this helps you formalize and test your assumptions. Getting counter-intuitive results is even more valuable than confirming what you already thought was true.


We took our simulation framework and reapplied it against what we are building now, Depot CI. In doing so we were able to simulate latencies of what our historical jobs would have been if we had run them in Depot CI. We could run experiments to see how best to schedule jobs using several different architectures.


A surprising result for us was how best to assign jobs to virtual machines. So, given many hosts with many different capacities, where should the job be run? The counterintuitive result for me was that the most efficient packing was to pick the first host in an ordered list that has enough capacity. My intuition was to pick the host with the least amount of capacity to run those "hot." Nevertheless, there was a pretty clear signal that "first-host" was better.


## What's next


Now that we've built and run our simulations, we can re-use them as part of our workflow when we make changes to the standby pool logic or other systems. Right-sizing standby pools still requires balancing latency against cost, but now we can quantify those tradeoffs instead of guessing.


I think it's well worth devoting some context to creating simulations and verifying them against historical data. That way when you do make changes you have some understanding of the potential impact. It's important to think of the simulator as a sharable tool. Your teammates, both carbon and silicon, can use it to iterate over all sorts of ideas and come up with very creative changes. Build it!


## FAQ


How does SimPy model queuing and resource contention?


SimPy uses a` Resource` to represent a pool of machines. When a job requests a machine via` machine_pool.request()` , it either gets one immediately or waits in the queue. Nothing is sleeping in real time, SimPy just advances simulated time when a process yields. The` with` block automatically releases the machine when the` with` block exits, so you don't have to manage cleanup manually.


How do you use optuna for infrastructure optimization instead of machine learning?


The same way you'd tune a neural net, but your "model" is a simulator. You expose system parameters as optuna trial suggestions (like` trial.suggest_int` ), run the simulator inside the objective function, and return the metrics you want to improve. We used multi-objective optimization to minimize both latency and cost at once, which lets optuna return a Pareto frontier rather than a single winner.


How do you know when a simulation is calibrated well enough to trust?


You don't get certainty, you get confidence. Feed real historical data into the simulator, compare its outputs against what actually happened in production, and track where it diverges. The gaps are fine as long as you document them and they don't undermine the conclusions you're drawing. Calibration is inherently qualitative and biased - being explicit about that is part of the process.


Can the same simulation framework be reused as the system evolves?


Yes, and that's one of the best reasons to build one. We originally built this simulator to tune our AWS standby pool, then reapplied it to simulate Depot CI job scheduling across different architectures. Once it exists, teammates can run experiments, test architectural changes, and get real signal before touching production. Think of it as a shared tool, not a one-time project.


## Related posts


- [Making EC2 boot time 8x faster](https://depot.dev/blog/faster-ec2-boot-time)
- [How we launch GitHub Actions runners in 5 seconds](https://depot.dev/blog/github-actions-breaking-five-second-barrier)
- [Building a custom Envoy discovery service for Depot](https://depot.dev/blog/depot-envoy-discovery-service)


Chris Goller


Principal Software Engineer at Depot
