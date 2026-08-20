---
schema_version: "1.0.0"
document_id: "e2e5caef15d48a6732816c323f18839282f310079471c75dad9f8d2aca66619a"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/scaling-development-environments-with-microservices/"
published_at: "2021-06-21T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:7c38bc11cbcc64c1e3c8b0dc0a9125129d4218de589f74836a8c644dbc131b37"
---

# Scaling Development Environments with Microservices

Developers spend most of their time coding and verifying code in their favorite integrated development environment (IDEs) and local terminals. This was simple when the entire application could easily be run locally with all its dependencies including databases. This is no longer the case for modern cloud native software which usually involves deploying with many services, cloud and 3rd party dependencies.


## Development Environments for Microservices


Microservices based architecture enables decomposition of a software application into small “services” that communicate using well defined interfaces. Though the term “Microservice” lacks a precise definition, the high level idea is borne out of the need to enable teams and developers to move independently.


In such an ecosystem, developers seek productive development environments but face difficult trade offs in terms of feedback speed and quality. They could optimize for speed (e.g by developing locally using mocks of dependent services) at the expense of quality or they could optimize for higher quality feedback (e.g by relying on a high-fidelity pre-production environment) at the expense of speed.


Isolation in these environments is also key. Relying on a shared dev environment slows developers down as it’s challenging to keep such environments stable.


One approach, to provide isolated environments, is to stamp out a full copy of the environment for every developer (as shown below in the diagram). However, this approach comes with significant management overhead and is not cost effective at scale.


Stamping out copies of an Environment


## Collaboration is Key


As software development teams get more distributed, developers need the ability to move forward independently AND collaborate asynchronously.


Microservices based software architectures enable splitting up the codebase into a manageable number of services that can be independently released. However, getting feedback on code changes early and collaborating on product features that span multiple services is still challenging.


## Introducing Signadot Sandboxes


What if developers could have their cake and eat it too? Could we have environments with the right isolation model that provide fast and high-quality feedback during the development process? It turns out the answer is a Yes(!) and we are excited to be building such a solution at Signadot!


Signadot “Sandboxes” (as shown in the figure below) is a Kubernetes based solution that provides ephemeral environments, that are quick to spin up, for every code change. Sandboxes integrate with source code hosting services like GitHub and provides an environment for every Pull Request. Sandboxes are created through a command line interface and accessed via Endpoint URLs hosted by Signadot.


Endpoint URLs can be shared with stakeholders for fast feedback and rapid product iteration. Moreover, developers across teams can share a single Sandbox to collaborate on product features that span multiple, independent PRs.


Signadot is installed on an existing pre-production Kubernetes cluster that has a baseline version of services continuously deployed. This enables high quality feedback during development and ensures testing of code changes against the latest, stable baseline.


A unique multi-tenant architecture enables sharing of the baseline version of services between Sandboxes and makes it cost effective to scale to 100s of Microservices.


## The Road Ahead


We are just getting started and here are few things that we’re very excited about:


- Debuggability: Debug test failures by providing greater visibility into Sandboxes
- Enhanced Endpoints: Host endpoints to configurable service entry points
- DB Schema changes: Manage lifecycle of ephemeral DB instances tied to Sandboxes


Stay tuned as we share more around our Product Roadmap and Technical Architecture.


## Schedule a Demo


Sign up on our[website](https://www.signadot.com/) to schedule a Demo or check out our[pricing page](https://www.signadot.com/pricing/) to learn more.


## We are Hiring


If you are a backend engineer with a passion for building cloud native infrastructure, we would love to hear from you! Email us atcareers@signadot.com .


‍
