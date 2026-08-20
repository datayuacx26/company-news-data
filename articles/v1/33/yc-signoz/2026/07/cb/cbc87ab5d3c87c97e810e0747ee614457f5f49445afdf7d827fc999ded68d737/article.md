---
schema_version: "1.0.0"
document_id: "cbc87ab5d3c87c97e810e0747ee614457f5f49445afdf7d827fc999ded68d737"
company_key: "yc-signoz"
company: "SigNoz"
source_id: "yc-signoz-rss-564a62b873f8"
canonical_url: "https://signoz.io/opentelemetry/customize-metrics-streams-produced-by-opentelemetry-python-sdk"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.602972+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:23568a81a77087e8545e07eaecc0bc3c21a8f8d8959209855f5a26f3467b167d"
---

# Customize metrics streams produced by OpenTelemetry SDK using views

# Customize metrics streams produced by OpenTelemetry SDK using views


Published on: June 20, 2024


Last Updated: July 14, 2026


4 min read


This article is part of the **OpenTelemetry Python series** :


- Previous Article:[Configure OpenTelemetry logging SDK in a Python application](https://signoz.io/opentelemetry/logging-in-python/)
- **You are here:** Customize metrics streams produced by OpenTelemetry SDK using views


Check out the complete series at:[Overview - Implementing OpenTelemetry in Python applications](https://signoz.io/opentelemetry/python-overview/)


In the[previous tutorial](https://signoz.io/opentelemetry/logging-in-python/) , we configured the OpenTelemetry logging SDK to collect logs from our sample Flask application.


In this tutorial, we will share some advanced concepts about customizing metric streams produced by OpenTelemetry SDK.


**


## Change aggregation


It shows how to configure to change the default aggregation using the name of the instrument. Note that views customize metrics that already exist; to define new instruments first,[create custom metrics in Python](https://signoz.io/opentelemetry/python-custom-metrics/) .


```text
import   random
import   time


from   opentelemetry .  metrics  import   get_meter_provider ,   set_meter_provider
from   opentelemetry .  sdk .  metrics  import   MeterProvider
from   opentelemetry .  sdk .  metrics .  export  import    (
ConsoleMetricExporter ,
PeriodicExportingMetricReader ,
)
from   opentelemetry .  sdk .  metrics .  view  import   SumAggregation ,   View


# Create a view matching the histogram instrument name `signoz_latency`
# and configure the `SumAggregation` for the result metrics stream
hist_to_sum_view  =   View (
instrument_name =  "signoz_latency"  ,   aggregation =  SumAggregation (  )
)


# Use console exporter for the example
exporter  =   ConsoleMetricExporter (  )


# Create a metric reader with stdout exporter
reader  =   PeriodicExportingMetricReader (  exporter ,   export_interval_millis =  1_000  )
provider  =   MeterProvider (
metric_readers =  [
reader ,
]  ,
views =  [
hist_to_sum_view ,
]  ,
)
set_meter_provider (  provider )


meter  =   get_meter_provider (  )  .  get_meter (  "view-change-aggregation"  ,    "0.1.2"  )


histogram  =   meter .  create_histogram (  "signoz_latency"  )


while    1  :
histogram .  record (  99.9  )
time .  sleep (  random .  random (  )  )


```


## Configure Temporality


It shows how to have multiple exporters with different temporalities.


```text
import   time


from   opentelemetry .  sdk .  metrics  import   Counter ,   MeterProvider
from   opentelemetry .  sdk .  metrics .  export  import    (
AggregationTemporality ,
ConsoleMetricExporter ,
PeriodicExportingMetricReader ,
)


temporality_cumulative  =    {  Counter :   AggregationTemporality .  CUMULATIVE }
temporality_delta  =    {  Counter :   AggregationTemporality .  DELTA }


# Use console exporters for the example


# The metrics that are exported using this exporter will represent a cumulative value
exporter  =   ConsoleMetricExporter (
preferred_temporality =  temporality_cumulative ,
)


# The metrics that are exported using this exporter will represent a delta value
exporter2  =   ConsoleMetricExporter (
preferred_temporality =  temporality_delta ,
)


# The PeriodicExportingMetricReader takes the preferred aggregation
# from the passed-in exporter
reader  =   PeriodicExportingMetricReader (
exporter ,
export_interval_millis =  5_000  ,
)


# The PeriodicExportingMetricReader takes the preferred aggregation
# from the passed-in exporter
reader2  =   PeriodicExportingMetricReader (
exporter2 ,
export_interval_millis =  5_000  ,
)


provider  =   MeterProvider (  metric_readers =  [  reader ,   reader2 ]  )
set_meter_provider (  provider )


meter  =   get_meter_provider (  )  .  get_meter (  "preferred-temporality"  ,    "0.1.2"  )


counter  =   meter .  create_counter (  "requests"  )


# Two metrics are expected to be printed to the console per export interval.
# The metric originating from the metric exporter with a preferred temporality
# of cumulative will keep a running sum of all values added.
# The metric originating from the metric exporter with a preferred temporality
# of delta will have the sum value reset each export interval.
counter .  add (  5  )
time .  sleep (  10  )
counter .  add (  20  )


```


## Control attributes


Shows how to limit the number of attributes that are output for a metric.


```text
import   random
import   time
from   typing  import   Iterable


from   opentelemetry .  metrics  import    (
CallbackOptions ,
Observation ,
get_meter_provider ,
set_meter_provider ,
)
from   opentelemetry .  sdk .  metrics  import   MeterProvider ,   Counter
from   opentelemetry .  sdk .  metrics .  export  import    (
ConsoleMetricExporter ,
PeriodicExportingMetricReader ,
)
from   opentelemetry .  sdk .  metrics .  view  import   View


# Create a view matching the Counter instrument `requests`
# and configure the attributes in the result metric stream
# to contain only the attributes with keys with `k_3` and `k_5`
view_with_attributes_limit  =   View (
instrument_type =  Counter ,
instrument_name =  "requests"  ,
attribute_keys =  {  "k_3"  ,    "k_5"  }  ,
)


exporter  =   ConsoleMetricExporter (  )


provider  =   MeterProvider (
metric_readers =  [
reader ,
]  ,
views =  [
view_with_attributes_limit ,
]  ,
)
set_meter_provider (  provider )


meter  =   get_meter_provider (  )  .  get_meter (  "reduce-cardinality-with-view"  ,    "0.1.2"  )


request_counter  =   meter .  create_counter (
name =  "requests"  ,
description =  "number of requests"  ,
unit =  "1"  ,
)


while    1  :
for   i  in    range  (  random .  randint (  1  ,    100  )  )  :
request_counter .  add (  1  ,    {  "k_{}"  .  format  (  i )  :    "v_{}"  .  format  (  i )  }  )
time .  sleep (  1  )


```


## Drop metrics


Shows how to drop a metric entirely


```text
import   random
import   time


from   opentelemetry .  sdk .  metrics  import   Counter ,   MeterProvider
from   opentelemetry .  sdk .  metrics .  export  import    (
ConsoleMetricExporter ,
PeriodicExportingMetricReader ,
)
from   opentelemetry .  sdk .  metrics .  view  import   DropAggregation ,   View


# Create a view matching the counter instrument `requests`
# and configure the view to drop the aggregation.
drop_aggregation_view  =   View (
instrument_type =  Counter ,
instrument_name =  "requests"  ,
aggregation =  DropAggregation (  )  ,
)


exporter  =   ConsoleMetricExporter (  )


provider  =   MeterProvider (
metric_readers =  [
reader ,
]  ,
views =  [
drop_aggregation_view ,
]  ,
)
set_meter_provider (  provider )


meter  =   get_meter_provider (  )  .  get_meter (  "view-drop-aggregation"  ,    "0.1.2"  )


my_counter  =   meter .  create_counter (  "requests"  )


while    1  :
my_counter .  add (  random .  randint (  1  ,    10  )  )
time .  sleep (  random .  random (  )  )


```


## Conclusion


We are at the end of our tutorial series on implementing OpenTelemetry in Python applications. OpenTelemetry is the best open-source standard for setting up observability in your applications. It is a complete solution with logs, metrics, and traces provided by a single standard.


We have built[SigNoz](https://signoz.io/docs/introduction/) on OpenTelemetry from the ground up. You can use SigNoz to visualize any data that you collect from OpenTelemetry.


Kudos on completing the entire[OpenTelemetry Python series](https://signoz.io/opentelemetry/python-overview/) . Share your achievement on[LinkedIn](https://www.linkedin.com/) /[Twitter/X](https://x.com/) 🎉
