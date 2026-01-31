# Partner-Integrated Sole-Proprietor Account Onboarding (Mobile Web)

> Public-safe case study. Internal IDs/URLs/protocol names are omitted.  
> Focus: end-to-end onboarding delivery, partner integration, and post-launch reliability.

## Overview
A mobile-web onboarding launch for a **sole-proprietor (business) deposit account** created in collaboration with an external partner platform.
The goal was to let eligible sole proprietors complete account opening through a partner entry point, while meeting strict eligibility/compliance requirements.

- **Timeline:** Jul–Sep 2025 (build), Oct 2025 (launch)
- **Result:** **2,856 new accounts within 3 months** (partner-integrated onboarding)
- **Stack:** Java (Spring/Spring Boot), React (TypeScript), Partner APIs, Monitoring/Logs

## My Role
- Owned the mobile-web onboarding implementation with a teammate (2-person dev team).
- Built/extended backend orchestration services and implemented/updated React UI screens.
- Coordinated with the partner engineering team for token exchange, callbacks, and operational fixes.
- Led post-launch monitoring and rapid fixes based on real production behavior.

## What Made It Hard
- Multi-step onboarding with many gate checks (eligibility, identity verification, compliance steps).
- Partner entry flow required secure context exchange and reliable completion callback.
- Sole-proprietor onboarding introduced additional business/persona requirements and edge cases.
- Needed to stabilize a recently rebuilt React codebase while delivering a new product.

## High-Level Architecture (Sanitized)
**Partner Entry → Mobile Web UI → Onboarding API → Core/Shared Services → Partner Callback**

### Key components
- **Partner adapter:** token/user context exchange, completion callback, membership registration, rate/benefit list query
- **Eligibility & policy gate:** “cooling-off” restriction, limited-account rules, safety-block flags, etc.
- **Identity/compliance orchestration:** identity verification steps + compliance branching
- **Persona & classification handling:** persona update to sole proprietor + industry classification validation/mapping
- **Documents/BPR sender:** generate and transmit required business documents (sanitized)
- **Observability:** step markers and error monitoring for drop-off and failure analysis

## User Flow (Screens)
Built/updated the full onboarding path (reusing shared patterns where possible):
1) Customer info confirmation (partner context validation)  
2) Join guide + terms  
3) Input + signing (branching steps for compliance/verification)  
4) Completion + return to partner  
(Expanded to ~8 screens when including branching cases like compliance steps and exception handling.)

## Backend Services (Sanitized Breakdown)
Implemented/extended ~6 onboarding services across:
- Partner adapter + callbacks
- Eligibility checks
- Identity/compliance branching
- Persona update + classification validation
- Document generation/transmission
- Account opening orchestration

## Post-Launch Monitoring (Drop-off Hotspots)
Added step-level markers to identify where users exited the flow and why.  
Top drop-off causes observed early after launch (daily average):
- **Cooling-off restriction check:** ~23/day  
- **Limited-account restriction check:** ~7/day  
- **Safety-block subscribers:** ~3/day  
- **Identity verification drop-offs:** ~2/day

This informed how we tuned guidance text and error handling for support teams and partner operations.

## Incidents & Fixes (Real Examples)
### 1) Special character issue during business data scraping
- **Problem:** certain business names with special characters caused a failure and blocked progression.
- **Fix:** applied safe escaping/validation before generating partner-required documents.
- **Outcome:** restored flow continuity; reduced recurring failures.

### 2) Classification mapping issue impacting persona updates (14 cases)
- **Problem:** classification mapping inconsistency caused failures during persona update/compliance registration.
- **Detection:** monitored error responses and user reports.
- **Fix:** corrected mapping and added preventive validation checks (“guardrails”).
- **Outcome:** issue resolved; added protection against recurrence.

## Operational Improvements (with Partner Team)
After launch, continued coordinated improvements with the partner engineering team:
- refined error messaging and operational monitoring
- improved account alias selection UX and support texts (sanitized)
- adjusted business logic for recommendation/attribution (sanitized)

## Key Takeaways
- Strong onboarding systems are built twice: once for launch, and again after real-world usage reveals edge cases.
- Step tracking + clear error handling dramatically reduces time-to-diagnosis and support overhead.
- Partner integrations succeed when failure modes and callbacks are treated as first-class product flows.

## What I’d Improve Next
- Convert repeated checks into reusable policy modules and centralize error/message mappings.
- Add dashboards for “drop-off by reason” and “partner callback success rate.”
