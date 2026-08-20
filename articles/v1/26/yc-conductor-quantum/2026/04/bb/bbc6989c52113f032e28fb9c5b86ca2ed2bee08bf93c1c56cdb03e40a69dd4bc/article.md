---
schema_version: "1.0.0"
document_id: "bbc6989c52113f032e28fb9c5b86ca2ed2bee08bf93c1c56cdb03e40a69dd4bc"
company_key: "yc-conductor-quantum"
company: "Conductor Quantum"
source_id: "yc-conductor-quantum-rss-fd18baa21fa7"
canonical_url: "https://blog.conductorquantum.com/p/128-double-quantum-dots"
published_at: "2026-04-08T20:18:59+00:00"
first_seen_at: "2026-07-24T23:15:28.367668+00:00"
fetched_at: "2026-07-28T20:52:48.201959+00:00"
content_hash: "sha256:d169c5b4b778296e5f454f210cd2cd188bf14bd2b715aea3629831ce21146988"
---

# Conducting 64 devices: what it actually takes to turn on a quantum chip

# Conducting 64 devices: what it actually takes to turn on a quantum chip


### Inside our latest milestone and why tuning up a quantum computer feels like preparing an orchestra.


Apr 08, 2026


---


Conductor Quantum builds AI software to scale and operate quantum technologies. Before a silicon quantum chip can run a computation, every device on it has to be located, characterised, and tuned. That process has until now required days of expert manual work per device. We automate it.


This post accompanies a video introducing our team and the problem we are working on. It also covers what our software can do: automated tuning of 128 double quantum dots across 64 devices on a multiplexed silicon chip made by our partner


