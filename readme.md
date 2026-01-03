<!--
System Design Framework: README
Fill in sections marked with [TBD].
-->

# <img src="assets/favicon.png" alt="Icon" width="28" height="28" /> ESP (Email-Service-Provider) System Design

## Problem Statement
Design an Email Service Provider (ESP) that supports thousands of customers sending millions of emails per day across different email types (OTP, bulk, and standard transactional). Messages may include multiple attachments, with a maximum size of 150MB per attachment. The system must route email through multiple third-party providers with varying latency, cost, and quota constraints while ensuring delivery reporting, retries (especially for OTP), and low latency for time-sensitive messages. Each customer has its own rate limits and quotas, and provider availability or cost spikes must be handled gracefully. The platform must scale to seasonal spikes (e.g., holiday campaigns) without sacrificing reliability or delivery guarantees.

## Requirements Summary
### Functional Requirements (High Level)
- Support up to 150M emails per day across OTP, bulk, and standard transactional sends.
- Allow multiple attachments per email, up to 150MB per attachment.
- Integrate multiple providers and enable easy onboarding/removal of providers.
- Provide delivery reports and analytics.
- Support bulk email sends at scale.

### Non-Functional Requirements (High Level)
- Minimize latency for OTP emails.
- Maintain high availability despite provider outages or degraded performance.
- Handle differing provider latencies, quotas, and costs during routing decisions.
- Enforce 150MB maximum attachment size per attachment.
- Sustain an average throughput of ~150M emails per day.
- Scale for peak periods such as holiday campaigns.
- Guarantee at-least-once delivery.

## Goals and Non-Goals
### Goals
- [TBD] What success looks like for this system.

### Non-Goals
- [TBD] Explicitly exclude features or use cases.

## Scope and Assumptions
### Scope
- [TBD] In-scope features for this iteration.

### Assumptions
- [TBD] Known constraints and business assumptions.

## Stakeholders
- [TBD] Product, engineering, legal/compliance, operations, users.

## System Overview
- [TBD] 2-4 sentences summarizing the architecture at a high level.

## Architecture at a Glance
- [TBD] Bullet list of primary components and their roles.

## Key Workflows
- [TBD] 3-5 user or system flows (e.g., "User signup", "Create order").

## Data Model Summary
- [TBD] Key entities and relationships.

## Scale and Capacity
- [TBD] Expected DAU/MAU, peak RPS, data size, growth assumptions.

## Constraints and Risks
- [TBD] Regulatory constraints, budget, legacy integrations, risk areas.

## Open Questions
- [TBD] Items needing clarification.

## References
- [TBD] Links to docs, tickets, or external constraints.
