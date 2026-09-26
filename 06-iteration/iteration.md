# Iteration: Analytics, Sprint, Final Recommendation

> Module 6 · Evals & Iteration. Read the analytics, run an iteration sprint, present with evidence.

## What the evidence says

_What real usage showed: numbers if your tool has analytics, counted behaviour if it does not. Put the signal that matters on screen._

- **Primary signal:** Bookings
- **What moved:** Bookings for verified users with verification tags went up
- **What didn't:** _____

_Analytics snapshot: visitors 15; page views 286; views per visit 19.73; duration 8m21s; bounce 8._

## Iteration sprint

| Change | Hypothesis | Result |
|---|---|---|
| Workflow Change | Verification Badges on new service providers will improve bookings | Sample size is too small. but the people who did went straight to established service providers |

## Peer feedback

Looking for help and Want to earn on Baazzaar cards not working

## The recommendation

**Decision:** ☐ Go  ☑ Iterate  ☐ Kill

_The evidence that justifies the call:_

Every event is in the "trust" arm. There is no control group traffic, so there's nothing to compare against — the kill switch can't fire and no lift can be measured. The hypothesis is untested, not validated or broken.
Established providers still dominate. Leo Hartman (reviewed) got 7 profile views, 6 booking starts, 3 completed bookings. The zero-review test providers got far less: Clara Vance 3 views and 0 bookings; Juno Park 1 booking; Amara and Tobi effectively none.
The funnel leaks at profile→booking for new providers, matching your original 68% search→profile→exit problem. Views happen; bookings don't follow.
Sample size is tiny — a handful of visitors, mostly direct/desktop, likely you and testers. No conclusion is statistically meaningful yet.

## Final showcase

- **Demo link:** https://trust-spark-project.lovable.app
- **The one-sentence story:** This test engine measures whether displaying verification badges on new service providers increases user trust enough to drive actual bookings with them.
- **Where it landed on the Confidence Line (M2 → now):** Because the sample size was too small its a test in progress. Definately much better than what i started with M2.  Will classify it at Evaluation and Iteration Stage
