---
schema_version: "1.0.0"
document_id: "7a68e7fb540a41daea37ab4bdc2732b668c7e6349bdcac64b3111b8afc12a554"
company_key: "ionq-inc-common-stock"
company: "IonQ Inc."
source_id: "ionq-inc-common-stock-news-import-8f7ead1ae5e6"
canonical_url: "https://www.ionq.com/blog/reimagining-error-correction-for-modular-quantum-computing"
published_at: "2025-11-17T00:00:00+00:00"
first_seen_at: "2026-07-22T00:33:28.377709+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:636cc3e9c0541e2d324ce210ae05c4a2949eead05bc7163410cd3f92e3ba1c66"
---

# Reimagining Error Correction for Modular Quantum Computing

While fault-tolerant quantum computing remains the industry’s long-term destination, IonQ believes there is already tremendous value to be gained today in the pre-fault-tolerant, or NISQ, era. Our systems are already tackling high-impact problems, such as energy optimization, materials discovery, and advanced machine learning. At the same time, IonQ’s new roadmap provides the fastest path to fault-tolerance, leveraging our trapped-ion technology to scale performance while reducing error rates. This dual approach ensures that customers can benefit from quantum computing now, while gaining a clear line of sight to the fault-tolerant future.


To achieve fault-tolerant design, we use Quantum Error Correction, or QEC, which acts like a shield, protecting the delicate quantum information from potential errors. It does this even when the physical qubits themselves experience problems.


Making these error-correction codes as efficient as possible is key to building larger, more powerful quantum computers, and there are different approaches to their layout that are particularly advantageous with trapped ion quantum computers.


## Efficient Error-Correction Codes


The way you arrange a quantum error-correction code and its syndrome extraction circuits on the actual hardware greatly affects how well it performs. A poorly planned layout can slow down calculations, make them more complex, and ultimately lead to worse results.


Different QEC codes have different underlying structures, which we can visualize using something called a Tanner graph. Think of a Tanner graph as a map showing the ”connection patterns” within the code. One common QEC code is the surface code. This code has Tanner graphs that are relatively simple and fit neatly onto a flat, 2D grid (see figure below). This allows qubits to interact only with their immediate neighbors, making the surface code easy to arrange on physical hardware (like superconducting quantum computers).


However, the surface code isn't very efficient. To protect just one piece of quantum information (one “logical qubit”), you need to use a very large number of physical qubits. For example, if you want a certain level of protection (measured as the code’s “distance”), the number of physical qubits you need grows quickly—for that one piece of information, you'd need 100 physical qubits for a distance of 10.


Fortunately, there's a broader category of codes called quantum low-density parity-check (qLDPC) codes, which are much more efficient. A great recent example is the Bivariate-Bicycle (BB) codes. These codes can protect *many* pieces of quantum information using *fewer* physical qubits for the same distance. For instance, a specific BB code can protect eight logical qubits using only 90 physical ones, achieving the same protection level of ten. That’s a dramatic improvement in error correction compared to the surface code, which needed 100 physical qubits for just *one* logical qubit with the same protection.


This efficiency comes with a trade-off: qLDPC codes, including BB codes, have much more complex Tanner graphs and connection patterns that don't fit neatly onto a simple 2D grid. This makes designing a good layout for them quite challenging, as we'll explore next.


## Challenges of a Modular Layout


One key requirement for building large and powerful quantum computers is modularity—this means putting it together from smaller, standardized parts. Modularity makes it easier to build the computers consistently, check that they're working correctly, and allows for gate parallelism, which means running multiple quantum operations at the same time to speed things up. It's a crucial approach that works together with fault-tolerance to make quantum computers reliable and economically viable.


However, efficient error-correction codes like qLDPC codes present a challenge for modular design. Their Tanner graph often has something called a large edge-expansion property. Imagine you've divided the qubits of a quantum code into different modules. Edge-expansion is a way to measure how many connections and operations are needed *between* those modules. A *large* edge-expansion means that if you divide the code, a *lot* of connections have to cross the boundaries between the modules.


**Figure 1:** Comparison of Tanner graphs for the surface code (left) and a bivariate-bicycle or BB code (right), which is an example of a qLDPC code. Dashed lines depict distributing the code into two separate modules.


This creates a challenge because performing operations *between* different physical modules in a quantum computer is generally more taxing, slower, and more prone to errors than performing operations *within* a single module. Think of it like sending a letter across the country versus just passing a note to someone sitting next to you—long-distance communication takes more effort and time.


The figures above show the connection patterns (Tanner graphs) for a simple surface code (left) and a more efficient BB code (right). Each line represents a connection, or a two-qubit operation. Now, imagine we divide the qubits in half (along the dashed line) to put them into two different modules.


Since operations *between* modules tend to be more expensive to implement, care must be taken in designing the modular layout of qLDPC codes to avoid overwhelming and bottlenecking inter-module communication. The goal is to minimize the number of connections needed between modules to avoid slowing down the computer.


## Building Smarter Layouts with Flying Qubits


As we've seen, designing a good layout for efficient qLDPC codes is challenging, especially in a modular quantum computer, because of their complex connection patterns. Fortunately, trapped-ion quantum computers have a unique advantage that helps solve this problem: their qubits can be easily moved around.


Qubits in a trapped ion computer are in small groups, or “parcels” (which we can also call “modules”). These parcels of qubits can be moved across shorter distances by adjusting the electromagnetic fields that trap them, and across longer distances by using information carriers like photons to connect them.


