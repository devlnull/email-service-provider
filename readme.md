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

## Back-of-the-envelope calculations
Capture early capacity estimates so the rest of the design is grounded in numbers.

- **Inputs to estimate**: target geography, DAU/MAU, average sessions per user per day, average session length, requests per session, read/write ratio, payload sizes, data retention window.
- **Core traffic metrics**:
  - **DAU**: `~10000`
  - **TPS**: `(10000 * 15000) / 86,400 = ~1750`
  - **QPS**: `(10000 * 1000) / 86,400 = ~115`
  - **Peak TPS**: `avg TPS * peak_factor -> 1750 x 5 = ~8750`
- **Data volume**:
  - **Average attachment size**: `5 MB`
  - **Avrage emails with attachments**: `20% of 150M = 30M`
  - **Daily storage**: `30M * 5MB = ~1.5 TB`
- **Other factors to call out**: latency targets, availability SLA, fanout amplification, cache hit rate, and burst handling strategy.

## Goals and Non-Goals
### Goals
- Provide a reliable multi-tenant ESP that routes traffic across providers to meet latency, cost, and quota targets.
- Deliver OTP emails with low latency while sustaining high-volume bulk and transactional throughput.
- Offer delivery tracking, retries, and analytics with at-least-once delivery guarantees.
- Scale elastically for peak campaigns without degrading reliability.

### Non-Goals
- Building a full marketing automation suite (journeys, CRM, lead scoring).
- Operating an MTA from scratch; leverage third-party providers.
- End-user mailbox features (inbox UI, storage, spam filtering).

## Scope and Assumptions
### Scope
- Email ingestion APIs for OTP, bulk, and transactional sends.
- Provider routing with quotas, rate limits, and health-aware failover.
- Attachment storage and retrieval with per-attachment size enforcement.
- Delivery events, status tracking, and reporting dashboards.

### Assumptions
- Tenants have pre-configured providers, credentials, and default routing policies.
- Attachments are stored in object storage and referenced by ID during send.
- Provider webhooks are available for delivery, bounce, and complaint events.
- Data retention policies allow at least 30-90 days of event history.

## Stakeholders
- Product management, platform engineering, SRE/operations, security/compliance, and customer success.
- External email providers and enterprise customers with SLAs.

## System Overview
- The system exposes a multi-tenant API for email ingestion, validates requests, stores attachments, and enqueues messages for delivery.
- A routing service selects providers based on latency, cost, quotas, and health while honoring tenant policies.
- Async workers send emails via provider adapters and emit delivery events to reporting and analytics pipelines.

## Architecture at a Glance
- Client SDKs and partner integrations for email submission.
- API gateway for auth, rate limiting, and request validation.
- Core services: ingestion, routing, scheduling, and provider adapters.
- Data stores for tenant config, message metadata, delivery events, and attachments.
- Messaging layer with queues/streams for send and event processing.

## Key Workflows
- Submit OTP email: ingest, validate, enqueue, route, send, track delivery.
- Submit bulk campaign: ingest batch, schedule, throttle per tenant, send, aggregate metrics.
- Provider failover: detect errors, reroute to backup provider, retry with idempotency keys.
- Delivery events: ingest webhooks, normalize, update status, publish analytics.

## Data Model Summary
- Tenant, ProviderConfig, RoutingPolicy, and RateLimit for configuration.
- Message, Recipient, Attachment, and SendAttempt for delivery tracking.
- DeliveryEvent and Metric aggregates for reporting.

## Scale and Capacity
- 150M emails per day average, with 3-5x peaks during campaigns.
- Peak ingestion in the tens of thousands of requests per second with burst control.
- Event volumes proportional to sends (delivery, bounce, complaint, open, click).

## Constraints and Risks
- Provider outages and throttling require fast failover and backpressure.
- Compliance obligations for PII, opt-out, and regional data residency.
- Large attachments increase storage and egress cost risk.

## Open Questions
- Exact SLAs for OTP latency and delivery success rates.
- Required data retention periods and deletion workflows per tenant.
- Provider selection policy and cost optimization strategy ownership.

## References
- `high-level-design.md`
- `functional-requirements.md`
- `nonfunctional-requirements.md`
