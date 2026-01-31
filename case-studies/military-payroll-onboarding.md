# Military Payroll Account Onboarding (Mobile Web) — Dual Issuance Modes + Card Handoff

> Public-safe case study. Internal IDs/URLs/protocol names are omitted.  
> Focus: complex eligibility validation, staged rollout readiness, partner integration, and reliability under high expected traffic.

## Overview
A mobile-web onboarding launch for a **military payroll account**, designed for incoming recruits.  
The program is high-impact because it targets a large population and requires a reliable issuance flow under tight timelines and strict eligibility rules.

This onboarding was built to support:
- **Dual issuance modes** (pre-issuance vs. on-site issuance via QR/kiosk)
- **Eligibility verification** via external authority/partner systems
- A smooth **handoff from account opening to military card issuance**
- **Exceptions** to standard retail banking policies (e.g., cooling-off restrictions, account type rules)

- **Timeline:** late 2025 build → Dec 2025 release → staged rollout in **Jan 2026**
- **Stack:** Java (Spring/Spring Boot), React (TypeScript), integrations, monitoring/logging

## My Role
- Owned end-to-end onboarding delivery on the mobile web channel (front-end heavy + orchestration touchpoints).
- Implemented a dedicated UI flow (separated from standard deposit onboarding due to unique steps and constraints).
- Built/extended eligibility validation and integration behavior to support dual issuance modes.
- Coordinated cross-team requirements across internal platform/core teams and an external eligibility authority.
- Led edge-case discovery and fixes prior to go-live to reduce launch risk.

## What Made It Hard
- **High expected volume** and strict timeline (must be ready for the next intake cycle).
- Eligibility and policy constraints required **many exception rules**, not the standard retail path:
  - exceptions to cooling-off restrictions (anti-fraud policy) for payroll onboarding
  - ensure issuance results in a **normal account** (not a limited account) due to payroll use
- **Designated location selection:** eligible “branches” are restricted (e.g., only certain designated offices).
- Integration depended on an external authority/partner interface; failures had to be handled safely and clearly.
- Testing was challenging due to limited data for edge-case reproduction.

## Rollout Modes (Dual Issuance)
Two modes were implemented to reduce on-site time and handle different entry points:

### Mode A — Pre-issuance (at home)
- Recruits complete most steps before arriving on site.
- Completion data is stored and later re-used on the day-of visit to reduce repeated steps.

### Mode B — On-site issuance (QR/Kiosk)
- Recruits start from a QR/kiosk entry point on the day-of.
- The flow validates eligibility and continues to issuance with fewer manual steps.

> Internally, the two modes are routed by a mode parameter (sanitized); implementation supports shared steps + mode-specific branches.

## High-Level Architecture (Sanitized)
**Entry Point (Pre / QR) → Mobile Web UI → Onboarding API → Eligibility Authority Integration → Core/Shared Services → Card Handoff**

Key components:
- **Eligibility verification adapter:** verifies recruit eligibility via external interface.
- **Policy/exception gate:** applies rules (cooling-off exceptions, account type constraints, location constraints).
- **Onboarding orchestration:** coordinates the issuance steps in correct order with safe failure handling.
- **Designated location selection module:** limits selectable locations to approved list.
- **Card handoff module:** upon eligible completion, redirects/bridges into the military card issuance journey.
- **Observability:** monitoring/logging and error-code/message alignment to support operations during rollout.

## User Journey (Simplified)
1) Entry (Pre-issuance link or QR/kiosk)  
2) Eligibility check + policy gate (including exception rules)  
3) Designated location selection (restricted list)  
4) Account issuance steps (mode-specific)  
5) Completion + **handoff to card issuance** (card-only issuance supported when account already exists)

> Product nuance: **card issuance may occur without opening a new account** if the user already has an existing account to link.

## Testing Strategy & Edge-Case Discovery
Because real-world edge cases were difficult to reproduce with limited datasets, we used:
- targeted scenario design for exception rules (cooling-off, account-type constraints, designated locations)
- integration testing with the external authority interface under different response conditions
- “negative testing” to validate safe failure behavior and clear error messages

### Example: High-impact edge case found before launch
Discovered a conflict where a separate eligibility path (from another student-targeted product) unexpectedly blocked military card issuance under certain conditions.  
Fixed the rule handling before rollout, preventing a high-visibility onboarding failure.

## Operational Readiness (Staged Rollout Plan)
We treated rollout as a staged operation rather than a single “big bang” release:
- **Jan 2026:** pre-issuance start → kiosk start → general issuance start (staged)
- Early signals include eligibility check behavior, step failure concentration, and handoff success rate.
- Post-launch focus: fast triage, partner-facing error alignment, and guardrails to prevent recurrence.

## What I’m Proud Of
- Delivered a complex, validation-heavy onboarding flow with dual issuance modes under tight schedule and high launch risk.
- Separated and redesigned the flow when necessary to avoid forcing a “standard” pipeline onto a non-standard product.
- Proactively found and fixed a critical edge case before it reached real users.
- Built the foundation for staged rollout monitoring and rapid operational response.

## Key Takeaways
- Complex onboarding is about managing *exceptions* as first-class logic, not just happy-path UI.
- Staged rollout reduces risk when eligibility and external integrations are involved.
- “Card handoff” is part of the product, not a redirect; treating it as a core step improves reliability.

## What I’d Improve Next
- Add a structured dashboard for **drop-off by step**, **eligibility failure reasons**, and **handoff success rate**.
- Centralize exception rules into reusable modules to reduce future branching.
- Add contract tests and alerting around external eligibility interface changes.
