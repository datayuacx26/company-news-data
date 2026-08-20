---
schema_version: "1.0.0"
document_id: "a773f12fc9b38dbc56b31d87ddb238af7f100bdca209da4002135f1bde45a846"
company_key: "yc-estimote-inc"
company: "Estimote, Inc."
source_id: "yc-estimote-inc-rss-9f8374160089"
canonical_url: "https://blog.estimote.com/post/165720366660"
published_at: "2017-10-11T03:18:03+00:00"
first_seen_at: "2026-07-27T09:08:36.947635+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:528d7c73941466283d9fe4732f3a7c0639dcc2e8a8381df8fa178a1a60151590"
---

# Estimote introducing Low Power Asset Tracking API

At Estimote, we keep adding building blocks for truly intelligent locations. Our vision of the future involves workplaces and offices that make lives easier, thanks to a sophisticated mixture of sensors,[location algorithms](http://blog.estimote.com/post/164744113385/estimote-launching-precise-indoor-positioning-for) , and communication over[low-power mesh networks](http://blog.estimote.com/post/162271862085/get-started-with-low-power-routed-ble-mesh-using) . Today, we’re adding another layer on top this strong foundation. We want to introduce you to Estimote’s Asset Tracking API.


This technology allows you to create an asset tracking solution in a matter of hours, not days, with elements of our stack you likely already have.


**Why are we building Asset Tracking technology?**


Asset Tracking API is an answer to real life problems. We hear a lot about them from our community. This kind of technology can be used in a factory to track the position of particular hardware or, say, at an assembly line where plane components have to be found and installed in an extremely efficient manner. Another interesting real-life example is tracking medical equipment at hospitals. We know that efficiency in locating crucial tools is a good way to save time and money from day one.


We attempted to tackle this problem earlier, with an initial version of asset tracking based on our beacons with Indoor and Nearables. Even in that first version, we had no need for gateways and wires. That doesn’t mean it was perfect: the core principle was the same (track the placement of a Nearable using beacons), but there was much more overhead in the process. In practice, it only collected data about the Nearables’ placement when an Internet-connected device with an app was around. This meant that someone had to walk around the entire space quite frequently, or the Nearables’ position wouldn’t be updated. Naturally, this wasn’t good enough for us long term. We wanted to take things further, and build something smarter.


**Estimote’s new way of tracking assets**


We began with a very specific focus, the same we have for our[mesh networking](http://blog.estimote.com/post/162271862085/get-started-with-low-power-routed-ble-mesh-using) : low power usage. We built Asset Tracking on top of our low-power BLE mesh, in fact. It’s cheaper and easier to deploy—and to maintain, since there’s no power supply needed at any point of the entire process.


**How does it work?**


It’s actually pretty simple: take some Estimote Stickers and place them on the items you’d like to track. They need to be within range of a single Location beacon or in a location that’s mapped using Location beacons. The only requirement is that you have firmware 4.13.2 or newer installed on the Location Beacons with F3.3 or I1.2 hardware version.


After a straightforward, cloud-based configuration process, the beacons scan for Stickers in their vicinity. By default, beacons are scanning for stickers every 3 minutes. To get the current location of Stickers, just send a request to the Estimote Cloud API — this triggers a scan and data collection over mesh. You can use a Swift helper method in Estimote iOS SDK:


```text
func generateReport()
{
let meshManager: ESTMeshManager = ESTMeshManager()
meshManager.prepareNearablesScanReportCommand(forNetworkIdentifier: meshIdentifier, completion: { (error) in


if let error = error {
print("Generate nearable scan report failed with error: \(error)")
return
}


print("Generate nearable scan report success!")
})
`</pre>


}
<pre>`
The data is then read from any of the beacons by either an iOS or Android device. This operation can be performed manually


<script src="https://gist.github.com/martinklimek/7463a55b69bad66ed68b19d0e668e10b.js"></script>
`</pre>


func readReportFromDevice(device: ESTDeviceLocationBeacon) {
<pre>`    // Connection is required to read Asset Tracking report
if device.connectionStatus != ESTConnectionStatus.connected { return }


device.scanReports?.readScanReport(completion: { (report: ESTMeshNearablesScanReportVO, error: Error) in


guard let error = error else {
return
}


// You should use first 4 bytes of Nearable identifier
// Returned string contains first 4 bytes of beacon identifiers
let closestDeviceIdentifier: String = report.closestDeviceIdentifier(forNearableIdentifier: "aabbccdd")
})
`</pre>


}
<pre>`
or with the help of a Mesh Gateway. With several dozen beacons in the space, you'll get the results in a matter of minutes with the [Estimote Cloud API]([https://cloud.estimote.com/docs/]( https://cloud.estimote.com/docs/  )). Ask yourself: how long would it take you to walk around a warehouse and check every nook and cranny for that one particular box you're looking for? For 25 beacons, it's well under 10 minutes.


<script src="https://gist.github.com/martinklimek/df46fd5a8e4db30d78f29b715f03a60d.js"></script>
`</pre>


class ViewController: UIViewController {
<pre>`let meshGateway: ESTMeshGateway = ESTMeshGateway()


override func viewDidLoad() {
super.viewDidLoad()


self.meshGateway.start()
}
`</pre>


}
<pre>`
Instead of creating a dedicated hardware gateway, we’re introducing gateway functionality in our SDK. Any phone with the right software can now work as a gateway. To make things sweeter, every beacon collects data from the entire mesh, so even a single phone, at any point in the range of the mesh, is enough to get the full picture. Isn’t that better than buying, say, 400 additional devices you need to configure and maintain?


The cherry on top? To get this setup, you don't even have to map out the space yourself. Thanks to [Location Beacons with UWB]([http://blog.estimote.com/post/154460651570/estimote-beacons-with-uwb-can-now-automatically]( http://blog.estimote.com/post/154460651570/estimote-beacons-with-uwb-can-now-automatically  )), Estimote does the mapping for you. Just follow our setup wizard and place the beacons around the location, then watch the map get drawn with inch-perfect precision.


### Try it now!


We're really excited to get this technology into your hands. If you want to know more just drop us a line at [contact@estimote.com](mailto:contact@estimote.com?subject=Asset%20Tracking).


[Rafał Ociepa](mailto:raphael@estimote.com), Content Manager at [Estimote]([http://estimote.com/]( http://estimote.com/  ))


[Marcin Klimek]([https://twitter.com/klimek_marcin]( https://twitter.com/klimek_marcin  )), Product Manager at [Estimote]([http://estimote.com/]( http://estimote.com/  ))


```
