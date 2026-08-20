---
schema_version: "1.0.0"
document_id: "ead73e50dd411b0e3d9634fe6348a0a2fe129d90e58e959e1968a35a1d73705a"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/packet-capture-without-tcpdump/"
published_at: "2021-07-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:55096f67fb32414a0a28ee745c8405b856160efcef496797b7f9cb5a6536b60f"
---

# Packet Capture Without "tcpdump" for Go Apps in Kubernetes

Every developer knows there are some utilities that are completely indispensable from their workflows. The *programmer’s toolbelt,* if you will. These toolbelts are usually different from person to person, but if there is one tool that everyone should use or at least know how to use, it is` tcpdump` . If you are unfamiliar,` tcpdump` is a tool that allows you to dump and inspect live network traffic being observed on a network interface. It supports a wide array of options including both filtering using BPF (Berkley Packet Filter) expressions and the ability to create packet capture (pcap) files. For example, we can watch an HTTP request occur to a local test server, like a` curl -v` but on steroids: This live traffic view of network packets is certainly useful, but I much prefer to use` tcpdump` to create pcap files and then inspect them more closely in` wireshark` : This works great if you are dealing with your local machine, a physical server, or a plain cloud compute instance like an EC2. However, the Kubernetes ecosystem throws a few wrenches into this approach:


- Your containers need to be built with` tcpdump` available
- You need to modify your container’s entrypoint to support running both your program and` tcpdump` (if you want continuous capture)
- You need to exec a shell in the container to run` tcpdump` (if you want on-demand capture)
- You need to modify your container definition to run with elevated permissions - something you may not want to do for security reasons


If you are developing in Go, it’s possible to instrument your application in such a way that diagnostic pcaps are generated for you automatically without needing to run` tcpdump` . To do this, we can use[github/google/gopacket](https://github.com/google/gopacket) , an extremely useful packet parsing library, which contains utilities for reading and writing pcap files, as well as for capturing packets directly from a network interface. There are few requirements to consider when using this approach:


- The packet capture utilities utilize` libpcap` C calls, so your application must not disable cgo when being built
- Your build environment must have` libpcap` and headers (usually` libpcap-dev` ) installed


The gopacket library makes it very easy to put together the kind of simple diagnostic packet capture we are looking to add to our program. What’s great about this approach is that you are effectively narrowing the scope to just the pod you want to inspect, so you don’t need to worry about the security risks of basically running tcpdump as a privileged user on your physical nodes. The downside to this approach is that, while you *can* capture TLS encrypted traffic, you won’t actually be able to view it in plaintext. First, we need to open a pcap handle to a live network interface, say` eth0` , as well as create and initialize our destination pcap file:


```text
package   example


import   (
"  context  "
"  os  "
"  time  "


"  github.com/google/gopacket  "
"  github.com/google/gopacket/layers  "
"  github.com/google/gopacket/pcap  "
"  github.com/google/gopacket/pcapgo  "
)


func   RunPacketCapture  (  ctx   context  .  Context  ) {
sourceHandle, err   :=   pcap.  OpenLive  (  "eth0"  ,   1600  ,   true  , pcap.BlockForever)
if   err   !=   nil   {
log.  Fatal  (  "Failed to create packet capture handle"  )
return
}
defer   sourceHandle.  Close  ()


// capture only tcp traffic
if   err   :=   sourceHandle.  SetBPFFilter  (  "tcp"  ); err   !=   nil   {
log.  Fatal  (  "Failed to set BPF filter"  )
return
}


// destination path you want your pcap saved to
outputFilename   :=   "/tmp/myapp-capture.pcap"
outputFile, err   :=   os.  Create  (outputFilename)
if   err   !=   nil   {
log.  Errorw  (  "Failed to craete packet capture output file"  ,   "err"  , err,   "filename"  , outputFilename)
return
}
defer   func  (  outputFile   *  os  .  File  ) {
if   err   :=   outputFile.  Close  (); err   !=   nil   {
log.  Errorw  (  "Failed to close file"  , log.FErr, err)
}
}(outputFile)


outputWriter   :=   pcapgo.  NewWriter  (outputFile)
if   err   :=   outputWriter.  WriteFileHeader  (  1600  , layers.LinkTypeEthernet); err   !=   nil   {
log.  Errorw  (  "Failed to write file header"  , log.FErr, err)
return
}
}
```


```text
packetSource   :=   gopacket.  NewPacketSource  (sourceHandle, sourceHandle.  LinkType  ())
```


```text
packetChan := packetSource.Packets()


for {
select {
case <-ctx.Done():
log.Println("Packet capture context cancelled, exiting")
return
case p := <-packetChan:
err = outputWriter.WritePacket(p.Metadata().CaptureInfo, p.Data())
if err != nil {
log.Fatal("Failed to write packet data")
```


```text
}
}
}
```


This gets the groundwork and setup out of the way, but we still need to actually connect the wires by reading the packets from the interface handle and then writing them to our output file. Adding to the above function:


```text
```


```text
packetChan := packetSource.Packets()


for {
select {
case <-ctx.Done():
log.Println("Packet capture context cancelled, exiting")
return
case p := <-packetChan:
err = outputWriter.WritePacket(p.Metadata().CaptureInfo, p.Data())
if err != nil {
log.Fatal("Failed to write packet data")
```


```text
}
}
}
```


Once you have something akin to the example function above, all that is left to do is start the packet capture when the program starts:


```text
package   main


import   (
"  context  "
)


func   main  () {
ctx, cancel   :=   context.  WithCancel  ()
defer   cancel  ()


go   RunPacketCapture  (ctx)


// the rest of your main() function
}
```


Now, in order for this to actually work, you still need to allow escalated privileges for the user running your application within the container. The first thing to do is to edit your pod spec’s template and add security context configuration to your container:


```text
spec:
template:
spec:
containers:
name: your-app
securityContext:
allowPrivilegeEscalation: true
capabilities:
add:
- NET_RAW
- NET_ADMIN
privileged: false
readOnlyRootFilesystem: false
runAsNonRoot: false
```


Also, depending on how your container is being built, you may need to run` setcap` as part of your` ENTRYPOINT` :


```text
ENTRYPOINT setcap cap_net_raw,cap_net_admin+eip /path/to/your/program && /path/to/your/program
```


And that’s it! Now, when your container starts within your pod, the packet capture goroutine will start automatically and you will be able to get this created pcap file with` kubectl` :


```text
kubectl   cp   -n   your-namespace   -c   your-container-name   your-pod-name:/tmp/myapp-capture.pcap   myapp-capture.pcap
```


The above approach gives a raw look at all of a pod’s underlying network traffic. It has helped us discover API connectivity issues that we would not have been able to observe otherwise. In fact, it was an unexpected TCP RST packet from the API (but that is a topic for another post)!


Alternatively, instead of doing all of this yourself,[Speedscale](https://speedscale.com/features/) can perform it automatically! Integration testing APIs can be complex. Code changes can introduce latency or outages, but testing everything with end-to-end environments is slow. With Speedscale, easily change the shape of the traffic replay with config files. Multiply traffic for load testing, validate field by field for integration testing, or introduce latency and non-responsiveness for chaos. No scripting required. Use Speedscale to replay past traffic and get updates out the door with confidence.


To learn more, visit the Speedscale[product page](https://speedscale.com/features/) or[schedule a demo](https://speedscale.com/company/demo/) to review product capabilities with our team.


-Shaun Duncan, Founding Engineer at Speedscale
