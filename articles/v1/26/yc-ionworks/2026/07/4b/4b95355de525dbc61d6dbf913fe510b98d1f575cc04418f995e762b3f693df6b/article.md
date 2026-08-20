---
schema_version: "1.0.0"
document_id: "4b95355de525dbc61d6dbf913fe510b98d1f575cc04418f995e762b3f693df6b"
company_key: "yc-ionworks"
company: "Ionworks"
source_id: "yc-ionworks-news-import-f340ab61a31b"
canonical_url: "https://ionworks.com/blog/predict-battery-test-duration"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-28T19:09:12.329209+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:bf126a9ed75e018ebbd04afd20f5484026cafe4c3b143f51f0b78908b4701712"
---

# Predict how long a battery test will take

Scheduling a battery test is really a timing problem. Before you can put the next cell on a channel, you need two numbers: when the test currently running will finish, and how long the new one will take. Most labs write both on a whiteboard, then reshuffle when a test they penciled in for three weeks runs for five.


The second number is the hard one. Test duration is not a property of the cell or the channel. It is a property of the protocol running on that cell, and protocols vary enormously.


## Why test duration is hard to estimate


Consider three common protocols on the same NMC/graphite cell:


- A **rate-capability sweep** (C/5, C/2, 1C, 2C) finishes in a day or two.
- An **HPPC** with rests and pulse trains can run several days.
- A **1C/1C cycle-life** test to 80% capacity can run for months.


Even within one protocol, a fixed "cycles times time-per-cycle" estimate drifts. Voltage and current cutoffs end steps early or late depending on the state of the cell. CV holds depend on how quickly the current tapers. And as the cell ages, capacity fades and resistance rises, so the 900th cycle does not take the same wall-clock time as the first. By the end of a long test, the whiteboard number is off by days.


## How Ionworks Operate predicts it


Ionworks Operate predicts duration by simulating the protocol. When you attach a protocol to a test, Operate runs that exact schedule against the cell's parameterized model, an equivalent-circuit or physics-based model from the same Ionworks stack you use for analysis. The simulation produces the full voltage and current time series the cell would follow, step by step, including CV tapers, cutoffs, and rests. The end of that series is the predicted finish time.


Because the model carries degradation, the prediction holds across the life of a long test. Later cycles are simulated with the aged cell, so the estimate tracks the real test instead of drifting from a fixed per-cycle number.


## Time remaining, not just total duration


The same simulation answers the other question the scheduler needs: how much is left on a test already running. Operate knows the protocol, the start time, and the model, so it can place the running test on its predicted trajectory and read off the time remaining.


That turns the channel wall from a set of green "running" bars into a set of channels with known free-by times.


## How prediction feeds the schedule


Put the two together and scheduling becomes a prediction rather than a guess:


- The **next-available window** on a channel reflects when the running test actually finishes, not when someone guessed it would.
- A new test request gets a **realistic start and estimated end** , computed from its own simulated duration.
- Booking the next test stops meaning "overwrite a spreadsheet cell and hope."


The estimate is specific to this protocol on this cell, because it comes from simulating exactly that, not from an average across a folder of past tests.


## Where this runs


Duration prediction is part of[Ionworks Operate](https://ionworks.com/operate) , the scheduling and status layer for multi-brand cycler fleets. It sits on the same measurements and models as the rest of the Ionworks stack, so the numbers that schedule your lab are the numbers that model your cells. For the status side of the same wall, see[what's running on your cyclers](https://ionworks.com/blog/whats-running-on-your-cyclers) . For simulating a protocol before it touches hardware, see[simulate your test protocol before you run it](https://ionworks.com/blog/simulate-your-test-protocol-before-you-run-it) .


You can try it on a live synthetic lab, free.[Request access](https://ionworks.com/operate) with a work email and schedule a few tests to watch the predictions play out in real time.
