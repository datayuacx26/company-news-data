---
schema_version: "1.0.0"
document_id: "e1af7a00588d11f5de06c2b9753ff908af8b01cb2c415dd22b48f70330fb439d"
company_key: "yc-navier-ai"
company: "Navier AI"
source_id: "yc-navier-ai-news-import-c904ef9ba107"
canonical_url: "https://www.navier.ai/blog/2026-03-22-prompt-to-flight-software-stella"
published_at: "2026-03-22T00:00:00+00:00"
first_seen_at: "2026-07-24T12:10:55.994382+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:4fbc6428241e3215d0782879b761d3e5e90e65b0d394a184365b1e667a687bbe"
---

# From Prompt to Flight Software: Building a Missile Evasion Simulation with Stella

Suppose you need to simulate an air-to-air engagement: a F-16 detects an incoming IR-guided missile and executes a high-G defensive break turn to defeat it. You know the scenario parameters and you need a full 6-DOF simulation that runs both vehicles simultaneously, computes miss distance at closest approach, and produces a verified result.


Today, the path from scenario concept to working simulation code runs through hours or days of MATLAB prototyping, C++ translation, painful integration testing, and the quiet dread of discovering your engagement model doesn't quite match your flight dynamics. The result is that teams can't easily experiment with different evasion strategies, threat geometries, or countermeasure timelines, let alone explore the full trade space of what's survivable.


Stella, our agentic GNC platform, eliminates those constraints. You describe the engagement. Stella generates the simulation, the flight software for both vehicles, the guidance laws, the autopilots, and runs Software-in-the-Loop before you've touched a compiler. The result is up to a 50x speedup over traditional tooling, letting you test dozens of engagement scenarios in the time it used to take to set up one.


This post walks through how that actually works, end to end, for a missile evasion scenario. But first, a question that comes up constantly: why can't you just do this with Claude or ChatGPT?


## Why Base LLMs Fall Short


Large language models can write code. They can explain flight dynamics. They can even produce something that looks like a 6-DOF engagement simulation if you prompt them carefully. But looking right and being right are very different things in flight mechanics, and the failure modes are subtle enough to be dangerous.


### Coordinate Transformations


Flight dynamics work is fundamentally a coordinate transformation problem. You're constantly moving between Earth-centered inertial, body-fixed, wind, stability, and navigation frames. Every transformation has to be consistent, right-handed, and correctly sequenced. Base LLMs routinely botch this. They'll mix up the rotation order in a DCM. They'll confuse the direction of an NED-to-body transformation. They'll silently apply a quaternion convention that's inconsistent with the rest of the codebase. The output compiles. The aircraft banks the wrong way. The missile guides into the ground.


This isn't a prompting problem you can engineer around. Frame transformations require global consistency across every module in the system. A base LLM generating code one function at a time has no mechanism to enforce that consistency.


Rust


```text
/// Compute aerodynamic forces on the F-16 in body frame.
/// Called at 100 Hz by the flight software loop.
fn compute_aero_forces(
v_ned: [f64; 3],       // NED velocity from nav filter [m/s]
attitude: [f64; 4],    // quaternion [w, x, y, z] body-to-NED
altitude: f64,         // [m]
) -> [f64; 3] {
let s_ref = 27.87;     // F-16 wing area [m²]
let cl_alpha = 3.44;   // lift curve slope [/rad]
let cd0 = 0.0175;      // zero-lift drag
let k = 0.12;          // induced drag factor


// Atmosphere (exponential model)
let rho = 1.225 * (-altitude / 8500.0_f64).exp();


// Rotate NED velocity into body frame
let v_body = quat_rotate(attitude, v_ned);


let vt = (v_body[0]*v_body[0] + v_body[1]*v_body[1] +
v_body[2]*v_body[2]).sqrt();


let alpha = (v_body[2] / v_body[0]).atan();  // angle of attack
let qbar = 0.5 * rho * vt * vt;


// Force coefficients
let cl = cl_alpha * alpha;
let cd = cd0 + k * cl * cl;


// Forces in body frame
let lift = qbar * s_ref * cl;
let drag = qbar * s_ref * cd;


[
-drag,   // axial (body X, forward)
0.0,    // side force (body Y)
-lift,   // normal (body Z, down)
]
}


```


Claude generated aero force model for a F-16 break turn. The quaternion convention is inverted and lift is resolved along the wrong axis.


### Physically Realistic Plant Models


Ask a base LLM to build a vehicle dynamics model and you'll get something. It will have gravity. It might have drag. But the foundational physics that the entire simulation depends on, such as 6-DOF rigid body propagation or coupling between translational and rotational states, often either get omitted entirely or implemented with subtle errors that propagate silently through every downstream result.