This ability to move qubits is a game changer because it means that qubits that are far apart can be brought together when needed, and vice versa. This allows to effectively change the qubit connectivity of a modular system temporarily, in order to perform gates necessary for fault- tolerance. The quantum computer's hardware has the flexibility to adapt to the complex connection requirements of even the most challenging error-correction codes. This added flexibility, combined with smart planning of how to arrange qubits and schedule operations, allows us to build effective layouts for qLDPC codes.


## The Cyclic Layout


The ability to move trapped ion qubits enables a great deal of flexibility. Nevertheless, we want to avoid unnecessary complexity and keep things as simple as possible.


We designed a layout that uses a very specific way to rearrange qubits called the cyclic shift. This has the benefit of being versatile, while being simpler to implement than other solutions for qubit rearrangement. Imagine our qubits are arranged in groups or “parcels” in a line. A cyclic shift is like sliding all those parcels over by a certain number of spots. If a parcel goes off the end of the line, it loops back around to the beginning. The figure below shows how this cyclic shift movement works.


**Figure 2:** Illustration of a cyclic shift, wherein modules or parcels (yellow boxes) containing qubits (gray circles) are shifted right, with modules/parcels at the end looping around to the left.


Armed with this cyclic shift, we construct a *cyclic layout* with just two rows of qubit parcels, illustrated in the figure below. In this layout, each ancilla qubit parcel (highlighted as red boxes) is specifically designed to perform error checking. These ancilla parcels undergo systematic cyclic shifts, moving one slot to the right with each step, as you can see from sequence (a) to (d) in the figure. Through this movement, each red ancilla parcel eventually comes into contact with each data parcel (shown as blue boxes) that holds the quantum information we want to protect. Once an ancilla parcel has completed a full round of cyclic shifts and reaches the end slot where there's no data qubit (as illustrated in figure (d)), all the qubits within that ancilla parcel are measured. We've proven that this straightforward procedure is sufficient to implement all the error-checking routines (syndrome circuits) for any type of stabilizer code, including the more powerful qLDPC codes!


**Figure 3:** Illustration of the cyclic layout. With repeated cyclic shifts, each ancilla module/parcel (red boxes) is brought into contact with every data module/parcel (blue boxes), in order to implement an arbitrary error-correction code.


However, for most of these stabilizer codes, as the code gets larger (meaning it can protect more information or offer a higher level of protection), you need to perform more and more of these cyclic shifts and ancilla qubit interactions. For some QEC codes, like the BB codes, we can do even better!


## The Sparse Cyclic Layout


In the previous section, we saw that while the general cyclic layout is effective for error checking, it can become slow for larger codes because it requires many cyclic shifts. Fortunately, there's a more efficient approach called the sparse cyclic layout. The sparse cyclic layout is a clever variation that skips many of the cyclic shift steps that aren't necessary. This makes the error-checking process much faster, especially for larger codes.


This approach works particularly well with certain types of efficient error-correction codes, like the Bivariate-Bicycle (BB) codes we discussed earlier. BB codes have a structure that naturally fits a cyclic pattern, making them a great match for the sparse cyclic layout.


Adapting the cyclic layout for a BB code is quite simple, and based on a specific rule derived from the code's mathematical definition. Without getting into the math, this rule tells us exactly how to shift the ancilla qubit parcels (our error-checking qubits) for each step of the process.


The figure below visually shows just four specific shifts that are all we need to implement a specific BB code (specifically the 288-qubit one). The specific shifts are applied to the ancilla parcels at each step, following the rule for BB codes. See how the ancilla parcels are shifted in a way that aligns with the code's natural cyclic pattern. Contrast this with the previous *general* layout that isn’t sparse – here we need only four cyclic shift steps, whereas there we would have needed to do L steps (where L gets larger for bigger codes).


**Figure 4:** Illustration of the *sparse* cyclic layout. Only a small set of cyclic shifts are needed to bring each ancilla module/parcel into contact with just a ***few*** necessary data modules/parcels. In this case, just four cyclic shift steps – tailored to the code – are needed even for a large BB code shown here. In essence, this eliminates many of the L cyclic shift steps in the non-sparse layout, leading to a much more efficient implementation.


Laying out a BB code using this sparse cyclic method produces very low overhead. Since BB codes are constructed with Tanner graphs that naturally have cyclic patterns, only a few cyclic shifts are needed to implement them. Crucially, this number remains small regardless of how large that BB code is! This is a significant advantage compared to the general cyclic layout, where the number of shifts increases with the size of the code.


Layout


Cost (BB Code)


2D grid of qubits


Impossible, or requires difficult-to-implement long-range gates


General cyclic


Grows with code size (practical for small codes)


Sparse cyclic


Small number of cyclic shifts for code with a cyclic structure of *any* size


## Looking Ahead


Efficient error correction is essential for scaling quantum systems. Our research tackles this challenge by taking advantage of a feature of trapped-ion quantum computers: their qubits can be moved around. By leveraging the flexibility of flying qubits, and combining it with innovations like sparse cyclic layouts, we’re building systems that bring together fault-tolerance, efficiency, and modularity.


This research is a crucial step towards building larger, more powerful, and fault-tolerant quantum computers.


Blog post based on pre-print article:[https://arxiv.org/abs/2508.01879](https://arxiv.org/abs/2508.01879)
