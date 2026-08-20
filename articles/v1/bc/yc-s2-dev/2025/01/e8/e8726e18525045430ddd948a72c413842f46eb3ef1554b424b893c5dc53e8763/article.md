---
schema_version: "1.0.0"
document_id: "e8726e18525045430ddd948a72c413842f46eb3ef1554b424b893c5dc53e8763"
company_key: "yc-s2-dev"
company: "s2.dev"
source_id: "yc-s2-dev-news-import-d1415bf25083"
canonical_url: "https://s2.dev/blog/iot"
published_at: "2025-01-31T00:00:00+00:00"
first_seen_at: "2026-07-22T12:29:40.978296+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:f68fe81c3186b03523bfca21585396d94806cad99e5fe7bd48286922c02c8eb5"
---

# Blazing-fast IoT data pipeline for infrared monitoring

Real-time IoT devices need real-time action, and wrangling hog loads of data from these devices requires a simple and fast streaming pipeline. Make the data durable, and you can easily have multiple consumers that don’t skip a beat.


**POV:** You have a shared space with your housemates and want to know if it's empty, but in the least privacy-invasive way.


The Recipe:


- **AMG8833** an infrared thermal imaging sensor – 8x8 array of infrared sensors, totaling` 64 pixels` measuring temperatures ranging from` 0°C to 80°C (32°F to 176°F) ±2.5°C (4.5°F)` up to` 7 meters (23 feet)` away at a max frame rate of` 10 Hz` . Cheap, and best for our use case as it covers a large area with a relatively high accuracy.
- **Raspberry Pi 4B** or just any low-cost, single-board computer.
- An **S2 account** as our secret sauce! Follow along[here](https://s2.dev/docs/quickstart) .


For schematics, or to purchase and assemble the electronic components, I recommend following this[MakerPortal article](https://makersportal.com/blog/thermal-camera-analysis-with-raspberry-pi-amg8833) , or this[Adafruit page](https://www.adafruit.com/product/3538) .


Once you have everything set up, it should look something like this:


The S2 API will allow us to model our raw sensor output as a **` Stream`** , which is just an unbounded sequence of records – in other words, our data in motion! The number of streams you can create is unlimited, all namespaced in a globally unique **` Basin`** .


Let's use the[S2 CLI](https://github.com/s2-streamstore/s2-cli) to create a basin called` monitors` , with a stream named` amg8833` .


```text
$   s2   create-basin   monitors
✓   Basin   created
$   s2   create-stream   s2://monitors/amg8833
✓   Stream   created
```


For building our backend I will use the[S2 Python SDK](https://github.com/s2-streamstore/s2-sdk-python) and the[adafruit-circuitpython-amg88xx](https://github.com/adafruit/Adafruit_CircuitPython_AMG88xx) library. Python will help us reduce friction and iterate faster.


Each frame from the sensor reshaped into a` 2D 8x8 array` can be written as a record at the tail of the stream. Since we want to keep pushing data as we read from the sensor, we will use the streaming[AppendSession](https://streamstore.readthedocs.io/en/stable/api-reference.html#streamstore.Stream.append_session) , which gives us acknowledgments in the same order that records were sent.


```text
async   def   read_amg8833  (sensor) -> AsyncIterable[AppendInput]:
while   True  :
try  :
loop   =   asyncio.get_running_loop()
pixels   =   await   loop.run_in_executor(  None  ,   lambda  : sensor.pixels)
pixels   =   np.array(pixels)
body   =   {  "grid"  : pixels.tolist()}
yield   AppendInput(  records  =  [Record(  body  =  json.dumps(body).encode(  "utf-8"  ))])
except   Exception   as   e:
print  (  f  "Sensor error:   {str  (e)  }  "  )
await   asyncio.sleep(  5.0  )
```


```text
async   def   producer  (sensor):
async   with   S2(  auth_token  =  AUTH_TOKEN  )   as   s2:
stream   =   s2[  "monitors"  ][  "amg8833"  ]
async   for   output   in   stream.append_session(sensor_data_gen(sensor)):
print  (  f  "appended   {  output.end_seq_num   -   output.start_seq_num  }   records"  )


if   __name__   ==   "__main__"  :
sensor   =   initialize_sensor()
asyncio.run(producer(sensor))


```


The S2 REST API[supports SSE](https://s2.dev/docs/api/records/read#sse) through which it can push real-time updates to a client over a persistent HTTP connection. SSE is perfectly encapsulated by the[S2 Typescript SDK](https://github.com/s2-streamstore/s2-sdk-typescript) generated using[Speakeasy](https://www.speakeasy.com/) . This will power our little Next.js app, where I can view the "live footage" and know if someone is in the shared space 👀.


```text
const   basinUrl   =   'https://monitors.b.s2.dev/v1alpha'  ;
const   s2   =   new   S2  ({
bearerAuth:   AUTH_TOKEN  ,
});


interface   SensorData   {
occupied  :   boolean  ;
grid  :   number  [][];
}


export   default   function   AMG8833  () {
const   [  occupied  ,   setOccupied  ]   =   useState  (  false  );
const   [  temperatureGrid  ,   setTemperatureGrid  ]   =   useState  <  number  [][]>([]);
useEffect  (()   =>   {
const   fetchStream   =   async   ()   =>   {
try   {
const   tail   =   await   s2.stream.  checkTail  (
{ stream:   'amg8833'   },
{ serverURL: basinUrl }
);


const   result   =   await   s2.stream.  read  (
{
stream:   'amg8833'  ,
startSeqNum: tail.checkTailResponse  !!  .nextSeqNum,
},
{
serverURL: basinUrl,
acceptHeaderOverride: ReadAcceptEnum.textEventStream,
}
);


const   stream   =   result.readResponse;
if   (  !  (stream   instanceof   EventStream  ))   return  ;


for   await   (  const   event   of   stream) {
const   outputData   =   (event   as   ReadResponseOutput  ).data;
if   (  !  outputData)   continue  ;


const   batch   =   outputData   as   Batch  ;
if   (  !  batch.batch?.records)   continue  ;


for   (  const   record   of   batch.batch.records) {
try   {
const   sensorData   =   JSON  .  parse  (record.body)   as   SensorData  ;
setOccupied  (sensorData.occupied);
setTemperatureGrid  (sensorData.grid);
}   catch   (parseError) {
console.  error  (  'Error parsing sensor data:'  , parseError);
}
}
}
}   catch   (error) {
console.  error  (  'Error fetching stream:'  , error);
}
};
fetchStream  ();
}, []);
}
```


I prompted[v0](https://v0.dev/) to help me plot this temperature grid as real pixels converting temperature to[hsl](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/hsl) , and I was able to[just ship it!](https://github.com/s2-streamstore/iot-amg8833)


I added an extra field to the records (see the small bar at the top of the thermal image) to determine whether someone is really in the frame or not by doing some naïve[connected-component labeling](https://en.wikipedia.org/wiki/Connected-component_labeling) to notify me in a text message.


```text
binary_mask   =   pixels   >   28
structure   =   generate_binary_structure(  2  ,   2  )
labeled_array, num_features   =   label(binary_mask,   structure  =  structure)


connections   =   sum  (
[np.count_nonzero(labeled_array   ==   i)   >   4   for   i   in   range  (  1  , num_features   +   1  )]
)
occupied   =   bool  (connections   >   0  )
occupied   =   {  "occupied"  : occupied,   "grid"  : pixels.tolist()}
```


A sweet spot temperature threshold was 28-29°C, and positioning the sensor at a height around 7-8 feet in a cooler area tends to work quite accurately!


The obvious next step that comes to my mind is to train a machine learning model, using supervised learning, for determining the number of people in the frame. Here, S2 stands out since we can not only consume data at the time it is published by the device, but have it available as a durable stream of records to train my model.


So how much does this project cost us in a month? S2 is free for now, but we can refer to the[intended pricing](https://s2.dev/pricing) and figure out a monthly estimate.


Each record is ~470 bytes and at 10 Hz over the course of a month this would be ~12 GiB of data.


- Initial operations to` CreateBasin` and` CreateStream` are fractions-of-a-cent.
- ` AppendSession` and` ReadSession` have a per-minute cost of $0.0000001, adding up to $0.00864.
- If we also keep record retention at a month, at $0.04/GiB-month that will cost us $0.48.
- Data transfer into S2 depends on the storage class. We'll go for the faster` Express` at $0.06 per GiB, which works out to $0.72.
- Data transfer out from S2 depends on where we are accessing it from, with the most cost-efficient being same cloud region. We'll be accessing the stream from home so at $0.08 per GiB for internet egress – assuming we kept the footage up at all times! – this works out to $0.96.


All together, around $2 a month. Not having to worry about high costs, and pushing that side project with a simple API for your data pipeline? Win!


S2 is currently in preview. Come join us on[Discord](https://discord.gg/vTCs7kMkAf) and get hacking!