This is where base LLMs are most dangerous, because the math looks right at first glance. An LLM will write a quaternion integration step that doesn't renormalize. It will implement a gravity model that doesn't account for the vehicle's attitude when resolving force components. The simulation runs. The numbers come out. They're just not physically correct.


### Autonomous Iteration


The deepest problem is that engagement simulation development is iterative. You don't write a guidance law, test it once, and ship it. You run Monte Carlo campaigns. You discover that the evasion maneuver timing is too late at certain launch ranges. You find that the missile's lateral acceleration saturates at 30 G but only needs 28 G to track through your break turn. You redesign, retest, and repeat.


Base LLMs don't do this. They can't look at a Monte Carlo spread and decide that the 3-sigma miss distance falls inside the warhead lethal radius for 15% of cases and that the fix is to increase bank angle or add a barrel roll at closest approach. Every iteration requires a human in the loop reinterpreting results and re-prompting. The "autonomous" part of autonomous engineering doesn't exist.


## Walking Through a Missile Evasion Scenario with Stella


Let's walk through how Stella actually builds a missile evasion simulation from a single prompt, and more importantly, what's happening under the hood at each stage.


### The Prompt


We started with a natural-language scenario description. Here's what we asked Stella to do:


> Set up a simulation of a missile evasion scenario. We have a f16 evading a sidewinder-like missile with the goal of the f16 having GNC code to evade the missile. Both the missile and the f16 should have simulated flight software. Let's have the f16 do something like a defensive break turn.


Alternatively, you can point Stella to a mission description in a .txt file. The agent takes it from there.


### The Foundation: StellaRust


Everything Stella generates is built on top of StellaRust, a Rust-native numerical framework that our team wrote by hand. This is the critical distinction from what a base LLM does. The foundational flight dynamics math, such as 6-DOF propagation, state-space methods, integrators, coordinate transformations, aerodynamic models, actuator models, lives in a validated, human-authored library. It was deliberately not left to AI to write.


This is the layer where the failure modes we described earlier would be fatal. Coordinate transformations, quaternion conventions, integrator numerics, frame definitions: these have to be correct once and reused everywhere. StellaRust provides that single source of truth. When the agent generates code for this missile evasion scenario, it isn't inventing a proportional navigation law from scratch or writing its own RK4 integrator. It's composing calls into a framework where those primitives have already been built and validated.


The framework also provides a SIM harness architecture that separates flight software from simulation truth. On one side sits the actual flight code: the F-16's break-turn autopilot, the missile's PN guidance law. On the other side sit truth models for the environment, aerodynamics, and propulsion, with configurable fidelity. The flight software is navigating and controlling against sensor measurements that behave like real hardware, not feeding on its own truth state.


Because it's all Rust, there's no translation step. The flight software code that runs inside the SIM harness is the same code that compiles to an embedded flight processor. We've already flashed embedded hardware to prove this path works end to end.


Compare that to the traditional Simulink workflow: design in blocks, export to auto-generated C that nobody can read, then have your flight software team translate it into something maintainable. That translation is where many bugs live. Stella eliminates that step entirely.


### The Agent: From Prompt to Code


The Navier GNC agent layer sits alongside the StellaRust source code in Prism, our desktop application. The UX is similar to a coding agent like Claude Code or Cursor, but with deep context about the flight dynamics domain and the full StellaRust API. When you submit a prompt, the agent doesn't just generate generic code. It reads your scenario parameters, decides which StellaRust modules to pull in, and writes the full simulation and flight software from scratch.


For this missile evasion scenario, that means the agent has to make real engineering decisions. Running two vehicles simultaneously at different update rates (1 kHz physics, 100 Hz flight software) requires a carefully structured sim harness with synchronized time stepping. The F-16's autopilot needs a three-phase architecture: cruise at level flight with 40% thrust until threat detection, immediate hard right break at 80° bank with max angle of attack and 95% thrust for the evasion window, then wings-level extend to accelerate away. The aerodynamic force model uses a linear lift curve with a parabolic drag polar, exponential atmosphere, and a 9 G structural limit.


On the threat side, the AIM-9's augmented proportional navigation needs two distinct flight phases: boost with thrust-vector control steered toward the PN guidance command, then terminal coast or partial thrust with aerodynamic fin steering. The missile's 30 G lateral acceleration limit is the parameter the evasion maneuver is designed to exploit, as the break turn's angular crossing rate is specifically chosen to force the seeker beyond this limit.


