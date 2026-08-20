---
schema_version: "1.0.0"
document_id: "d350c06507c16d8109438e759c5066bc8a79f92bec8a1f88d87991eb1cf30db8"
company_key: "yc-sf-tensor"
company: "SF Tensor"
source_id: "yc-sf-tensor-news-import-73b0de6f3d55"
canonical_url: "https://sf-tensor.com/engineering/bitwise-tcgen05"
published_at: "2026-07-11T00:00:00+00:00"
first_seen_at: "2026-08-04T22:31:30.643765+00:00"
fetched_at: "2026-08-05T03:48:37.411401+00:00"
content_hash: "sha256:1c4a0a4df7448cf0cce99ae341b2c1d43458fa9ead34f35490ff96044d6f5696"
---

# Reverse-Engineering the B200's Tensor Core, One Bit at a Time

**The short version:** We built a software model of NVIDIA Blackwell's` tcgen05.mma` instruction that matches real B200 hardware bit-for-bit. Not "close enough for numerics." Every output word, identical. What started as one datapath (TF32) is now 46 hardware-validated datapaths from dense and sparse, floating-point and integer to block-scaled and mixed-format, covering the whole` tcgen05` family we could reach on the chip. We did it by treating the tensor core as a black box and probing it until it confessed how it actually computes: operand layout, output slice, special-value handling and a fixed-point accumulation datapath that looks nothing like the IEEE FMA you'd assume it runs. We're releasing the whole thing[open source](https://github.com/sf-tensor/tcgen05) .


## The problem


If you want to compile to high utilization, you have to know exactly what the hardware does. Not the documented abstraction: the actual behavior of the silicon on edge cases, denormals, cancellation and NaN payloads. For` tcgen05.mma` , the matrix multiply-accumulate instruction at the center of Blackwell's tensor core, unfortunately, NVIDIA does not bless us with that information in the manual. The PTX docs tell us the instruction exists and give us a descriptor format. They do not, however, tell us how operands are physically laid out or what rounding the accumulator uses when three of the nine summands cancel.


So we stopped reading and started measuring. We run controlled MMA cases on a real B200, capture the raw output bits and compare them against candidate software models. A model is correct only when it reproduces the hardware bits precisely across all cases without a single bit of delta. Everything below is what the hardware told us.


## Why the obvious approach fails


The obvious model is the one you may assume: convert inputs to floats, compute` D = C + sum(a\[i\] · b\[i\])` , round to nearest even, done. It's clean, it's IEEE and it's unfortunately wrong.


That's the first thing we tried and the exact round-to-nearest-even model mismatched 144 of 2,000 raw cases. Next we tried a more careful two-stage model, fusing the product group and then merging C. Still, it missed 12 of 2,000. For many cases this may be sufficient, because the error rates are small, but "small" is disqualifying when the goal is bit-exactness, because each of those mismatches is the hardware doing something the standard FMA-shaped model doesn't express.


However, looking at the failures, they are clustered in exactly the places floating-point gets interesting: cancellation, sub-ULP contributions and denormal boundaries, which was the first signal that the accumulator isn't rounding term-by-term but instead is doing something with more internal precision and, to model it, we have to find the actual quantum.


## The probe ladder


To find out what's going on, we had to isolate one unknown at a time, with each probe constrained to a single variable, so a failure points at one thing instead of the whole system. We'll walk through it for TF32, the first datapath we cracked; the same ladder is what we later re-ran on every other format.


**Confirm the instruction.** First, we ran SASS inspection to verify the compiler emitted the real tensor-core op (` UTCHMMA` ) and not some fallback, because we've learned the hard way that if you're not measuring the instruction you think you are, nothing downstream means anything.


**Find the output slice.** Tensor memory hands back a pile of words and only some of them carry the scalar result. We replicated the same eight A values across all rows and the same eight B values across all columns, so the useful output degenerates to one scalar dot product` D = C + sum(a\[i\]*b\[i\])` , then we probed which tensor-memory words actually tracked that scalar. For TF32 the stable slice is the first 32 threads times 4 registers.


**Prove position independence.** By activating one product lane at a time, plus C-only cases, we can confirm each logical summand contributes consistently regardless of position, which is how we prove the replication trick is valid and the dot product is really behaving like a sum of independent terms.


