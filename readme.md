<!--
System Design Framework: README
Fill in sections marked with [TBD].
-->

# <img src="assets/favicon.png" alt="Icon" width="28" height="28" /> ESP (Email-Service-Provider) System Design

## Problem Statement
We're going to provider an ESP which is going to be use by thousands of users to handle milions of emails daily, these users are going to use their credits to send different types of emails such OTP, bulk and simple emails.
Each email might have several attachments, and maximum attachment size is 150MB for each attachment.
we have several email providers like mailgun which have different latencies and costs and quotas which must be considerd while selecting target provider.
delivery report is mandatory and retrying is also a key feature for emails, sepcifically OTP emails.
low latencies for OTP emails is really important and should be considered.
since each customer have it's own rate limit and qouta, each provider also use this policy for us to use them.
providers might be down, not responding and cost heavily, we should always consider these parameters as key important parameters to select our provider and reduce costs.
due to high load for different periods of time like black friday and other holidays, different compaigns might be run by customers which increase the load much more than regular days, we should be able to scale and able to handle them.
bulk emails is another feature which is going to be used by our customers, we must be able to handle them.

## Requirements Summary
### Functional Requirements (High Level)
- 150M daily emails
- attachments should be considered
- providers should be easily added and removed
- bulk send
- report and analytics is important

### Non-Functional Requirements (High Level)
- Latency should be at minimum for OTP emails.
- Availability of service is a crucial factor
- latency of providers are different
- maximum size of attachments should be 150MB
- system shoud be able to handle AVG(150 milions) of daily email
- system should be able to scale for peak times like holidays
- system should gurantee delivery of at-least-once

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
