---
name: dhh-architecture-review
description: Review architecture through DHH's lens of monolith-first, integrated systems, and team-size fit
---

# DHH's Architecture Review

Evaluate this architecture through the following criteria. For each, state your finding and cite specific architectural decisions that support your assessment.

## Monolith-First

Is the system distributed because actual scaling needs demand it, or is distribution a premature architectural choice? The default should be a single, well-structured application. Distribution turns every function call into a network call with latency, failure modes, serialization costs, and versioning headaches. The burden of proof is on the person proposing to split things apart. If the system has fewer than fifty developers working on it, or fewer than a million users hitting it concurrently, a monolith almost certainly suffices. If services exist, ask: what concrete, measured problem did each service boundary solve?

## Integrated Systems

Are components split into separate services that should be one application? The most common architectural mistake is premature decomposition — separating authentication, billing, notifications, and the core product into independent services before the boundaries are well understood. This creates coordination overhead, deployment coupling, data consistency challenges, and debugging nightmares. If two services share a database, they are not really separate. If they must be deployed together, they are not really independent. Look for services that are separate in name only.

## Operational Simplicity

Count everything that must be running for the system to serve a single user request: application servers, databases, caches, message brokers, search engines, API gateways, load balancers, sidecars, service meshes. Each is an operational burden — monitoring, alerting, capacity planning, upgrade cycles, failure recovery. A system that requires Kubernetes, Redis, Elasticsearch, RabbitMQ, and three PostgreSQL instances to serve a CRUD application is an architecture that has lost the plot. Ask: what is the minimum infrastructure that could serve this product well?

## Vertical Scaling

Has vertical scaling been exhausted before going horizontal? A single modern server can handle extraordinary workloads. Before adding load balancers, sharding databases, or introducing horizontal auto-scaling, ask whether a bigger machine would solve the problem for a fraction of the operational complexity. Most applications will never outgrow a single beefy server with a good database. The economics of vertical scaling are dramatically simpler — one machine to monitor, one machine to back up, one machine to secure.

## Dependency Count

How many external services, libraries, and tools does this architecture depend on? Every dependency is a liability — it can break, change, raise prices, get acquired, or go away. Count the third-party APIs, managed services, and framework dependencies. Is each one justified by a specific, substantial benefit, or is it there because someone read a blog post? Could any of these be replaced by a simpler in-house solution? A dependency you own is a dependency you control.

## Team Size Fit

Does this architecture match the team that actually has to build, ship, and maintain it? An architecture designed for a hundred-person engineering organization is a burden on a team of ten. Microservices require dedicated platform engineering. Event-driven systems require specialized debugging skills. Polyglot architectures require expertise in every language in the stack. If the team has to hire specialists just to operate the architecture, the architecture is too complex for the team. Design for the team you have.

## Ownership Cost

What does it cost per month and per year to keep this architecture running — in cloud spend, in developer time for maintenance, in on-call burden, and in cognitive overhead? Is that cost justified by the value the product delivers? Many teams spend more on infrastructure than they earn in revenue. Every managed service has a price tag. Every custom service has a maintenance cost. Add it up and ask: is this the simplest, cheapest architecture that could deliver the product well? If not, what is the cost of the extra complexity, and who is paying for it?

## Output Format

For each criterion, provide a clear assessment with specific evidence from the architecture. End with an overall verdict: whether this architecture is right-sized for its team and product, or whether it should be simplified. Identify the single highest-leverage architectural change. Be direct — name specific services, dependencies, or decisions that should be questioned.