**Find the input truncation.** By flipping individual low bits of A and B near 1.0 and watching the output we see that bits 0 through 12 changed nothing while higher mantissa bits moved the result, which is the hardware truncating A/B to the TF32 container before it does anything else (` bits & 0xffffe000` , sign plus exponent plus the top 10 fraction bits, which is truncation, not rounding).


Which input bits the hardware actually sees (TF32)


A/B operands are truncated into the TF32 container before arithmetic or special-value classification.


31


S


30


E


29


E


28


E


27


E


26


E


25


E


24


E


23


E


22


F


21


F


20


F


19


F


18


F


17


F


16


F


15


F


14


F


13


F


12


x


11


x


10


x


9


x


8


x


7


x


6


x


5


x


4


x


3


x


2


x


1


x


0


x


preserved by the hardware


sign


exponent


top 10 fraction bits: TF32 container


flipping these changed nothing


preserved


discarded before compute


Mask observed: bits & 0xffffe000.


Flipping fraction bits 12 through 0 did not move the hardware output; bits 31 through 13 are the visible input payload.


**Find the visible precision.** By testing` 1 + epsilon` for ever-shrinking epsilon we saw the result kept changing down through 2^-23, then vanished at 2^-24, which is an F32-sized visibility at the output, as expected.


**Find the internal precision.** Now the interesting one. By testing` -1 + 1 + epsilon` we cancel the big terms and see how small a survivor the accumulator actually preserves. We saw that these stayed exact through 2^-25, which means the internal path carries more precision than the F32 output can show, which was the first major hint at why the naive model fails.


**Rule out simple rounding.** Fractional-ULP probes around C (` 1 + 0.25ulp` ,` 1 + 0.5ulp` ,` 1 + 0.75ulp` ) show the hardware was not doing one exact nine-term sum followed by round-to-nearest-even and the pattern of what survived and what didn't only fits a shared fixed-point window with truncation without any final rounding steps.


## The mechanism


When we put the probe results together, it became clear that the datapath is a fixed-point accumulator, not a sequence of floating-point adds. From that we built a model of what the hardware actually does and how we match it.


**Truncate the inputs.** A and B get chopped to their TF32 container first:` bits & 0xffffe000` , then special-value classification happens, which matters more than it sounds, because a NaN whose payload lives only in the discarded low bits becomes an infinity after truncation; therefore, it's important that we truncate first, then classify or we'd get the wrong special values.


**Decode to integers internally, not floats.** One of the core design decisions in the model to ensure it's truly exact is that we never convert to a Python float and multiply; instead, we decode each input's raw bits into an integer significand and an exponent, so that a normal TF32 value becomes a mantissa` (1 << 10) | frac_top10` with exponent` raw_exp - 10` and C decodes as full F32 (a 23-bit mantissa). After that, everything downstream is integer/bit arithmetic, so that there's no hidden rounding sneaking in from the host's float unit.


**Multiply integer mantissas.** For each product,` product_mant = a_mant * b_mant` and` product_exp = a_exp + b_exp` are calculated through exact integer multiplication without any per-product rounding, because the hardware doesn't seem to round either.


**Align everything into one shared window.** Through the cancellation probes, we exposed an important mechanism, which is that the accumulator picks a single quantum for the whole dot-product-plus-C group:


```text
quantum_exp = max(nonzero product raw exponent sums, nonzero C raw exponent) - 25


```


Every product and C is shifted into units of` 2^quantum_exp` with truncation toward zero, so terms too small to reach the window lose their low bits. Then all the shifted integers are summed exactly as integers/bits, or otherwise put: it's a fixed-point accumulator carrying 25 fractional bits below the largest exponent in the group without any products or C being rounded independently, instead, the only "rounding" is that alignment into this window and the truncation in doing so.


**Convert back to F32 by truncating magnitude.** Finally, the resulting signed integer's bits become F32 bits through pure bit logic: find the bit length, pick the exponent, shift the mantissa into 23 fraction bits, truncate toward zero, handle subnormal and overflow, canonicalize zero to +0 without any "real floats" ever touching the arithmetic path.


The tcgen05 accumulator datapath


The bit-exact model is integer arithmetic all the way until it emits raw F32 bits.


1. Truncate


container first


A/B: bits & 0xffffe000


C: full F32


classify after truncation


2. Decode to int


no host float


(signed mantissa, exponent)


TF32 mantissa: 1<<10 | frac


F32 mantissa: 1<<23 | frac


3. Multiply


exact products


