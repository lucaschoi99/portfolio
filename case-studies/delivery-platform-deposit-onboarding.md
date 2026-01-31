# Delivery-Platform Integrated Deposit Account Launch (Mobile Web)

> Public-safe case study. Product/brand specifics are generalized.

## Overview
A mobile-web launch of a deposit account integrated with an internal delivery platform initiative.
The product flow reused the existing account-opening pipeline, but required UI generalization and dynamic product messaging to support future launches efficiently.

- **Launch:** Dec 2025
- **Result:** **4,069 new accounts in the first ~3 weeks**
- **Stack:** React (TypeScript), React Query, Java (Spring), shared onboarding APIs

## My Role
- Primarily owned the **frontend** deliverables, while aligning with backend/shared services.
- Refactored shared onboarding UI so multiple products could share the same flow with fewer product-specific branches.
- Improved completion UX to display product-specific limits/messages correctly based on runtime product metadata.
- Supported release readiness and early operations feedback (monitoring + quick fixes where needed).

## What Changed vs. “Standard” Account Opening
Even though the underlying process was similar to existing deposit products, this launch needed:
- product metadata to be handled generically (code/name stored and consumed consistently)
- completion screens and copy to adapt dynamically to product characteristics
- minimal special-case branching so future products could be added faster

## What I Built (Frontend)
### 1) Shared onboarding UI refactor (React Query)
- standardized server-state fetching patterns
- reduced repeated logic across product pages
- improved maintainability by minimizing product-specific conditional code

### 2) Product metadata generalization
- ensured session/context carried product identifiers consistently
- used the metadata to drive UI behavior (texts, limits, completion content)

### 3) Completion experience improvements
- implemented product-aware completion rendering
- ensured messaging reflected the product’s characteristics without requiring separate code paths

## Results & Impact
- **4,069 new accounts in the first ~3 weeks post-launch**
- enabled faster future launches by reducing special-case code and increasing reusability
- improved operational support readiness by keeping flows consistent and predictable

## Key Takeaways
- The fastest way to ship the next product is to remove special cases in the current one.
- A clean server-state pattern (React Query) makes onboarding flows easier to extend and safer to operate.
