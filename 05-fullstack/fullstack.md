# Full-Stack: Data, Access Rules, Edge Cases, Deploy

> Module 5 · Full-Stack. Add data schemas, access rules, and edge cases; stress-test and deploy.

## Deployed link

_The working, shareable link that survives real users._

_____

## Data schema

| Entity | Key fields | Notes |
|---|---|---|
| experiment_settings | id bool PK with CHECK (id), arm, updated_at, updated_by | The CHECK (id) forces exactly one row. Currently arm = trust |
| experiment_events | event_type, arm, provider_id, service_id, session_id, user_id, reason, metadata jsonb, created_at, is_simulated | _____ |

## Access rules

_Who can see / do what? Where are the auth boundaries?_

Public — anyone, no sign-in (read only)

Categories, providers, services, offerings, success stories, and all provider child tables (verifications, reviews, endorsements, portfolio, social links, stories, documents): anyone can read, nobody can write through the app.
Experiment arm setting: anyone can read it; only the product team can change it.
Experiment events: anyone can log one, but user_id must be empty or their own — and the arm is stamped server-side by a trigger, so it can't be spoofed.
Signed-in buyers

Own profile: read, create and update only your own row.
Own bookings: create and read only your own.
Own feedback: read your own; can only add feedback tied to a booking you own — feedback without an owned booking is rejected.
Signed-in providers

Update only the listing you own; read, create and update only your own workspace, payout details and support tickets.
Full manage access to documents on your own listing.
Read-only access to bookings on your listing.
Product team

Read-only on experiment events, daily rollup, buyer personas, all bookings and all feedback — gated by the has_role security-definer function.
The only role that can set the experiment arm.

## Edge cases hardened

| Case | Before | After |
|---|---|---|
| Empty / first-run state | Provider with no completed verifications. - Blank area where badges should be | "Verification in progress — no checks completed yet" |
| Bad / malicious input | Double-click "Confirm booking"-Two bookings created | One booking; the second click returns the original with "You've already booked this slot |
| Failure / offline | Booking save fails-No feedback to the user | Retried quietly, then a notice with Retry |

## Stress test results

_What you threw at it, and what held / broke._

_____
