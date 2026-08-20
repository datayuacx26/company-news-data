---
schema_version: "1.0.0"
document_id: "4ba46c415644d3b28d733bb0ce0ad079f07d8a8fd0df484221870c7a1b1aeca6"
company_key: "ionq-inc-common-stock"
company: "IonQ Inc."
source_id: "ionq-inc-common-stock-news-import-8f7ead1ae5e6"
canonical_url: "https://www.ionq.com/blog/accelerating-towards-fault-tolerance-unlocking-99-99-two-qubit-gate"
published_at: "2025-10-21T00:00:00+00:00"
first_seen_at: "2026-07-22T00:33:28.377709+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:d67d7455f84c91f530e020ab44def1baac10f9aa835601373aed1a9e5dc14ad8"
---

# Accelerating Towards Fault Tolerance: Unlocking 99.99% Two-Qubit Gate Fidelities

Today, we released the[technical paper](https://arxiv.org/abs/2510.17286) demonstrating that IonQ is the first and only[quantum computing company](https://www.ionq.com/) to cross the ‘four nines’ threshold, demonstrating two-qubit gate fidelities of 99.99%. This is a landmark technical milestone that underpins IonQ’s accelerated roadmap to millions of qubits by 2030 – unlocking more complex applications for customers sooner and a thousands of time reduction in logical error rates.


IonQ has set the new world record in two-qubit gate fidelity and have also done so **without ground-state cooling,** which can be resource-intensive for large-scale systems. This allows us to operate with record-level performance whilst speeding up and simplifying the overall computation.


## Understanding the quantum computer clock speed


One of the most important metrics to describe a quantum computer is its operation error rate, typically limited by the ‘two-qubit gate error rate’. The error rate determines how many gates can be applied in each circuit, in turn upper-bounding the size of achievable quantum algorithms. In 2024, our team at Oxford Ionics (now an IonQ company) demonstrated[a two-qubit gate error rate of 3e-4](https://arxiv.org/abs/2407.07694) – shattering the previous world record and opening the door to run more complex quantum algorithms.


Once the quantum computer has sufficiently low error rates to implement a given quantum algorithm, the next question becomes: how quickly can it do that? There are many metrics that affect this so-called “time-to-solution”: from low-level quantum computing characteristics such as gate speed and error rate, through mid-level system properties like qubit connectivity, all the way to the latency and throughput of the classical control system.


[To date, trapped ion quantum computers have led the way regarding time-to-solution](https://arxiv.org/abs/1702.01852) , largely thanks to their flexible, long-range qubit connectivity, and low error rates. However, relatively little progress has been made in recent years on the speed of the primitive operations – the quantum computer’s "clock speed". We thus identified a critical opportunity – significant improvements in the clock speed would directly cut the time-to-solution as well, extending our lead for near-term applications and alleviating runtime challenges of future fault-tolerant algorithms.


## The traditional bottleneck to speed: ion cooling


In trapped ion quantum computing, there is an operation that takes place under the hood that has historically eaten up most of the runtime, acting as a bottleneck to speed: ion cooling.


Suppose a user requires a trapped ion quantum computer with 100 qubits to implement a two-qubit gate between qubits 1 and 2, and then between qubits 1 and 100. To the user, these two gates look the same – but beneath the surface, two very different operations take place.


Leveraging Electronic Qubit Control, the first two-qubit gate between neighbor qubits 1 and 2 is implemented as a simple radio frequency (RF) pulse. However, to implement the gate between distant qubits 1 and 100, these qubits must first be moved to the same physical location on the chip. This is accomplished electrically following a principle similar to how standard charge-coupled device (CCD) imaging chips move electrons around - we thus call it the ‘quantum CCD’ or the[‘QCCD’ architecture](https://tf.nist.gov/general/pdf/1275.pdf) .


The QCCD architecture allows for real-time adjustments to the qubit connectivity graph, which is typically fixed in solid-state quantum computers. This architecture makes trapped ions a very powerful quantum computing platform, as the flexible connectivity dramatically reduces the gate count compared to fixed connectivity.


The downside is that, historically, every update of the connectivity graph moves the ions around, which makes them hotter. For qubit error rates to remain stable, they thus need to be periodically recooled. So, the standard computation in the QCCD architecture follows the pattern of: gates → ion movement → ion cooling → gates → ion movement → ion cooling → and so on.


Therefore, before our demonstration, the vast majority of time in trapped ion quantum computers was taken up by ion movement and ion cooling. For example,[in one of the seminal demonstrations of QCCD-based quantum computing](https://www.nature.com/articles/s41586-021-03318-4) , quantum gate pulses only took ~2% of system runtime, ~27% of system runtime was spent on ion movement, with a whopping ~68% of time spent dedicated to cooling.


One might conclude from this result that ion movement and ion cooling are both slow, but the truth is more subtle. Actually, ions can be moved around at extremely high speeds, but most of the time their movement is deliberately slowed to keep the ions cold! So ultimately, **slow ion transport is primarily caused by slow ion cooling.**


## Why the real hurdle is ‘ground-state cooling’


Given all this background, it’s apparent that speeding up ion cooling would yield tremendous advantages. And this is indeed possible – we can speed up ion cooling if we can increase the ion temperature by a fraction of a millikelvin.


As many will know, one of the largest advantages of trapped ion quantum computers is that, unlike solid-state alternatives, they don’t need to be held at millikelvin (mK) temperatures. However, the trapped ions themselves **do** like to be kept cold; it's just that since the qubits are floating above a chip (rather than embedded in it), it's possible to cool them without having to cool the chip at the same time. And since the ions are ultra-light, this can be achieved through the fastest cooling method ever invented: **laser cooling** .


Laser cooling efficiently cools hot ions (through so-called “Doppler cooling”) down until it hits a wall at a few hundred uK – the so-called ‘Doppler limit’. However, beyond this limit, a different cooling technique is required for the last fraction of a millikelvin: **“ground-state cooling”.** And unfortunately, the methods available for this ‘second-stage cooling’ tend to be many orders of magnitude slower. Thus, a ‘cooling speed’ vs. ‘temperature graph’ may end up looking something like this:


Illustrative plot of the relationship between ion temperature (x-axis, linear) and cooling speed (y-axis, logarithmic). The Doppler cooling speed is generally orders of magnitude larger than the ground-state cooling rate, and increases with ion temperature, as long as it's above the Doppler limit. However, below the Doppler limit, Doppler cooling is no longer effective, leading to a dramatic reduction in the cooling speed. As a result, the total cooling time is significantly increased if the ions need to be cooled below the Doppler limit.


In summary, ion cooling speed has historically been severely bottlenecked by this “last mile cooling” – reducing the temperature below the Doppler limit and close to the absolute lowest ground state. This bottleneck is well-known in the trapped ion quantum computing community, and has been a strong motivator for research into faster ground-state cooling techniques like Electromagnetically-Induced-Transparency (EIT) cooling or polarization gradient cooling.


But at IonQ, we decided to take a different route altogether by asking: “What if we could avoid ground-state cooling entirely?”


## High-fidelity quantum gates above the Doppler limit


Ten years ago, the founders of Oxford Ionics (now an IonQ company) were able to[perform the world’s highest-fidelity single-qubit gates using all-electronic control above the Doppler limit](https://arxiv.org/pdf/1403.1524) . The two-qubit gate demonstration, however, remained elusive before today.


The issue was that, while electronic two-qubit gates were in principle highly temperature-insensitive, there were still a few second-order effects that increased the errors for warmer qubits. Because of this, the world-record for two-qubit gates in[Oxford Ionics’ 2024 paper](https://arxiv.org/abs/2407.07694) included ground-state cooling, as have all previous two-qubit gate fidelity records. For the technical audience, in geometric phase gates, non-zero ion temperature acts as an amplifier of residual spin-motion entanglement, increasing the gate error associated with the gate mode frequency fluctuations and cross-Kerr couplings.


We have made a breakthrough on this problem by developing a highly efficient and robust coherent control technique to combat thermally-sensitive errors in two-qubit gates. And as made public today, we were able to prove it in practice by performing ***two-qubit gates above the Doppler limit with an estimated fidelity of 99.99%.*** The full discussion of how our improved two-qubit gates work is outlined in[this technical paper](https://arxiv.org/abs/2510.17286) which we released to arXiv earlier today.


Subspace Leakage Error Randomized Benchmarking showing the application of up to 500 sequential Cliffords (1,083 two-qubit gates) to a qubit pair. In the absence of errors, the qubits remain in the initial state (P_survival =1). The plot illustrates that, for ~1,000 two-qubit gates, qubits remain in the initial state with ~90% probability, indicating an error rate of ~0.01%. See:[https://arxiv.org/abs/2510.09508](https://arxiv.org/abs/2510.09508)


Illustrative plot of the relationship between ion temperature (x-axis, linear) and cooling speed (y-axis, logarithmic), showing how increasing the required gate temperature by a fraction of a Kelvin to above the Doppler limit drastically reduces the amount of slow cooling required, which therefore significantly decreases the total cooling time.


In summary, IonQ has now achieved world-record two-qubit gate fidelities of 99.99% above the Doppler limit. This removes a critical legacy bottleneck to speed, opening up the possibility of order-of-magnitude quantum computing speedup, and making our quantum computers significantly simpler and more robust. With record-level performance, fewer physical qubits are needed to build large-scale fault-tolerant devices – empowering IonQ to deliver smaller devices the size of standard server racks as well as requiring far less energy.


Ultimately, IonQ’s breakthrough marks a defining moment for the quantum computing industry: world-record qubit performance on mass-manufacturable chips built in standard semiconductor fabs. By engineering 99.99% fidelity on devices that are simpler, more stable, and more robust, we have now reached a critical inflection point on our path to scaling millions of qubits by 2030 –[ushering us ever closer to achieving large-scale quantum advantage.](https://ionq.com/blog/ionqs-accelerated-roadmap-turning-quantum-ambition-into-reality)