a_mant * b_mant


exp = a_exp + b_exp


C passes through


4. Shared window


one quantum


quantum_exp = max(raw exponents) - 25


shift terms into window


truncate low bits toward zero


sum exact integers


5. Emit F32


raw output bits


find bit length


truncate magnitude to 23 fraction bits


handle zero, subnormal, overflow


The shared fixed-point window is the behavior the cancellation probes exposed: terms align once, truncate toward zero, then sum as integers. Explore the shared fixed-point window


Change the input vectors and watch the model pick one quantum, align all terms, chop low bits and emit raw F32.


A/B operand format. C remains F32.


A vector


B vector


C accumulator


Modeled result


32


0x42000000


quantum 2^-22


no alignment truncation


emitted F32 matches exact sum


Aligned bit window


quantum 2^-22


no alignment truncation


2^


4


3


0


-4


-8


-12


-16


-20


-22


-24


a0 x b0


a1 x b1


a2 x b2


C


sum


kept positive bit


kept negative bit


below window floor


summed units


exact value


1048576 x 2^-18


window shift


left 4 bits


survives


16777216 units


chopped


nothing


Use the presets to hit the interesting cases: coarse inputs, C-dominated windows, cancellation and special values.


This is the same raw-window rule rendered as a small hardware oracle: A/B are TF32 or BF16, C is F32 and the output is the modeled raw F32 word.


**Special values, from observation.** Through tests we verified that NaN is canonical` 0x7fffffff` and that zero times infinity produces said canonical NaN, that mixed-sign infinities produce canonical NaN, that finite underflow canonicalizes to positive zero and that classification runs after input truncation.


## The result for TF32


Testing our model for TF32 (` Tcgen05RawWindowTf32MmaModel` ) against real B200 hardware, we observe 0 mismatches across millions of random and finite-only cases.


## One rule, many formats


The great thing about getting the mechanism right is that because NVIDIA reuses most of the datapath, it generalizes. What we found across the rest of the instruction family is that the *arithmetic core* (truncate inputs, decode to integers, multiply mantissas, align into one shared truncating fixed-point window, convert back by truncating magnitude) does not change. What changes from format to format is almost entirely bookkeeping around that core:


- **The input decode.** Each element type keeps a different number of mantissa bits and uses a different exponent bias. BF16 keeps the top 7 fraction bits (` bits & 0xffff0000` , mantissa` (1 << 7) | frac_top7` , exponent` raw_exp - 7` ). F16 keeps 10 fraction bits with the FP16 bias. FP8 (E4M3, E5M2), FP6 (E2M3, E3M2) and FP4 (E2M1) each decode their own tiny fields. In the code this is one method swap:` Tcgen05RawWindowBf16MmaModel` is the TF32 model with the decoder replaced and every other unscaled float format is the same story.
- **The operand layout in shared memory.** This is a hardware-discovery problem per format, not an arithmetic one (more below).
- **The output slice** in tensor memory (also below).


That's the shape of the whole project: one accumulator, many decoders and many extraction harnesses. Below is what each family added on top of the core.


Explore floating-point precision


Choose a format, quantize a value, then flip individual sign, exponent or mantissa bits to see the decoded number change.


Format / size


Value


Stored FP16 value


6.94921875


normal


bias 15


E5M10


error -0.000781250000000178


Valid finite range


-65504


0


65504


min positive 5.9604644775e-8


min normal 0.00006103515625


max finite 65504


Format facts


field shape


E5M10


positive normal exponents


-14…15


max finite


65504


Bit pattern


16 bits


0x46f3


Click any bit to flip it and decode the new pattern.


sign · 1


exponent · 5


mantissa · 10


How this pattern evaluates


sign


(-1)^0 = 1


exponent


2^(17 - 15) = 2^2


significand


1.1011110011₂ = 1.7373046875


1 × 2^2 × 1.7373046875 = 6.94921875


Small NVIDIA formats trade exponent range against mantissa resolution; UE4M3 and UE8M0 appear here because they are the block-scale encodings used by NVFP4 and MX.


The explorer uses round-to-nearest-even for standard conversions, finite saturation for FP4/FP6 and E4M3 and the observed low-bit truncation for the TF32 container used in this article.


### BF16, F16 and the sub-byte floats