The agent writes all of this as Rust code that calls into StellaRust. The guidance laws use the framework's propagation and steering tools. The autopilots use the framework's actuator models with proper aerodynamic geometry. Every module references the same coordinate frame definitions, the same quaternion conventions, and the same time system because they're all built on the same foundation.


The Navier GNC agent generating the plan for a full missile evasion simulation from a single prompt inside Prism.


### The Iteration Loop: Where It Gets Interesting


Here's what separates Stella from a coding agent that happens to know some flight mechanics. The agent doesn't generate code, hand it back, and wait for you to tell it what's wrong. It runs the simulation, analyzes the results, identifies problems, and fixes them autonomously.


For a missile evasion scenario, the first version of the code is almost certainly not the final version. Maybe the F-16's autopilot can't achieve the commanded bank angle fast enough and the break turn initiates too slowly to generate sufficient crossing rate. Maybe the missile's PN gain is tuned too aggressively and the guidance oscillates in the terminal phase, artificially inflating miss distance. Maybe the aerodynamic model's lift curve saturates at high AoA and the aircraft can't sustain 7.5 G at the specified altitude and Mach number.


In a traditional workflow, an engineer would run the sim, stare at the plots, diagnose the problem, and go fix it. Then do it again. And again. Stella's agent does this loop autonomously. It runs the simulation, parses the output, compares results against the scenario requirements in the prompt, traces violations to root causes, modifies the design, and re-runs. If the autopilot can't track the commanded bank angle, it can adjust gains or try different control methodologies. If the evasion geometry doesn't generate enough miss distance, it can modify the break timing or bank angle. Each iteration is done autonomously in minutes, not hours.


The agent diagnosed the original F-16 model it generated had no aerodynamic forces. It then worked to correct the error until it verified that the break turn is physically accurate.


## What You Get


When Stella finishes, the output is a complete Rust codebase: F-16 autopilot and missile guidance flight software running inside a full two-vehicle simulation environment. Not MATLAB prototypes. Not pseudocode. Not auto-generated C from a block diagram. Readable, maintainable Rust code with a clean architecture:


```text
src/
main.rs          # Scenario setup, sim harness, results analysis, output
vehicles.rs      # Vehicle parameters (mass, inertia, thrust, initial states)
f16_fsw.rs       # F-16 flight software: aerodynamic force model + break-turn autopilot
missile_fsw.rs   # AIM-9 flight software: augmented proportional navigation guidance
gen_charts.py    # Post-processing: engagement geometry and telemetry charts


```


The simulation runs both vehicles simultaneously at 1 kHz physics / 100 Hz flight software, computes miss distance at closest point of approach, and writes a bundle for 3D visualization.


3D visualization of the missile evasion engagement generated by Stella.


The agent can also generate structured engineering documentation directly: engagement analysis reports, survivability assessments, and trade study summaries formatted as proper deliverables with title blocks, revision fields, and chapter organization. These take the agent minutes to compile versus the days an engineer typically spends assembling them by hand.


Full algorithm description document generated by Stella, featuring visulizations and commentary.


## Why This Matters


The missile evasion scenario above took just under **40 minutes** to go from prompt to verified simulation. That speed changes what's possible.


Run the evasion with a 7.5 G break turn. Then run it again at 5 G and find exactly where survivability degrades. Try initiating the break at 2.5 km threat range instead of 3 km and quantify the sensitivity. Swap the AIM-9 for a longer-range semi-active threat with different guidance dynamics and see whether the same evasion maneuver works. Increase the missile's lateral acceleration limit from 30 G to 40 G and find out where the break turn stops being effective. Add chaff or flare deployment at a specific time and model the seeker's response. Each of these is a full 6-DOF simulation with flight software, not a back-of-the-envelope estimate.


This is how you find out that the 80° bank angle is actually the sweet spot, or that initiation timing matters more than G-loading for survivability, or that the evasion works against the AIM-9's 30 G limit but fails if the threat is upgraded to 35 G. You find it in simulation, in an afternoon, instead of discovering it in flight test six months from now.


The translation gap matters too. The Rust code Stella generates in the SIM harness is the same code that compiles to an embedded flight processor. There's no Simulink-to-C export step, no manual reimplementation, no praying that the flight software matches the model. What is designed, tested, and reviewed is exactly what flies.


But the real shift is what your engineering team spends their time on. Instead of hand-coding the same proportional navigation law for the tenth time or debugging a frame transformation that got flipped during reimplementation, they're evaluating engagement geometries, interrogating evasion trade studies, and making tactical decisions backed by simulation evidence. Stella handles the implementation. Your engineers do the engineering.


**Want to see Stella in action on your GNC use cases?**


[Get in touch with our team](https://navier.ai/contact) to schedule a demo today.
