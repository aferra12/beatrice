---
name: dhh-plan-review
description: Review a plan through DHH's lens of pragmatism, shipping, and operational simplicity
---

# DHH's Plan Review

Evaluate this plan through the following criteria. For each, state your finding and cite specific parts of the plan that support your assessment.

## Pragmatism Check

Is this plan solving a real, present user problem — or an imagined scaling problem? Look for language like "future-proof," "when we scale to," or "in case we need to." These are warning signs that the plan is optimizing for a hypothetical future instead of shipping something useful today. Real plans start from real pain. Ask: whose life gets better when this ships, and how soon?

## Over-Engineering Detection

What can be removed from this plan and still deliver a useful product? Apply "build less" ruthlessly. Every feature, integration, and abstraction layer has a cost. The first version should be the smallest thing that solves the core problem. If the plan reads like a comprehensive system design document rather than a path to a first deploy, it is over-engineered. Flag anything that smells like premature optimization — caching layers before there is traffic, message queues before there is load, event sourcing before there is a second consumer.

## Convention Over Configuration

Are there decisions in this plan that a sensible default could eliminate? Every time the plan says "we need to decide" or "we will evaluate options," that is a configuration point that could be a convention. The best plans pick a stack, pick a pattern, and move on. Time spent choosing between Kafka and RabbitMQ is time not spent shipping. If a well-known framework or tool provides a default answer, use it.

## Small Team Viability

Could a team of three to five people build, ship, and maintain what this plan describes? If the plan implicitly requires a platform team, a DevOps team, a separate frontend team, and a backend team, the plan is designing for an organization that probably does not exist. Good plans are honest about the team they have, not the team they wish they had. Count the number of distinct specializations required — if it exceeds the team size, the plan needs to shrink.

## Time to First Deploy

How many days or weeks until something real is running in production and in front of users? This is the single most important metric for a plan. Plans with long lead times before any deployment are plans that accumulate risk and untested assumptions. Look for a credible path to a first deploy within one to two weeks. If the plan has a three-month "foundation phase" before anything ships, push back hard.

## Operational Simplicity

How many distinct services, databases, queues, caches, and external dependencies need to be running, monitored, and coordinated for this plan to work? Each is a potential failure point, an on-call burden, and a deployment coordination cost. A plan that requires Kubernetes, three databases, a message broker, a cache layer, and four microservices for a product that has not yet found product-market fit is a plan that values architecture over outcomes.

## Complexity Theater

Is any part of this plan complexity for the sake of complexity? Look for patterns adopted because they are fashionable rather than because they solve a demonstrated problem. Event-driven architecture without an event-driven problem. CQRS without meaningfully different read and write patterns. Domain-driven design terminology applied to a CRUD application. GraphQL for a single client. These are not inherently wrong, but they demand justification that starts with the specific, concrete problem they solve — not the blog post that inspired them.

## Output Format

For each criterion, provide a clear assessment: what the plan does well, what concerns you, and what you would change. End with an overall verdict: ship it, simplify it, or rethink it. Be direct and specific — quote the plan where possible.
