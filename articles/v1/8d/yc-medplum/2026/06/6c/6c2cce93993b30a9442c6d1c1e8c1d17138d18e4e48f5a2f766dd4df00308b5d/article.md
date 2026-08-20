---
schema_version: "1.0.0"
document_id: "6c2cce93993b30a9442c6d1c1e8c1d17138d18e4e48f5a2f766dd4df00308b5d"
company_key: "yc-medplum"
company: "Medplum"
source_id: "yc-medplum-atom-baaaecda9acc"
canonical_url: "https://www.medplum.com/blog/scheduling-beta"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.588032+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:1981b464bc4ad75b17bd34c2aa474ce21f9b3917ef226e4dcba5315dceec6da4"
---

# Medplum Scheduling API - Beta Release

After several months of preview as an[Alpha](https://www.medplum.com/docs/compliance/alpha-beta) product, we are pleased to announce that[Medplum's Scheduling API](https://www.medplum.com/docs/scheduling) has graduated to[Beta](https://www.medplum.com/docs/compliance/alpha-beta) .


Medplum's Scheduling API provides an interface for applications to atomically interact with[Schedule](https://www.medplum.com/docs/api/fhir/resources/schedule) ,[Slot](https://www.medplum.com/docs/api/fhir/resources/slot) , and[Appointment](https://www.medplum.com/docs/api/fhir/resources/appointment) resources to safely create and manage bookings without creating scheduling conflicts. Developing a feature like this required the ability to iterate as we learned, and our Alpha period provided exactly that.


## What changed during Alpha​


We made some breaking changes over the course of the Alpha period to simplify the API surface and support multi-resource scheduling:


- **Removed` Schedule/:id/$find`** : This route has been replaced by` Appointment/$find` , which handles single-Schedule lookups as a basic case of its ability to query multiple Schedules at once.
- **Removed slot-based` Appointment/$book`** : The` slot` parameter input to` $book` has been removed. The operation now only accepts an` appointment` as input, which can act as a container for the potentially many Slot resources involved in a single atomic booking operation.
- **` HealthcareService` as required service type** : Our original specification considered using[ActivityDefinition](https://www.medplum.com/docs/api/fhir/resources/activitydefinition) resources as an optional place to store shared service type definitions. After considering the impact it would have when booking appointments with several participants, we decided to require that service types be explicitly defined as[HealthcareService](https://www.medplum.com/docs/api/fhir/resources/healthcareservice) resources, and a reference to such a healthcare service must be included to search for available appointments in` $find` .
- **Changed` availability` format** : We've chosen a storage format for customizing availability that mirrors the[FHIR R5 Availability type](https://hl7.org/fhir/R5/metadatatypes.html#Availability) . This will let us migrate these definitions easily when Medplum Server adds support for later FHIR releases.


## The Beta API surface​


The Beta API consists of five operations:


Operation Description


[Appointment/$find](https://www.medplum.com/docs/scheduling/appointment-find) Find available slots across one or more Schedules


[Appointment/$book](https://www.medplum.com/docs/scheduling/appointment-book) Book an appointment in one step


[Appointment/$hold](https://www.medplum.com/docs/scheduling/appointment-hold) Temporarily hold a slot


[Appointment/$confirm](https://www.medplum.com/docs/scheduling/appointment-confirm) Confirm a held appointment


[Appointment/$cancel](https://www.medplum.com/docs/scheduling/appointment-cancel) Cancel an appointment


## What Beta means​


The core API contract is now stable. Breaking changes are still possible during Beta, but we will note them in release notes and provide a migration path where practical. The API is appropriate for production use in non-critical workflows. See our[Alpha & Beta policy](https://www.medplum.com/docs/compliance/alpha-beta) for the full details.


## Get started​


Check out the[Scheduling documentation](https://www.medplum.com/docs/scheduling) for a full reference, including how to define availability, find open slots, and book appointments.


Thank you to all our partners who experimented with the API during the Alpha period and provided such valuable feedback!
