---
schema_version: "1.0.0"
document_id: "1ea1b6780382a3d094ebc63fdd2230e31a9ce2efbd7424646a7491cbad7d76a3"
company_key: "yc-apoxy"
company: "Apoxy"
source_id: "yc-apoxy-rss-ae7e2d86e063"
canonical_url: "https://apoxy.dev/blog/netstack-bbrv3"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:03.880414+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:d41f7cf49bedf30fd21048603b5680a1876117e25b22e2c46082c377a5176084"
---

# This Blog Post Runs a TCP Stack

Our co-founder Matt's home internet provider, who shall remain nameless, is not the best. It tends to lose packets and introduce jitter which occasionally causes frustrating moments where you have to talk to Matt's still image on Zoom. We also run some of our probes from Matt's private lab in his house and so the ISP shenanigans also cause spurious alerts when, in reality, nothing is wrong.


See, the probes are running using our[tunnel](https://apoxy.dev/docs/guides/basic-tunnels) infrastructure via` apoxy tunnel` and we designed this to securely tunnel entire IP packets using[CONNECT-IP](https://www.rfc-editor.org/rfc/rfc9484) over QUIC, which is commonly referred to as[MASQUE](https://datatracker.ietf.org/wg/masque/about/) . This is different from most other tunnel platforms that implement custom framing and shuttle only higher-level protocol payload. On the agent side of the tunnel (where` apoxy tunnel` runs), we inject tunneled IP packets into user's network using TAP device, if available, or more frequently, using[netstack](https://github.com/google/gvisor/tree/master/pkg/tcpip) - a fully userspace TCP/IP stack written in Go for gVisor's networking needs.


Netstack contains an entire TCP/IP stack implementation which includes TCP Congestion Control machinery. It has long been my suspicion that default TCP congestion control algorithms in Linux like CUBIC or Reno are not very good on even mildly lossy networks. I heard from a friend about Google's BBRv3 algorithm, which is supposed to be better at this, so I decided to try it out in netstack.


Below is the explanation of how BBRv3 works and why it is indeed better than CUBIC or Reno (at least on lossy networks). The graph sims are *actual* network simulations run in netstack *within your browser* using WebAssembly.


## Bottleneck Bandwidth and Round-trip propagation time


First, there is a realization that from the perspective of TCP, any arbitrary complex path behaves as a single link with just two constraints - *RTProp* (round-trip propagation delay) and *BtlBw* (bottleneck bandwidth).


FIG. 1 - BOTTLENECK


loss **0 %**


jitter **1 ms**


bottleneck 100 Mbps · base rtt 40 ms · buffer 350 pkt · 30 s run ready


t = 00.00 s


0.40×


Netstack simulated traffic, aggregated so the particles remain readable. Their timing comes from forward and reverse link activity. CUBIC's growing window appears in the in-flight bar and queue stack, ACKs follow the complete return path, and queue-overflow ✕ marks leave the queue


while random-loss marks leave the downstream wire. In the RTT strip the solid trace is the latest raw RTT sample and the dotted one is the sender's own smoothed srtt estimate, both against the dashed base-rtt rule. In the delivery strip the solid trace is goodput (bytes reaching the receiver's app), the dotted one is offered load entering the bottleneck - the gap between them is congestion - and the dashed rule is the configured link rate. Playback cruises at 0.4× and slows to 0.15× around losses and phase changes.


In *FIG 1* , *RTProp* is the round-trip time for a ball to traverse the entire path and back without getting queued anywhere along the way. *BtlBw* is the bottleneck bandwidth - maximum number of balls per second that can move through the bottleneck - a point along the path with a minimum data delivery rate. These two values can be combined to calculate the *Bandwidth-Delay Product* or *BDP = BtlBw x RTProp* - which characterizes the maximum amount of data that can be in-flight on this link.


There's another way to think about this - imagine you're driving a delivery truck from Spectrumville to Apoxygard. First, there's a wide highway but then you encounter one or more narrow exits to traverse to another highway. In this metaphor you're also in a rush so you're moving at a *constant* speed of roughly 2/3rds of the speed of light (fiber optic light propagation speed). *BtlBw* is the bottleneck bandwidth - maximum number of trucks per second that can move through the narrowest bridge along your path. And the *RTProp* is the time it takes you to drive the entire distance (and back since one way speed of light[is unmeasurable](https://www.youtube.com/watch?v=pTn6Ewhb27k) ) without any delays along the way.


