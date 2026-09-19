# Living PRD

> Module 4 · Production Specs. Refactor for readability; extract a living PRD that stays true as the build evolves.

## Problem

_What user problem does this solve? Tie to the validated hypothesis._

Buyers don't book providers who have no reviews, and providers can't get reviews until someone books them. The prototype's own on-screen evidence:

0 reviews on 41% of active provider profiles
2.3% booking rate for zero-review providers vs 14% for reviewed providers
68% of sessions go search → profile → exit
19 days median time to a new provider's first booking
−4% QoQ booking growth
Buyer and provider voice (shown as real content in the product):

"If there are no reviews, I assume something's wrong with them. I'll pay more for someone with a track record." — Buyer, churned at checkout
"I'm great at my job but I'll never get a review if no one books me first. It's a chicken-and-egg trap." — New provider, 0 bookings
"I wish I could see something, a verified ID, a portfolio, anything, before I commit money." — Buyer, abandoned search
"The established providers are booked out for weeks. I'd try someone new if I felt safe doing it." — Repeat buyer
Hypothesis under test: a trust panel on new-provider profiles — verification badges, early social proof, links to portfolio/social work, off-platform experience, and the provider's own story — increases first bookings for zero-review providers, and increases buyer belief that the marketplace vets on their behalf.

Success signal: first-booking rate for new providers rises month over month, with satisfaction and ratings holding or improving.

Kill switch (observable in the product): the /experiment page compares the trust arm against the control arm. If the trust arm shows no lift once both arms have new-provider profile views, the decision panel reads Pivot — trust is not the barrier. If the trust arm books ahead, it reads Continue. This verdict is computed live from session events, not hardcoded.

## Users & jobs

- **Primary user:** Primary user — Marketplace buyer (new or repeat). Job to be done: "When I need a service and the available providers have no track record, I want enough credible evidence about who they are and what they've done, so I can book confidently instead of dropping out or overpaying for an established name."  Secondary user — Service provider with zero reviews. Job to be done: "When I'm new on the platform, I want to show verification, real work and my own story, so buyers take a first chance on me and I escape the no-reviews trap."  Internal user — Product/growth owner running the test. Job to be done: "When I ship a trust intervention, I want a per-arm funnel, buyer-credited reasons and a day-over-day log, so I can decide to continue or pivot on evidence."
- **Job to be done:** _____

## Scope

- **In:** Browse: home, category, service and provider pages
Trust panel on new-provider profiles: verification badges, endorsements, portfolio images, social/off-platform links, provider story
Global trust-panel On/Off toggle (experiment arm switch) in the header
Booking flow: pick slot → notes → confirm, gated behind login
Post-booking feedback: which profile components drove the decision
Login/sign-up with two doors: marketplace user and service provider
Buyer account: profile, service interests, payment profile
Provider hub: analytics, profile metadata, verification journey with expected boost per remaining step, payout profile, biweekly statements, support requests
Experiment workspace: Overview + kill switch, Users funnel with named lists, Service providers metadata comparison, Daily log
- **Out (explicitly):** Real authentication, real accounts, password reset, email verification
Real identity/background/insurance verification — documents are status flags only, no upload or review pipeline
Real payments, payouts, card or bank capture, invoicing, tax
Real reviews and ratings lifecycle, messaging between buyer and provider
Search and ranking algorithm changes; the trust panel does not affect ranking
Server-side data, multi-device continuity, admin/ops tooling, notifications
Statistical significance testing on the kill switch (it's a directional comparison)

## Requirements

| # | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| 1 | Trust panel on zero-review provider profiles | Must | With the arm set to trust, a zero-review provider profile shows verification badges, endorsements, portfolio images, off-platform experience, social links and the provider's story. With the arm set to control, the same profile shows only name, price and "No reviews yet." | |
| 2 | Kill-switch decision panel | Must | Once at least one zero-review profile has been viewed in each arm, `/experiment` shows Continue or Pivot derived from per-arm first-booking rates against the 2.3% baseline. Before that it states evidence is insufficient. | |

## Data & events

_What gets stored, what gets tracked._

Event log (real within the browser, localStorage)

Event	Payload	Emitted when
profile_view	provider id, provider name, isNew flag, arm, timestamp	A provider profile renders
booking_start	provider id, arm, timestamp	A signed-in buyer starts the booking flow
booking_complete	provider id, arm, timestamp	Booking is confirmed
booking_reason	provider id, credited factors, optional note, arm	Post-booking feedback is submitted
arm_change / session reset	arm value	Toggle flipped, or session data cleared on /experiment
Derived measures: per-arm profile views, booking starts, confirmed bookings, first-booking rate vs. the 2.3% baseline, trust-factor vs. price/availability factor tally, booked-vs-viewed metadata averages, and a per-day rollup.

Stored entities: session (role, name, email), buyer profile (name, city, phone, interests, mock payment fields), provider profile (trade, service area, rate, response time, off-platform experience, story, socials, payout details, document statuses, support tickets). All keyed by email in browser storage.

Mocked vs. real

Real: the arm toggle, event capture, funnel and kill-switch maths, per-account persistence, booking and login flows as interactions.
Mocked: all accounts and credentials (two demo logins, any sign-up accepted, no password hashing); provider listings, portfolio images, endorsements and stories are authored content; verification document statuses are flags with no upload or review; payment and payout forms capture nothing sensitive and charge nothing; statements, fees and payout cycles are computed from seeded jobs; 20 buyer personas and 5 prior days of experiment data are simulated to make the funnel and daily view legible; provider analytics blend seeded counts with live session events.
Not present at all: any server, database, API or third-party integration. Clearing browser storage resets the entire product state.

## Open questions

Should accounts and the event log move server-side so results survive devices and can be trusted for a real decision?
How much of the marketplace should require login — browsing open until booking (current behaviour), or login before anything?
What is the real verification pipeline: which documents, who reviews them, what SLA, and which badge does each produce?
Are the per-step "expected boost" numbers shown to providers evidence-based, or do they need to be derived from live data before we display them?
Which payment and payout providers, and what happens to a booking payment between confirmation and the biweekly payout?
What is the significance bar and minimum run length before the kill switch is allowed to read Continue or Pivot?
Does the trust panel also need to influence search ranking, or is profile-level presentation enough to move first bookings?
Should buyer-credited factors be optional feedback or a required step, given the whole decision depends on that signal?
How do we handle a provider whose verification lapses or whose off-platform claims can't be substantiated?
What replaces the panel if the verdict is Pivot — price guarantees, booking protection, or something else?