BF16, F16 and the FP8/FP6/FP4 formats all run through the exact same shared-window fixed-point accumulator with the same truncation and the same special-value handling. The only model change is the decoder. Each validates to the same bar as TF32: 1,000,000 raw random cases and 200,000 finite-only cases with 0 mismatches, plus sparse-lane suites across their active-lane counts.


Two things did *not* transfer for free, and both were hardware-discovery problems in their own right.


First, the operand layout: BF16 needed its own physical layout in shared memory and finding it was the hardest part of the BF16 work, because naive row-major or compact layouts would execute and even return 16.0 for all-ones inputs, which looks like success, but it isn't because it only proves some lanes are live. The isolated-lane probes exposed the truth: with wrong signatures we saw only lanes 0-7 contributing, or active lanes contributing 2.0 instead of 1.0, or the whole thing collapsing to` sum(a\[0..7\]) * (b0 + b1)` . The layout that finally made every isolated A lane and every isolated B lane contribute exactly 1.0 follows the PTX descriptor rule with T=8, SBO=32 and LBO=64 BF16 elements:


```text
offset(row_or_col, k) =
(row_or_col & 7) * 8
+ (row_or_col >> 3) * 32
+ (k & 7)
+ (k >> 3) * 64


```


Second, the output slice: TF32's valid slice does not carry over, because BF16 uses a stable 96-word slice: threads 0-15 registers 0-3, plus threads 16-31 registers 0-1. Keeping the two flatteners separate was necessary, because if you reuse the TF32 slice for BF16 then a perfectly correct arithmetic model appears broken, because the hardware harness is reading the wrong words. This is a trap worth flagging explicitly: when the math is right but the extraction is wrong, the failure looks identical to a math bug and there's no easy way to differentiate between them. Every format we added carried its own version of this "layout plus slice" cost even when the arithmetic came for free.


### Integer: a genuinely different datapath


The` kind::i8` path is the one place the fixed-point-float story does *not* apply. Integer MMA (` Tcgen05I8S32MmaModel` ) is exactly what the name says: decode each operand as a signed or unsigned 8-bit integer, accumulate the exact` sum(a\[i\]*b\[i\])` into the S32 C value with no windowing, no truncation quantum and no float conversion anywhere. The only descriptor knobs are operand signedness (the four U8/S8 combinations) and whether the S32 result wraps or saturates to the int32 range. That's eight datapaths and they're bit-exact for the boring reason that integer arithmetic is integer arithmetic — but they had to be validated as their own family rather than assumed.


### Block-scaled: the shared window grows a floor


How NVFP4 shares one scale across a block


Each row is a 16-value block. Hover, focus or tap one to pair its FP4 values with the E4M3 scale used to reconstruct them.


Weight precision Hover values or their green scale to isolate a 16-value block.


NVFP4 block layout


Block reconstruction


Hover or tap any row to see the 16 values that share its scale.


FP4 value


shared E4M3 block scale


value ≈ fp4 × block_scale


NVFP4 stores tiny FP4 values alongside a higher-precision scale for every 16-value block. The BF16 view shows the source values before scaling and quantization.


The MX and NVFP4 formats attach a per-block scale to the operands and this is where the accumulator picked up genuinely new structure that the unscaled probes never exposed. The model (` Tcgen05BlockScaledMmaModel` and its subclasses) folds the scale into each product's integer significand and exponent, exactly like an extra factor, so` product_mant = a_mant * b_mant * scale_a_mant * scale_b_mant` . That much is unsurprising.


What the hardware forced us to add is a second lower bound on the quantum. Instead of the window being set purely by the largest product exponent, it is:


```text
quantum_exp = max(max_product_raw - product_window_fraction_bits,
max_scale_raw - scale_floor_offset)


```


In other words, there's a scale-anchored floor sitting underneath the product-anchored window, and which one wins depends on the actual scales in the block. With unit scales, the floor is invisible; it looks like a fixed` 2^-35` quantum, and only arbitrary UE4M3 or UE8M0 scales reveal that it moves with` max(scale_a_raw + scale_b_raw)` . We would never have found this from the dense-float probes; it took block-scaled cases with non-trivial scales to separate the two terms.


The block-scaled families we validated:


- **UE8M0 MX formats** (E4M3, E5M2, E2M3, E3M2, E2M1) with 32-wide scale blocks.
- **MXF4 and MXF4NVF4** E2M1 with` scale_vec::2X` and` scale_vec::4X` , which additionally reduce aligned K groups before quantizing. Quantizing individual products loses a carry on rare scale-floor boundary cases; reducing the K=16 (or K=4) product group first preserves it. We have a labeled hardware vector (` nvfp4-k4-product-carry` ) that lands exactly one internal quantum below an F32 boundary and only comes out right if you do the group reduction — it's the kind of case that makes the difference between 99.99% and bit-exact.
- **NVFP4 with UE4M3 scales** (` Tcgen05RawWindowNvfp4MmaModel` ), which has its own C-dominant merge behavior: when C's exponent reaches the product maximum, the datapath exposes four K=16 product block sums, each truncated into a C-relative 35-bit window before the final sum.


### Mixed formats


The instruction also lets A and B be *different* element types. Rather than write a model per pair, there's one mixed accumulator (` Tcgen05MixedRawWindowMmaModel` ) that plugs an independently selected decoder into each of A and B and otherwise runs the identical shared-window core, with optional F16 output rounding (round-to-nearest-even into the FP16 container) when the destination type calls for it. This covers every ordered A/B combination among E4M3, E5M2, E2M3, E3M2 and E2M1, with F32 or F16 output, which is 50 descriptor-level mixed combinations sharing one implementation.


### Sparse


Sparsity turned out to be purely a front-end concern: the metadata selects which logical K positions are active (one-of-two for TF32, two-of-four for the byte-and-smaller formats, two adjacent pairs of eight for NVFP4) and the disabled positions are simply removed *before* special-value classification. Once the active operands are selected, the arithmetic is identical to the dense path, which means a disabled lane carrying a NaN must not poison the result, which we check directly by comparing a sparse dot against an explicitly masked dense dot, which lets every dense format pick up a sparse sibling for free on the arithmetic side, while still paying the per-format layout-and-slice cost on the harness side.


## The full validated suite


Putting it together, we validate 46 distinct hardware datapaths, each to the same 0-mismatch bar, grouped as:


- **Dense unscaled float (8):** TF32, BF16, F16, E4M3, E5M2, E2M3, E3M2, E2M1 — all → F32.
- **Dense integer (8):** the four U8/S8 sign combinations, each in wrapping and saturating variants, → S32.
- **Dense UE8M0 block-scaled (8):** MX E4M3/E5M2/E2M3/E3M2/E2M1, plus MXF4 and MXF4NVF4 (` scale_vec::2X` and` 4X` ) → F32.
- **Dense UE4M3-scaled NVFP4 (2):** MXF4NVF4 and NVFP4 at` scale_vec::4X` , which use separate retained hardware runners but exercise the same UE4M3/NVFP4 arithmetic family.
- **Sparse unscaled float (8):** sparse siblings of the eight dense float formats.
- **Sparse UE4M3-scaled NVFP4 (1)** and **sparse UE8M0 block-scaled (8):** sparse MX E4M3/E5M2/E2M3/E3M2/E2M1, MXF4 and MXF4NVF4 (` 2X` and` 4X` ).
- **Sparse UE4M3-scaled hybrid (1):** sparse MXF4NVF4 at` scale_vec::4X` .
- **Mixed-format representatives (2):** E4M3 A × E2M1 B → F32 and E2M1 A × E4M3 B → F16.


The two mixed representatives stand in for the full 50-combination mixed space; because those permutations share one accumulator, the suite covers the two directional representatives plus separate million-case coverage of every individual decoder.


## The honest truth: what's still unprobed


Everything above is a bit-exact model of` tcgen05.mma` datapaths we could actually run and check against silicon on a B200. For each of the 46, the operand layout, output slice, special-value behavior and finite arithmetic all match the hardware exactly, which is the whole point, because you can't compile to peak utilization or build a formal model on top of a datapath you don't understand.


But this is still a slice of the descriptor space, not all of it. The suite pins down specific M/N shapes and the format/scale/sparsity combinations listed above. Larger N tilings, shapes we didn't enumerate and any descriptor corner outside this list are unprobed by the released repository and its tests and we make no bit-exactness claim there. The arithmetic core has generalized cleanly every time we've extended it, which makes us optimistic, but optimism isn't a hardware vector, so where we haven't measured, we like to say so.


That said, the whole idea behind this exercise is to back everything with tested hardware cases. If you find this type of work interesting and are looking for a job: if you extend the model to another datapath, along with the tests to prove it and send it to me, I'll interview you for any of our[roles](https://sf-tensor.com/careers) .