## BBR vs Loss-based congestion control


Now that we have some intuition for the problem space, let's compare BBRv3 to loss-based congestion control algorithms like CUBIC or Reno. In traditional loss-based algorithms, congestion is detected by packet loss, and the sender adjusts its rate by decreasing cwnd (congestion window size) - the number of trucks that can be en route without delivery acknowledgment. This made sense 30 years ago when buffers were limited and it was pretty easy to max out the network.


Today, with much larger buffers and faster networks, loss-based algorithms have two major flaws:


1. **Packet loss detection** : Loss-based algorithms rely on packet loss as a signal for congestion. These days we have plenty of high bandwidth but relatively high loss networks such as residential WiFi and even some wired networks that do throttling/policing, etc. Packet loss there can lead to dramatic bandwidth underutilization.
2. **Bufferbloat** : Loss-based algorithms routinely lead to bufferbloat on links with bottlenecks. We'll explore why this happens and how BBRv3 deals with this below.


CUBIC, which is the standard TCP congestion control algorithm on most production systems today, comes from the same lineage of loss-based algorithms. It tries to make up for the limitations of loss-based approach by using a more aggressive cwnd (congestion window) growth rate, proportional to the cube of time since the last drop.


Wcubic(t)=C (t−K)3+WmaxW_{cubic}(t) = C\\,(t - K)^3 + W_{max}


W


c


u


bi


c


​


(


t


)


=


C


(


t


−


K


)


3


+


W


ma


x


​


where WmaxW_{max}


W


ma


x


​


is the window size at the last drop, KK


K


is the time it takes to grow back to WmaxW_{max}


W


ma


x


​


, and CC


C


is a scaling constant.


You can observe its concave/convex shape in *FIG 1* throughput chart - it first ramps up fast, then more slowly to allow network to stabilize. Then as it's hit by the taildrop, it cuts the cwnd abruptly, allowing the network to recover. This same cutting back behavior also happens for packet drops *unrelated to buffer saturation* which is why CUBIC delivery rate drops drastically even with modest loss rates.


To better understand the algorithm's behavior and how BBR-style congestion control is different, take a look at this genius diagram I lifted from the original 2016[BBR paper](https://dl.acm.org/doi/10.1145/3012426.3022184) :


FIG. 2 - RTT & DELIVERY VS. INFLIGHT


— cubic


— bbrv3


× cubic drop


× bbrv3 drop


owd **20 ms**


jitter **1 ms**


loss **0 %**


buffer **350 pkt**


link 100 Mbps · BDP 333 pkt · buf 1.05×BDP · base rtt 40 ms · 30 s × 2 runs ready


t = 00.00 s


cubic 0.02 bdp / rtt 1.00×


bbrv3 0.02 bdp / rtt 1.00×


bbr phase: ■ —


First, looking at the lower part of the *FIG 2* - the *X-axis* shows bytes in-flight, limited by the congestion window (` cwnd` ), and the *Y-axis* is delivery rate. When you climb up the chart starting from the lower left corner of the APP-LIMITED


region by increasing the *cwnd* , you eventually reach the *peak rate* - at the BDP point - the highest possible delivery rate for the given bottleneck.


This is the BANDWIDTH-LIMITED


region. Once there is roughly one BDP of data in flight, increasing the congestion window further will not increase the delivery rate: the bottleneck is already saturated. Instead, the additional data accumulates in the bottleneck queue, increasing latency, as you can observe in the upper panel of *FIG 2* . In our delivery-truck metaphor, this is like arriving at the exit ramp and finding a line of trucks already waiting on the shoulder. When a large, unmanaged buffer sustains a queue like this, the result is commonly called bufferbloat.


Eventually, as the growing *cwnd* permits more data in flight, you will reach the BUFFER-LIMITED


region. Here, the bottleneck buffer is full, and when it can't drain the FIFO queue fast enough, newly arriving packets are dropped - this is known as tail-drop. In the simulation run, you can see CUBIC repeatedly attempting to grow beyond this point, then backing off whenever packet drops occur. Back to our delivery truck fable... well, I guess your truck just explodes when there's no more room on the shoulder, sorry. (Driving is the leading cause of mortality for children and young adults).


