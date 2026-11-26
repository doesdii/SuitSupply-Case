# Design options for trousers: reasoning

**In one line:** each fit already comes with its own waistband, hem, and front style, but only the photo shows it. I make the three details visible and choosable inside the fit picker, with no new step.

Main outputs: `presentation.html` and `prototype/index.html`. Detail: the tracking plan, user stories, and gap analysis.

## Why this placement

- **Speed.** A customer who taps "Keep" sees nothing new: 4 taps, as today. I assume about three in four customers take this path.
- **Same trade.** The fit picker already trades delivery time for a more personal pair. A changed detail makes the same trade: made for you, 1-2 weeks.
- **Honesty.** One "Details" row names what the photo already shows.
- **Safety.** A detail that is not standard needs an active tap. Nobody gets changed trousers by accident.
- **The cost.** Fewer customers find the options, and one changed detail takes 9 taps.

Rejected: a new step before the size step (one more screen for everyone), a row on the size step (a new decision on the fast path), and a block after "Add to bag" (the delivery changes after the decision).

**Third option: the front style**, flat or pleated. Each fit already has a fixed front, the product page names it, and Suitsupply makes both. It uses the same control and needs no measurement.

**Next tests, each behind its own flag:** shortcuts for known customers (Release 1.1), and an entry from "Size & Fit" on the product page (Release 1.2, only if Adoption is low).

## What I left out on purpose

- No extra step and no confirmation screen. Both add taps, and a warning makes a sure customer unsure.
- No return text. The return promise of today does not change, so the flow needs no new words.
- No price change. A made-to-order fit has the same price today.
- No fourth option, free text, or measurement. These need another control and add return risk.

## What I want to validate before the real build

1. **Production:** which details can each fit have, and in which time? How does a turn-up get its length?
2. **Returns:** does today's policy cover changed pairs? My reading of the EU guidance: preset options probably keep the 14-day right. Is there a change window before production starts?
3. **Customers:** 5 to 8 moderated tests. Do customers find the row, understand the options, and see the delivery change?
4. **Data:** today's rate at each step and the share of non-base fits, to set the thresholds.

## What I am least sure about

- **Discovery:** the row can be too hidden. Release 1.2 is ready if Adoption lands between 2% and 5%.
- **Money:** a changed pair earns less than a stock pair. About 3 in 10 changed orders must be new orders to break even. Per order, the test cannot tell a new order from a moved one.
- **Returns:** a changed pair cannot go back into stock, so every return is a write-off. The final rule is Suitsupply's call.
- **Images:** no image shows a changed combination. The fit is a real photo, and each detail reuses a render from the full Custom Made configurator.

Success is three numbers, one per constraint: Adoption, Speed, and Keep. A fourth, Come back (repeat orders within 90 days), shows later if it lasts. All numbers are first assumptions. Suitsupply data must replace them.
