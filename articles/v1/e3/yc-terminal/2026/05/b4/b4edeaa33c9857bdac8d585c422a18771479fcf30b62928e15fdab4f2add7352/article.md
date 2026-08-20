---
schema_version: "1.0.0"
document_id: "b4edeaa33c9857bdac8d585c422a18771479fcf30b62928e15fdab4f2add7352"
company_key: "yc-terminal"
company: "Terminal"
source_id: "yc-terminal-rss-78690d64b189"
canonical_url: "https://docs.withterminal.com/changelog"
published_at: "2026-05-08T12:06:49+00:00"
first_seen_at: "2026-07-26T01:53:49.682680+00:00"
fetched_at: "2026-08-20T02:47:00.006608+00:00"
content_hash: "sha256:e061a62b1121daf8b3b6ddbb4f0a529f1121b969ea21b4c613ec50f95f360110"
---

# 2026-05-08

### Common Model Updates


- Expanded[Vehicle](https://docs.withterminal.com/models/vehicle)` fuelType` values so additional alternative fuel and plug-in hybrid types are now preserved end-to-end


### New Integrations


- [New ELD World](https://docs.withterminal.com/providers/tsp/new-eld-world)
- [Gorilla Safety](https://docs.withterminal.com/providers/tsp/gorilla-safety)
- [Fleet Track](https://docs.withterminal.com/providers/tsp/fleet-track)
- [HCSS eLogs](https://docs.withterminal.com/providers/tsp/hcss)
- [Stellar ELD](https://docs.withterminal.com/providers/tsp/stellar-eld)


### Integration Updates


- [Loop ELD](https://docs.withterminal.com/providers/tsp/loop-eld) now supports[Devices](https://docs.withterminal.com/models/device)
- [Rigbot](https://docs.withterminal.com/providers/tsp/rigbot) now supports[Historical Vehicle Locations](https://docs.withterminal.com/models/vehicle-location)
- [IntelliShift](https://docs.withterminal.com/providers/tsp/intelli-shift) now supports[Fault Code Events](https://docs.withterminal.com/models/fault-code-event) and improved active driver status syncing
- [Motive](https://docs.withterminal.com/providers/tsp/motive) now supports additional[Vehicle Stat Logs](https://docs.withterminal.com/models/vehicle-stat-log) , including` battery_voltage` ,` fuel_level` , and` engine_runtime`
- [Omnitracs ES](https://docs.withterminal.com/providers/tsp/omnitracs-es) now supports` engine_state` in[Vehicle Stat Logs](https://docs.withterminal.com/models/vehicle-stat-log)
- [Zonar](https://docs.withterminal.com/providers/tsp/zonar) now supports` engine_runtime` in[Vehicle Stat Logs](https://docs.withterminal.com/models/vehicle-stat-log)
- [Geotab](https://docs.withterminal.com/providers/tsp/geotab) now supports trip backfills older than 13 months


### Documentation Updates


- Added a new guide for[deleting connections](https://docs.withterminal.com/guides/deleting-connections)
- Updated[Loop ELD documentation](https://docs.withterminal.com/providers/tsp/loop-eld) with vehicle location sample rate and retention details
