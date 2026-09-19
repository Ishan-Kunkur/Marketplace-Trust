# Engineering Handoff Note

> Module 4 · Production Specs. Open the black box, make the build legible to an engineer.

## What this is

_One paragraph an engineer can read in 60 seconds._

This is a TanStack Start v1 prototype that tests one product hypothesis: adding a "trust panel" (verification badges, off-platform story, portfolio, social proof) to zero-review provider profiles will raise first-booking rates. The app is a clickable marketplace with three doors: marketplace buyer, service provider, and product team. The product team can flip an A/B arm toggle; everyone else browses the trust arm. All data, auth, and experiment instrumentation currently live in the browser (static TypeScript data + localStorage), but a Lovable Cloud Supabase project is connected and the schema/types are generated — the next step is to wire the UI to the real backend.

## Architecture (plain language)

- **Frontend:** Frontend Framework: TanStack Start v1 (React 19, Vite 8, file-based routing). Styling: Tailwind CSS v4 with native CSS theme variables in src/styles.css; components built on Radix + shadcn patterns (src/components/ui/). Visual identity: Green/white, Arial family, rounded cards, bg-surface / bg-card, border-border, font-display, subtle animate-[rise_...] entrance animations. Routing: / — Home /category/:categoryId — Category page /service/:serviceId — Service page /provider/:providerId — Provider profile (the A/B test surface) /book/:providerId — Booking screen /login — Three-door login /account — Buyer account hub /provider-hub/* — Provider workspace /experiment — Product-team experiment dashboard Feature-folder structure: src/features/{browse,provider-profile,booking,auth,buyer-account,provider-hub,experiment} hold screen components and their helpers. src/data/ holds static content shapes and selectors. src/components/ holds shared UI pieces (SiteHeader, ProviderCard, etc.). Backend / data (current state) In-app state: Everything the user sees today is driven by static TypeScript modules and localStorage. src/data/ — categories, providers, services, offerings, stories, simulated buyers, metrics. src/features/auth/session.tsx — mock auth/session store keyed by baazzaar-session-v3. src/features/experiment/experiment-store.tsx — mock experiment store keyed by trust-experiment-v2; tracks view, booking_start, booking, booking_reason, and arm_change events.
- **Backend / data:** Supabase backend: A Lovable Cloud Supabase project is connected. Generated types live in src/integrations/supabase/types.ts and the standard clients (client.ts, client.server.ts, auth-middleware.ts, auth-attacher.ts) are present. The schema was created to mirror the prototype data (catalogue tables, profiles, bookings, experiment events, daily rollups, and a product app role). However, the UI has not yet been rewired to read from or write to Supabase — it still uses the in-browser mocks.
- **Key flows:** Browse → Book (buyer): Home → category → service → provider profile → book. Booking is gated: only signed-in marketplace users can confirm. Unsigned visitors are nudged to /login?book={providerId} and returned.
Trust-panel A/B: The provider profile renders the full trust panel when the arm is "trust" and a minimal control view when "control". The toggle is visible only to signed-in product-team accounts.
Experiment measurement: Events are written to localStorage. The experiment dashboard computes conversion rates, shows a funnel, and renders a "Pivot" verdict when the trust arm does not outperform control among zero-review providers.
Provider workspace: Signed-in providers see analytics, profile metadata editor, verification journey with per-step boosts, payouts/billing, and support tickets.
Feedback loop: After booking, buyers see a card asking which profile components influenced the decision; selections are logged as booking_reason events.

## What's solid vs. what's duct tape

| Area | State | Notes |
|---|---|---|
| A/B instrumentation shape: The event schema (view, booking_start, booking, booking_reason, arm_change) and the kill-switch rule are explicit and testable. | solid | _____ |
| Experiment data is fake and gameable. Buyer personas are hard-coded, conversion is probabilistic, and events are client-written. The numbers look real but are not from actual users. | rough | Payments, verification, and payouts are static mocks. Card forms save strings to localStorage; document uploads are toggles, not files; billing statements are generated from local events, not real money movement. |

## Risks & assumptions for the team

Risks and assumptions
Risk	Why it matters	Mitigation
Demo data is mistaken for a pilot	Stakeholders may treat the fake conversion numbers as evidence	Label the experiment dashboard as simulated; replace hard-coded buyers with real traffic before reporting metrics
localStorage schema drift	Keys (baazzaar-session-v3, trust-experiment-v2) can collide or corrupt across iterations	Version keys on breaking changes; migrate old keys on startup or clear them in dev
SSR hydration mismatches	Components read localStorage only after hydration; some screens gate renders with hydrated but not all	Audit for window/localStorage access outside useEffect; test hard refreshes on protected routes
Arm toggle is client-side only	A determined user can flip the arm or spoof events	Move arm assignment and event logging to the backend; gate /experiment behind server-verified product role
Supabase is connected but unwired	The schema exists but the app ignores it, so the "backend is live" impression may be misleading	Treat Supabase wiring as the next milestone; delete static data modules once tables are seeded from them
Role-based gating is local	Product-team access is enforced by session.role, which is just a string in localStorage	Use requireSupabaseAuth + has_role(..., 'product') once the backend is wired
Key assumptions
The trust panel is the primary lever for first bookings; if the A/B test fails, the hypothesis is rejected and we pivot.
Zero-review providers are the population of interest; the dashboard filters experiment results to isNew === true providers.
All buyer/provider/product traffic runs from the same deployment; role gating is sufficient for this prototype.

## How to run it

```
# Install dependencies
bun install

# Start the dev server (already running in this environment at http://localhost:8080)
bun run dev

# Typecheck
bunx tsgo --noEmit

# Build (catches import/SSR issues)
bun run build:dev
Demo accounts
Role	Email	Password	What it unlocks
Marketplace user	buyer@baazzaar.test	Baazzaar#1	Booking, buyer account, post-booking feedback
Service provider	clara@baazzaar.test	Baazzaar#1	Provider hub, analytics, verification journey
Product team	product@baazzaar.test	Baazzaar#1	/experiment dashboard and A/B arm toggle
Reset local state
Open browser DevTools and clear:

localStorage.removeItem('baazzaar-session-v3');
localStorage.removeItem('trust-experiment-v2');
Then refresh. This is useful when switching personas or when the experiment state feels stale.
```
