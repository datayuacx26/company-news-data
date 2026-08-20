---
schema_version: "1.0.0"
document_id: "91a2acf67b248cc844b77cac9d1e31e504d0e300a62444ca1a4cd00633a2b6ed"
company_key: "yc-influxdata"
company: "InfluxData"
source_id: "yc-influxdata-rss-012b8d0fa152"
canonical_url: "https://www.influxdata.com/blog/predictive-maintenance-plugin-tutorial/"
published_at: "2026-06-09T08:00:00+00:00"
first_seen_at: "2026-07-20T23:23:58.693982+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:65d34d4f82d623484f5518b62d9b0e240d95fbda99a2a4e255c702374046f699"
---

# Building a Predictive Maintenance Plugin with the InfluxDB 3 Processing Engine

Table of Contents


Predictive maintenance is one of the most compelling use cases for time series data. Instead of waiting for equipment to fail or servicing it on a fixed calendar regardless of condition, you watch the live sensor data and act when it indicates that a failure is coming. That “watch the data and act” loop is exactly what the InfluxDB 3 Processing Engine was built for.


In this tutorial, we’ll build a working predictive maintenance plugin from scratch. We’ll install[InfluxDB 3 Core](https://docs.influxdata.com/influxdb3/core/install/?utm_source=website&utm_medium=direct&utm_campaign=predictive_maintenance_plugin_tutorial&utm_content=blog) , load a well-known public dataset of jet engine sensor data, write a Python plugin that runs inside the database to estimate each engine’s Remaining Useful Life (RUL), and have it raise maintenance alerts automatically. By the end, you’ll have an end-to-end system that you can adapt to pumps, motors, HVAC units, CNC machines, or any other instrumented asset.


## What we’re building


Here’s the architecture at a glance:


1. Sensor data lands in InfluxDB 3 Core. We’ll use[NASA’s C-MAPSS turbofan engine degradation dataset](https://www.kaggle.com/datasets/bishals098/nasa-turbofan-engine-degradation-simulation) , replayed into a` sensors` table as if it were arriving live from a fleet of engines.
2. A[scheduled plugin](https://docs.influxdata.com/influxdb3/core/plugins/) runs every minute. It queries the most recent sensor readings per engine, computes a health/degradation score, and converts that into an estimated Remaining Useful Life.
3. The plugin writes its conclusions back into the database. RUL estimates go into a` rul_estimates` table, and when an engine crosses a danger threshold, the plugin writes a row into a` maintenance_alerts` table and logs a warning.


The key idea is that the analysis logic lives *embedded in the database* . There’s no separate service to deploy, scale, or keep in sync. When data arrives, the engine acts on it.


## Prerequisites


Before starting, make sure you have:


- A Linux or macOS machine (Windows works too via the installer or Docker; commands below assume a Unix-like shell)
- Command-line access
- Python 3 installed locally
- The` train_FD001.txt` file from the C-MAPSS dataset


That’s it. InfluxDB 3 Core itself is a single binary and brings its own bundled Python for plugins.


## Install InfluxDB 3 Core


The quickest path is the official install script, which always pulls the latest release:


```text
curl -O https://www.influxdata.com/d/install_influxdb3.sh \
&& sh install_influxdb3.sh core
```


When the script finishes, confirm it worked:


```text
influxdb3 --version
```


If the` influxdb3` command isn’t found, the installer’s output tells you what to do—usually it’s a matter of sourcing your shell config (for example` source ~/.bashrc or source ~/.zshrc` ) so the new binary is on your` PATH` .


#### Docker


If you’d rather containerize, the Processing Engine is enabled by default in the Docker image (the plugin directory defaults to` /plugins` ):


```text
docker run -it -p 8181:8181 --name influxdb3-core \
--volume ~/.influxdb3_data:/var/lib/influxdb3 \
--volume ~/.influxdb3_plugins:/plugins \
influxdb:3-core influxdb3 serve \
--node-id my_host \
--object-store file \
--data-dir /var/lib/influxdb3 \
--plugin-dir /plugins
```


For the rest of this tutorial we’ll assume the local binary install. The commands translate directly to Docker by prefixing` docker exec -it influxdb3-core` .


## Start InfluxDB with the Processing Engine enabled


The Processing Engine activates only when you tell InfluxDB where your plugins live, using the` --plugin-dir` flag.


First create a directory to hold plugins, then start the server:


```text
mkdir -p ~/influxdb3/plugins


influxdb3 serve \
--node-id host01 \
--object-store file \
--data-dir ~/.influxdb3 \
--plugin-dir ~/influxdb3/plugins
```


A few notes on these flags:


- ` --node-id` is a unique name for this server instance. It forms part of the storage path.
- ` --object-store file` keeps everything on a local disk under
- ` --data-dir` . In production you’d point this at S3 or another object store; InfluxDB 3 uses a “diskless” architecture where object storage is the source of truth.
- ` --plugin-dir` is the directory the engine scans for plugin files. This is the switch that turns the Processing Engine on.


Leave this server running in its own terminal. Open a second terminal for the remaining commands.


It’s worth noting that the` influxdb3` binary depends on an adjacent` python/` directory that ships alongside it. If you extracted from a tarball manually, keep the binary and that` python/` folder in the same parent directory and add the parent to your` PATH` , don’t move the binary out on its own, or plugins won’t run.


## Create an admin token


InfluxDB 3 Core uses token authentication. Several operations require an admin token. Create one with the following command:


```text
influxdb3 create token --admin
```


This prints a token string once. Copy it somewhere safe; you can’t recover it later (you’d have to regenerate). For convenience in this session, export it:


```text
export INFLUXDB3_AUTH_TOKEN="paste-your-token-here"
```


The CLI automatically picks up` INFLUXDB3_AUTH_TOKEN` , so you won’t have to pass` --token` on every command.


## Create a database


Create a database to hold our data:


` influxdb3 create database engine_fleet`


Verify it exists:


` influxdb3 show databases`


You should see` engine_fleet` in the list.


## Load the dataset into InfluxDB


The[C-MAPSS](https://www.kaggle.com/datasets/bishals098/nasa-turbofan-engine-degradation-simulation) file is a flat, headerless, space-separated table. We need to turn each row into a time series point. There’s one wrinkle worth thinking through: the data has a` time_cycles` counter per engine, not real timestamps. To make this behave like a live stream, we’ll synthesize timestamps by mapping each engine cycle to one second of wall-clock time, anchored to “now minus the engine’s lifetime.” That way recent cycles look recent, which is what a scheduled “look at the last N minutes” plugin expects.


Here’s a small loader script. It uses the InfluxDB 3 Python client to write line protocol in batches. First install the client into your local Python (this is separate from the engine’s bundled Python), using[uv](https://docs.astral.sh/uv/) or[venv](https://docs.python.org/3/library/venv.html) for package management if desired::


```text
pip install influxdb3-python pandas
```


Save the following as` load_cmapss.py` , adjusting` DATA_FILE` and` TOKEN` as needed:


```text
# load_cmapss.py
import time
from datetime import datetime, timedelta, timezone


import pandas as pd
from influxdb_client_3 import InfluxDBClient3, Point


# ---- Configuration ----
DATA_FILE = "train_FD001.txt"
HOST = "http://localhost:8181"
DATABASE = "engine_fleet"
TOKEN = "paste-your-admin-token"   # or read from env


# ---- Column names (NASA C-MAPSS layout) ----
index_cols = ["unit_number", "time_cycles"]
setting_cols = ["setting_1", "setting_2", "setting_3"]
sensor_cols = [f"s_{i}" for i in range(1, 22)]
col_names = index_cols + setting_cols + sensor_cols


# ---- Read the space-separated file ----
df = pd.read_csv(DATA_FILE, sep=r"\s+", header=None, names=col_names)
df = df.astype({"unit_number": int, "time_cycles": int})


print(f"Loaded {len(df)} rows across {df['unit_number'].nunique()} engines")


# ---- Synthesize timestamps: 1 cycle == 1 second, anchored so the
#      last cycle of each engine lands at 'now'. ----
now = datetime.now(timezone.utc)
client = InfluxDBClient3(host=HOST, database=DATABASE, token=TOKEN)


points = []
BATCH = 5000


# Per-engine max cycle so we can anchor each engine's final cycle to "now"
max_cycle = df.groupby("unit_number")["time_cycles"].transform("max")
df["ts"] = [
now - timedelta(seconds=int(mc - tc))
for mc, tc in zip(max_cycle, df["time_cycles"])
]


for row in df.itertuples(index=False):
p = (
Point("sensors")
.tag("unit_number", str(row.unit_number))
.field("time_cycles", int(row.time_cycles))
)
for c in setting_cols + sensor_cols:
p = p.field(c, float(getattr(row, c)))
p = p.time(row.ts)
points.append(p)


if len(points) >= BATCH:
client.write(points)
points = []


if points:
client.write(points)


client.close()
print("Done writing sensor data.")
```


Run it:


```text
python load_cmapss.py
```


A few design choices worth calling out:


- **` unit_number` is a tag, everything else is a field.** Tags are indexed and are how you separate one engine’s series from another. Sensor values and the cycle counter are fields.
- **We anchor each engine’s final cycle to “now.”** This means every engine in the fleet looks like it just reached end-of-life, which is convenient for demonstrating alerts. In a real deployment your data already has real timestamps and you’d skip all of this.
- **Batching matters.** Writing 20,000+ points one at a time is slow; batches of a few thousand keep the loader quick.


Confirm the data landed:


```text
influxdb3 query --database engine_fleet \
"SELECT COUNT(*) FROM sensors"
And take a peek at one engine's recent readings:
influxdb3 query --database engine_fleet \
"SELECT time, unit_number, time_cycles, s_2, s_4, s_11 \
FROM sensors WHERE unit_number = '1' \
ORDER BY time DESC LIMIT 5"
```


## An overview of the plugin model


Before writing code, let’s get the mental model straight.


A plugin is a Python file (or a directory with an` __init__.py` for multi-file plugins) placed in your plugin directory. It defines a function whose signature matches the trigger type:


- Data-write plugin:` def process_writes(influxdb3_local, table_batches, args=None):` </pre>
- Scheduled plugin:` def process_scheduled_call(influxdb3_local, call_time, args=None):` </pre>
- HTTP-request plugin:` def process_request(influxdb3_local, query_parameters, request_headers, request_body, args=None):` </pre>


A trigger is the database resource that connects an event to a plugin. You create it with` influxdb3 create trigger` , specifying a *trigger spec* that defines when the plugin runs.


Whatever the type, every plugin receives` influxdb3_local` which is the shared API object that’s your gateway to the database. The methods we’ll use:


- ` influxdb3_local.query(sql)` - run a SQL query and get results back as a list of dictionaries.
- ` influxdb3_local.write(line)` - write a point back into the database, built with the` LineBuilder` helper.
- ` influxdb3_local.info(msg)` /` .warn(msg)` /` .error(msg)` - log to stdout and the system.processing_engine_logs table.
- The engine also offers an in-memory cache for keeping state between runs (useful for things like tracking a rolling baseline), though we’ll keep our first version stateless.


` LineBuilder` is the recommended way to construct a point inside a plugin:


```text
line = LineBuilder("my_table")
line.tag("device", "pump_7")
line.float64_field("value", 42.0)
line.int64_field("count", 3)
influxdb3_local.write(line)
```


Plugins receive trigger arguments as a` Dict\[str, str\]` in` args` , which is how we’ll pass tunable thresholds without editing code.


## Predictive maintenance plugin logic


We need to turn raw sensor readings into a Remaining Useful Life estimate. A full production system might run a trained LSTM or gradient-boosted model, but a tutorial plugin should be transparent and dependency-light, so we’ll use a degradation-index approach that captures the real idea behind RUL prediction without a black box:


1. Pick the sensors that carry degradation signals. In FD001, several sensors drift steadily as the high-pressure compressor wears. Sensors s_2, s_3, s_4, s_8, s_11, s_13, and s_15 trend upward over an engine’s life; s_7, s_12, s_20, and s_21 trend downward. (Some sensors in FD001 are flat and carry no information—we ignore those.)
2. Normalize each chosen sensor against the healthy-baseline range so they’re comparable, flipping the downward-trending ones so “more degraded” always means “higher.”
3. Average them into a single health index between roughly 0 (factory-fresh) and 1 (failure imminent).
4. Map the index to an RUL estimate. Using the dataset convention that degradation becomes meaningfully predictable in roughly the final 125 cycles, we estimate` RUL ≈ 125 × (1 − health_index)` .
5. Alert when the estimated RUL drops below a configurable threshold.


This is intentionally simple and explainable. The plugin structure is identical whether the scoring function is ten lines of arithmetic or a 50-megabyte neural network, you’d just swap the body of` compute_health_index` .


To normalize, the plugin needs each sensor’s healthy and degraded reference values. Rather than hard-code magic numbers, we’ll derive them once at the top of each run from the fleet itself with the early-life readings approximate “healthy” and the latest readings approximate “degraded.”


## Creating the plugin


Create the plugin file directly in your plugin directory. Save this as` ~/influxdb3/plugins/predictive_maintenance.py` .


```text
# predictive_maintenance.py
#
# Scheduled plugin: estimates Remaining Useful Life (RUL) for each engine
# from the most recent sensor readings, writes the estimates back into the
# database, and raises alerts when RUL drops below a threshold.


# Sensors that rise as the engine degrades
RISING = ["s_2", "s_3", "s_4", "s_8", "s_11", "s_13", "s_15"]
# Sensors that fall as the engine degrades
FALLING = ["s_7", "s_12", "s_20", "s_21"]


# Convention from the C-MAPSS literature: degradation becomes meaningfully
# predictable in roughly the final 125 cycles.
MAX_PREDICTABLE_RUL = 125.0


def _safe_float(value, default=0.0):
try:
return float(value)
except (TypeError, ValueError):
return default


def compute_baselines(influxdb3_local):
"""Derive healthy and degraded reference values for each sensor.


Healthy  ~ average of the earliest cycles across the fleet.
Degraded ~ average of the latest cycles across the fleet.
"""
sensors = RISING + FALLING
avg_cols = ", ".join([f"AVG({s}) AS {s}" for s in sensors])


healthy_rows = influxdb3_local.query(
f"SELECT {avg_cols} FROM sensors WHERE time_cycles = 15"
)
degraded_rows = influxdb3_local.query(
f"SELECT {avg_cols} FROM sensors WHERE time_cycles = 175"
)


if not healthy_rows or not degraded_rows:
return None


healthy = healthy_rows[0]
degraded = degraded_rows[0]


baselines = {}
for s in sensors:
lo = _safe_float(healthy.get(s))
hi = _safe_float(degraded.get(s))
baselines[s] = (lo, hi)
return baselines


def normalize(value, lo, hi):
"""Scale a reading to 0..1 between the healthy (lo) and degraded (hi)
references. Clamps to the [0, 1] range."""
if hi == lo:
return 0.0
frac = (value - lo) / (hi - lo)
return max(0.0, min(1.0, frac))


def compute_health_index(reading, baselines):
"""Combine the signal-bearing sensors into a single 0..1 degradation
index. 0 = healthy, 1 = failure imminent."""
scores = []


for s in RISING:
lo, hi = baselines[s]
scores.append(normalize(_safe_float(reading.get(s)), lo, hi))


for s in FALLING:
lo, hi = baselines[s]
# Falling sensors: degraded value is lower, so invert the scale.
scores.append(1.0 - normalize(_safe_float(reading.get(s)), hi, lo))


if not scores:
return 0.0
return sum(scores) / len(scores)


def process_scheduled_call(influxdb3_local, call_time, args=None):
# --- Read tunables from trigger arguments ---
args = args or {}
rul_threshold = float(args.get("rul_threshold", "30"))
lookback = args.get("lookback", "10m")


influxdb3_local.info(
f"Predictive maintenance run starting. "
f"rul_threshold={rul_threshold}, lookback={lookback}"
)


# --- Build per-sensor baselines from the fleet ---
baselines = compute_baselines(influxdb3_local)
if baselines is None:
influxdb3_local.warn("Not enough data to compute baselines; skipping run.")
return


# --- Get the most recent reading per engine ---
sensor_select = ", ".join(RISING + FALLING)
latest = influxdb3_local.query(
f"""
SELECT unit_number, time_cycles, {sensor_select}
FROM sensors
WHERE time >= now() - INTERVAL '{lookback}'
ORDER BY time DESC
"""
)


if not latest:
influxdb3_local.warn("No recent sensor data in lookback window.")
return


# Keep only the newest row per engine (results are time-desc ordered).
seen = set()
newest_per_engine = []
for row in latest:
unit = row.get("unit_number")
if unit not in seen:
seen.add(unit)
newest_per_engine.append(row)


alerts = 0
for row in newest_per_engine:
unit = str(row.get("unit_number"))
cycles = int(_safe_float(row.get("time_cycles")))


health = compute_health_index(row, baselines)
est_rul = round(MAX_PREDICTABLE_RUL * (1.0 - health), 1)


# --- Write the RUL estimate ---
est = LineBuilder("rul_estimates")
est.tag("unit_number", unit)
est.float64_field("health_index", round(health, 4))
est.float64_field("estimated_rul", est_rul)
est.int64_field("time_cycles", cycles)
influxdb3_local.write(est)


# --- Raise an alert if the engine is in the danger zone ---
if est_rul = rul_threshold:
alerts += 1
severity = "critical" if est_rul = rul_threshold / 2 else "warning"


alert = LineBuilder("maintenance_alerts")
alert.tag("unit_number", unit)
alert.tag("severity", severity)
alert.float64_field("estimated_rul", est_rul)
alert.float64_field("health_index", round(health, 4))
influxdb3_local.write(alert)


influxdb3_local.warn(
f"[{severity.upper()}] Engine {unit}: estimated RUL "
f"{est_rul} cycles (health index {round(health, 2)}). "
f"Schedule maintenance."
)


influxdb3_local.info(
f"Run complete. Scored {len(newest_per_engine)} engines, "
f"raised {alerts} alert(s)."
)
```


Let’s walk through the important parts.


- **Baselines from the data help us avoid any magic numbers.** -` compute_baselines` asks the database for the average sensor values during early life (cycles ≤ 15, “healthy”) and late life (cycles ≥ 175, “degraded”). This adapts automatically to your fleet and means you don’t have to know the absolute scale of s_4 in advance.
- **A single, explainable health index.**` compute_health_index` normalizes each signal-bearing sensor onto a 0–1 scale and averages them. Rising and falling sensors are both oriented so that 1 always means “more degraded.” This is the piece you’d replace with a trained model in production — the surrounding plumbing stays identical.
- **Newest reading per engine.** The query pulls everything in the lookback window ordered newest-first, then we keep the first row we see for each` unit_number` . Because of how we loaded the data (each engine’s last cycle anchored to “now”), the most recent rows are the most degraded.
- **Writing conclusions back.** Every engine gets a row in` rul_estimates` . Engines past the threshold also get a row in` maintenance_alerts` , tagged with a` severity` derived from how deep into the danger zone they are, plus a logged warning you’ll see in the server terminal.
- **Configurable via trigger arguments.**` rul_threshold` and` lookback` come from` args` , so you can retune behavior by recreating the trigger.


## Creating the trigger


Now connect the plugin to a schedule. We’ll run it every minute, with a 30-cycle RUL alert threshold and a 10-minute lookback window:


```text
influxdb3 create trigger \
--trigger-spec "every:1m" \
--path "predictive_maintenance.py" \
--trigger-arguments "rul_threshold=30,lookback=10m" \
--database engine_fleet \
pdm_scheduler
```


Breaking that down:


- ` --trigger-spec "every:1m"` runs the plugin once a minute. You could also use` cron:` for calendar schedules, for example` cron:0 0 8 * * *` for 8am daily (the format includes seconds).
- ` --path "predictive_maintenance.py"` is the filename relative to your plugin directory. (For a multi-file plugin you’d point this at the directory containing` __init__.py` instead.)
- ` --trigger-arguments` passes our tunables as key=value pairs.
- ` pdm_scheduler` is the trigger’s name.


The trigger is enabled by default and starts running on the next interval boundary.


## Testing out the plugin


Within a minute, the plugin fires. Check the server terminal and you’ll see the` info` and` warn` log lines. Now query what it produced.


The latest RUL estimates, most-degraded first:


```text
influxdb3 query --database engine_fleet \
"SELECT unit_number, estimated_rul, health_index, time_cycles \
FROM rul_estimates \
ORDER BY time DESC, estimated_rul ASC LIMIT 15"
```


The maintenance alerts:


```text
influxdb3 query --database engine_fleet \
"SELECT time, unit_number, severity, estimated_rul, health_index \
FROM maintenance_alerts \
ORDER BY time DESC LIMIT 20"
```


If you want to confirm the plugin is registered and see its file details:


```text
influxdb3 show plugins
```


And the engines flagged critical right now:


```text
influxdb3 query --database engine_fleet \
"SELECT unit_number, estimated_rul FROM maintenance_alerts \
WHERE severity = 'critical' ORDER BY estimated_rul ASC"
```


You should see a spread of RUL estimates across the fleet, with the most-degraded engines surfacing as warnings or criticals.


## Inspecting logs and iterating


Plugin logs go both to the server’s stdout and to a system table, which is handy for debugging without scrolling the terminal:


```text
influxdb3 query --database engine_fleet \
"SELECT * FROM system.processing_engine_logs ORDER BY time DESC LIMIT 20"
```


When you change the plugin code, you don’t need to recreate the trigger. Edit the file and push the update, preserving the trigger’s configuration and history:


```text
influxdb3 update trigger \
--database engine_fleet \
--trigger-name pdm_scheduler \
--path "predictive_maintenance.py"
```


For local development you can also develop a plugin on your own machine and upload it with` --upload` when creating or updating a trigger, which copies the file to the server for you (this requires an admin token). And if you want to dry-run a scheduled plugin without waiting for the interval, there’s` influxdb3 test schedule_plugin` .


To pause the system without deleting anything:


```text
influxdb3 disable trigger --database engine_fleet pdm_scheduler
# ...and later...
influxdb3 enable trigger --database engine_fleet pdm_scheduler
```


#### Error Handling


By default, plugin errors are logged and the trigger keeps going. For a critical pipeline you might prefer automatic retries or auto-disable. Set this when creating the trigger with` --error-behavior retry` or` --error-behavior disable` (the default is log).


## What to build next


You now have a complete, self-contained predictive maintenance loop running inside InfluxDB 3 Core. Some natural extensions:


- **Swap in a real model.** Replace` compute_health_index` with a trained regressor or classifier. Install the library with` influxdb3 install package` , load your serialized model at the top of the plugin, and predict per engine. The trigger, the writes, and the alerts don’t change.
- **Add a notifier.** Have the alert branch call an external service like Slack, PagerDuty, email directly from Python. InfluxData also publishes an official notifier plugin you can compose with.
- **Use the engine’s cache for stateful detection.** Track a rolling baseline or a per-engine trend slope across runs instead of recomputing fleet baselines each time, using the in-memory cache to persist state between executions.
- **Lower the latency.** Move from a scheduled trigger to a data-write trigger if you need to react the moment a reading crosses a line.
- **Browse the official plugin library.** InfluxData maintains a public repository of plugins (anomaly detection via MAD, threshold/deadman checks, Prophet forecasting, downsampling, and more) that you can reference directly in a trigger with the` gh:` prefix, or copy and adapt.


The broader point is the pattern: with the InfluxDB 3 Processing Engine, the intelligence lives in the database, next to the data, reacting as the data moves. For time-series-heavy domains like industrial IoT and predictive maintenance, that proximity is exactly what you want.