[SemiQon](https://semiqon.tech/) , run from scratch, without a researcher at the controls. To understand why that matters, it helps to understand what a quantum computer actually is and why turning one on is so hard.


## Why quantum computers are different


Classical computers store information as bits. Each bit is either a 0 or a 1, like a light switch: on or off. This binary logic is the foundation of every laptop, phone, and data centre on the planet.


Quantum computers use quantum bits, or qubits. A qubit can hold a value of 0 or 1, but it can also hold a linear combination of both at the same time. This is superposition, a phenomenon with no classical analogue. A qubit in superposition is not undecided between two states. It occupies both, until it is measured and collapses to one. Two qubits can also become entangled, meaning the state of one is bound to the state of the other regardless of the distance between them. These two properties together give quantum computers a fundamentally different relationship with information.


Quantum computing is not a speed improvement. It is access to a different class of problems. Simulating the molecular interactions of penicillin, for example, would take every supercomputer on Earth working in concert longer than the age of the universe. A fault-tolerant quantum computer could in theory do it in a reasonable time. The same applies to the discovery of new materials, drugs, and chemical processes governed by quantum mechanics at the atomic scale. These problems are not hard because our classical computers are slow. They are hard because the tool we use to describe nature at the fundamental level is quantum mechanics, and a quantum computer is the natural machine for that language.


Access to a different class of problems is supported by exponential scaling. N qubits can represent 2 to the power of N states. Three qubits give you eight states. Fifty give you over a quadrillion.


But today’s qubits are noisy. They decohere before a computation finishes, and the operations performed on them are imperfect. To get reliable results, you need error correction: encoding one logical qubit across many imperfect physical ones. That means a useful quantum computer will need not hundreds of qubits, but millions.


Silicon quantum chips are built using the same fabrication processes that put a billion transistors in your phone. They are nanoscale, dense, and manufactured in structures the semiconductor industry already runs at volume. That path to scale is why we targeted silicon first. Every chip that comes out of a fab needs to be tuned before it can do anything, and that tuning does not scale by hand. It is the bottleneck we set out to break.


## The fragile part is not just the chip


When quantum chips are described as fragile, we mean three distinct things.


First, the chips are vulnerable to electrostatic discharge. Static electricity, the kind you generate walking across a carpet, can destroy a chip before a single measurement is taken. There are stories in the research community of teams losing a run of chips to electrostatic discharge before they ever got to connect a wire. This is a handling problem, solved with discipline and proper equipment.


Second, the qubits and corresponding quantum dots (more on these later) drift. The voltages needed to control them change over time, as temperature shifts and charges in the material move around. A configuration that worked yesterday may not work today. The voltages that worked for calibrating one quantum dot, may not work on another quantum dot on the same chip. This is a calibration problem, and it is the one our software is built to solve.


Third, and most fundamentally, the qubit state itself is fragile. A qubit has a lifetime. A qubit has a limited lifetime due to its interactions with the surrounding environment. After a certain period, it decoheres, loses its quantum properties, and collapses into a classical state. This is the deepest challenge in the field, and it brings us to Schrödinger’s cat.


## The paradox at the heart of quantum computing


In 1935, the physicist Erwin Schrödinger proposed a thought experiment. A cat is placed in a sealed box, along with a small amount of radioactive material. If a single atom decays, a mechanism triggers and the cat dies. Quantum mechanics says the atom is in a superposition of decayed and not-decayed until it is observed. So what is the cat? Until the box is opened, it is, in a strict quantum mechanical sense, both alive and dead at once.


Schrödinger meant this as a provocation, a reduction to the absurd of what quantum superposition implied at a macroscopic scale. For quantum computing, the thought experiment maps onto something real.


A qubit in superposition is the cat. It must be isolated from its environment, sealed in its box, to preserve the quantum state. Any interaction with the outside world, any stray electromagnetic field, any vibration, any thermal fluctuation, acts like opening the box. The qubit collapses. The state is destroyed.


Here is the contradiction that makes quantum computing hard: To run a computation, you need to manipulate the qubit, gate it, entangle it with its neighbours. Ideally you have perfect control of the qubit, but how can you do this if you also want the qubit to be perfectly isolated from the outside world.


Perfect isolation preserves the quantum state but makes computation impossible. Perfect control enables computation but opens the door to destroying the quantum state. Every quantum computing architecture is an attempt to live in the narrow space between those two extremes. You need to be isolated enough that environmental noise does not kill the qubit before the computation finishes, and controlled enough to perform the operations you need before the clock runs out.


It is not a problem that gets solved once. It gets managed at every layer of the stack: in the physical design of the qubit, in the cryogenic systems that reduce thermal noise, and in the software that tunes and recalibrates the system before every use.


## What is a quantum dot?


We have built software to control quantum dots. A quantum dot is a nanoscale structure, around 100 nanometres across, engineered to trap individual charge carriers. Those carriers are typically electrons which hold a negative charge (or holes - the absence of an electron which carries a positive charge). The key property is confinement: the structure is small enough that electrons inside it behave like they are in an atom. They do not occupy a continuous range of energies. They occupy discrete “quantized” levels, stacked like rungs on a ladder. Each electron sits at a higher energy than the one below it, following the same rules of atomic physics that govern electrons in real atoms. This is why quantum dots are sometimes called artificial atoms, and the zero-dimensional density of states is the “dot” in the name.


By adjusting the voltages on metal gate electrodes, for example a barrier gate or a plunger gate, surrounding the quantum dot, we can control the number of electrons inside, one by one, all the way down to a single electron. That single electron has a property called spin: it points either up or down. That spin is the qubit.


A double quantum dot places two of these structures side by side. When tuned correctly, an electron can tunnel between the two dots. The foundation for reading and controlling a spin qubit is in that tunnelling event. Each of our 64 devices contains two separate double quantum dots. Forming those pairs of dots, is what we did and announced today, across 64 devices.


## Turning on a quantum computer


Turning on a quantum computer is nothing like booting a laptop. There is no power button. There is a process, one that until software like ours existed required days of expert manual work per device.


We ran that process from scratch across every one of the 64 devices on the chip. It begins with a


[health check](https://docs.conductorquantum.com/control/stanza/core-concepts/routines/health-check) : a source-drain voltage sweep to confirm current flows as expected, a gate leakage test to verify the gates are not shorted, a global turn-on check to confirm the device responds to gate voltage, and a pinch-off sweep for each gate to verify it can cut off current flow between source and drain. These steps tell you whether a device is worth tuning. If any fail, the device is set aside.


From there, the real work begins. Puddles of electrons must be formed at the right locations in the device. These become the quantum dots. The voltages controlling each dot must be swept and mapped to produce a charge stability diagram: a fingerprint that shows how many electrons occupy the dot and how sensitive it is to changes in voltage. The target is a regime where electron occupancy can be controlled with precision and the two dots in each pair are coupled, forming a double quantum dot.


The process is iterative: adjust, measure, adjust again. Every chip is different. Device variability means the voltages that work on one device are a poor starting point for the next. The parameter space is vast. Doing it by hand, across 64 devices, is not a realistic option.


## 128 double quantum dots: what we achieved


Our automated tuning software completed the full tuning sequence across all 64 devices on a SemiQon chip. The routine goes from health check through to double quantum dot formation for both electrons and holes, without a researcher at the controls. Each device contains a pair of double quantum dots, giving 128 in total. Each one was checked, characterised, and tuned from scratch in less than an hour on average.


The


[DQD (double quantum dot) search algorithm](https://docs.conductorquantum.com/control/stanza/core-concepts/routines/dqd-search) divides the voltage space into a grid and works through each square in three stages, each one more detailed than the last. First, a fast voltage sweep screens for electrical activity worth investigating. Squares in the grid that pass get a low-resolution charge stability diagram. Squares that pass that get a high-resolution scan for final confirmation. Each stage uses a machine learning model to automatically analyse the data acquired from the device in real-time. Finding the right voltage parameters to form a double quantum dot is like finding a needle in a haystack. The search space is vast and most regions will not contain what we are looking for. We cannot afford to waste time on false positives, which is why accurate classifiers at each stage matter so much. A square that fails at stage one costs 128 measurements. A square that makes it to stage three costs over 2,000.


The algorithm also learns where to look. Confirmed double quantum dot regions tend to cluster on a chip, so the search prioritises unvisited squares near previous successes before returning to a more explorative approach.


Forming a double quantum dot is the prerequisite for everything that follows: spin readout, qubit control, entanglement.


A useful quantum computer will need millions of qubits. Scaling to that number requires this kind of automation at every layer. There is no version of that future where researchers tune devices one at a time. Automation is not a convenience. It is the only path forward.


To learn more about DQD search and how to implement it on your own devices, see our


[documentation](https://docs.conductorquantum.com/control/stanza/core-concepts/routines/dqd-search) .


## Why the name Conductor


We chose the name “Conductor” because we wanted something to outlast the moment, but specific enough to mean something in the industry. We knew we were going to initially start building for semiconductor spin qubits. We noticed that both “semiconductor” and “superconducting,” two qubit platforms, share the root “conduct.” People in the field were saying our name without knowing it.


The deeper reason was the metaphor. What our software does is calibrate arrays of qubits, coordinating their behaviour, bringing them into alignment before a computation. Our software also turns natural language into a working quantum circuit or hybrid quantum-classical workflow that you can run on calibrated qubits, allowing you to conduct a new computational paradigm with a few keystrokes. This maps onto what a conductor does before a performance. You are not playing the instruments. You are listening to each one, adjusting, coordinating, until the whole ensemble is in tune and the symphony is delivered.


Watching a conductor work with an orchestra, you understand that the conductor is not decorative. The conductor is the reason the performance works.


We are conducting an orchestra of qubits. The name was never going to be anything else.


---


*Conductor Quantum is building quantum superintelligence. We are an American company headquartered in San Francisco, California. We are assembling a highly leveraged team of hardcore engineers whose sole focus is to develop quantum superintelligence.*


*If you would like to solve one of the hardest technological challenges of our time, this is your chance. [Join us](https://www.conductorquantum.com/team)* .
