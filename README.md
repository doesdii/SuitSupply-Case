# Design options for a made-to-order trouser flow

A take-home product case. Customers can already order trousers in another fit from the product page. The case adds three design options (waistband, hem, and one of my choice) and asks where they go, what the customer sees, and how to measure whether they were a good idea.

## Open it

| File | What it is | How to use it |
|---|---|---|
| `presentation.html` | The 13-slide presentation, with the live prototype on slide 5 | Double-click. `→` or space: next. `←`: back. `N`: speaker notes. `T`: timer. |
| `prototype/index.html` | The clickable prototype: phone and desktop views, feature flag, event log, demo scenarios | Double-click. It needs no network. Add `#v=d` to the address for the desktop view, or `#selftest` to run its self-test. |
| `engineering_flow.html` | The technical flow for engineers: every screen, action, condition, event, property, and state rule, read from the prototype's code | Double-click. Click any screen, line, or decision for its trigger, condition, and events. |
| `deliverables/2_reasoning.md` | One page: why this placement, what I left out, what to validate, what I am least sure about | Read first. |
| `deliverables/3_tracking_plan.md` | Success metrics, stop rule, test design, events and properties, money | For product managers and developers. |
| `deliverables/4_user_stories.md` | 21 user stories with acceptance criteria | For developers. |
| `deliverables/5_gap_analysis.md` | Open questions per team, with working assumptions | For the first meeting after the case. |

## The solution in short

- Each fit already comes with its own waistband, hem, and front style. Only the photo shows it.
- One "Details" row in the fit picker opens a sheet with the three options. A customer who keeps the fit has the same 4 taps as today.
- Success is three numbers: Adoption (5 in 100 orders have a changed detail), Speed (time and add-to-bag rate hold for the customers who skip), and Keep (at most 5 more returns per 100 than made-to-order fits). Come back (repeat orders within 90 days) is read later. One feature flag turns it off.

## Notes

- Product photos are not in this repository. Without them, the prototype and the presentation show drawings.
- All numbers are first assumptions. Real data must replace them.