In contrast, our BBRv3 implementation hovers over the BDP point, avoiding tail-drop and only does infrequent incursions towards the drop point, never quite reaching it, to probe for improved bottleneck conditions. BBRv3 avoids congestion tail-drops entirely by measuring RTT increase instead of pushing the limit until drops occur.


Another key difference is handling of random loss and jitter on the link. You can adjust the sliders to say just *0.003%* loss and watch how CUBIC spends much of the run well below the optimal point, pushing a fraction of the bottleneck bandwidth. At the same time, BBRv3 still tries to hover around the optimal point, called *Kleinrock's operating point* , named after researcher[Leonard Kleinrock](https://en.wikipedia.org/wiki/Leonard_Kleinrock) .


You can go further and set loss to *1%* and watch how CUBIC bandwidth completely collapsing while BBRv3 is chugging along just fine. This kind of loss is not unheard of - WiFi will happily deliver 1-2% loss with some less-than-ideal radio conditions, channel hopping, etc. And while LTE/5G networking in theory should deliver 0% loss, in practice, as we've probably all noticed, that's not the case.


## Behind the scenes


The initial BBRv3 implementation was delivered by Fable 5. As I've put it up against the[IETF draft (-03)](https://datatracker.ietf.org/doc/html/draft-ietf-ccwg-bbr-03) and[Google's C implementation](https://github.com/google/bbr/blob/90210de4/net/ipv4/tcp_bbr.c) and started going line-by-line, it was immediately clear that what was implemented could only be categorized as "loosely inspired by BBR". Many bugs were fixed and as I was building more sims I decided it would be fun to build some visuals from the original BBR paper using *actual* TCP connections which was easy to do as it's all right there in userspace.


In doing so, I've had an even crazier idea to try and put it in directly in the browser by compiling it to WASM and running it inside a worker. Here, I have to hand it to AI - this experience was nearly flawless.


Here's the design in all its glory:


FIG. 3 - CC SIM ARCHITECTURE


binary result stream


precomputed fast path (bypasses WASM)


control & simulation path


Surprisingly, netstack itself compiled to WASM pretty easily, the main challenge was all the browser API machinery required to preserve simulator behavior. The gVisor sender and receiver, as well as the bottleneck model remain pure Go. A thin syscall/js layer exposes interface to load the sim, step through it, and collect results.


Keeping native and WASM results byte-identical was difficult because browser execution, gVisor goroutines, and architecture-specific floating-point behavior could change event ordering or measurements. We had to engineer a single deterministic virtual event loop and explicit floating-point rounding barriers so that our native-versus-WASM parity tests passed.


The other challenge was making a CPU- and memory-heavy Go runtime behave like an interactive web component. Each simulation runs in its own Web Worker so it cannot block React; it advances in 250 ms simulated-time batches and yields through` MessageChannel` , avoiding throttled browser timers.


Because Go's WASM linear memory does not shrink, retired simulation must close their gVisor stacks, break JS callback references, run collection, and ultimately terminate the worker.


We also had to cap concurrency on smaller devices, and precompute default scenarios - my old iPhone SE was taking a minute+ to run a sim.


## Future plans


There are upcoming, newer solutions to this problem like[ECN/Prague](https://www.ietf.org/archive/id/draft-briscoe-iccrg-prague-congestion-control-02.html) that may solve some of these challenges that we're going to explore in a later post. For now, we're pulling BBRv3 into our tunnel infra as well as enabling Google's in-kernel implementation on all edges after seeing such massive performance gains.


As an aside, we're looking into more interesting uses for our netstack-WASM setup so if you have any ideas, please share them! Full implementation for our netstack-WASM setup is available on[GitHub](https://github.com/apoxy-dev/ccsim) under AGPL.


Dmitry Ilyevsky


Co-founder & CTO


Dmitry is co-founder and CTO of Apoxy. He previously built and operated infrastructure at Google, Cruise, and Mux (where he and Matt met).


[GitHub ↗](https://github.com/dilyevsky)


[← Back to all posts](https://apoxy.dev/blog)
