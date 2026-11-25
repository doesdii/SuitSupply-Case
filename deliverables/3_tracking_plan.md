# Design details for trousers: tracking plan

For the developers and PMs who build the test. Every threshold, rate, and cost is an assumption until Suitsupply supplies the data in section 7. Gap IDs refer to `5_gap_analysis.md`. The prototype fires all 12 events with these names and properties.

A **changed** pair has at least one detail (waistband, hem, or front) that differs from the standard of its fit. A **point** is one percentage point.

## 1. What decides

### Success metrics (all three must hold)

| Metric | Definition | Threshold |
|---|---|---|
| Adoption | Share of trouser orders in the test group with at least one changed detail | 5% or more |
| Speed | (a) Add-to-bag rate from the prompt. (b) Median time from the prompt to "Add to bag" on the Keep path. | (a) Not more than 2% lower, relative (1 point at a 50% rate). (b) Not more than 10% longer. |
| Keep | Return rate of changed pairs within 30 days of delivery | Not more than 5 points above made-to-order fits |

- Adoption = 25% open the fit picker × 40% open the details sheet × 50% change a detail. Replace each factor with real data (D3).
- Keep: returns are allowed under today's 30-day policy [6] (L1). A changed pair cannot go back into stock, so every returned changed pair is a write-off. Split returns by reason, so a size return does not count against the detail.

**Safety metrics.** Order rate from the prompt: not significantly lower than control. Made wrong: 2% or less of changed orders. Late: 10% or less.

### Stop rule

The Custom Made PM sets `trousers-design-options` to OFF within one working day when one condition is true. A daily sequential test checks the conditions [4].

1. **Speed:** the add-to-bag rate or the order rate is significantly lower than control.
2. **Keep:** the return rate of changed pairs is more than 5 points above made-to-order fits, on the first 300 changed orders with a closed 30-day window.
3. **Operations:** made wrong above 2%, or late above 10%.

Early warning, also OFF: expectation match under 90% with 100 answers or more, or 24-hour cancellations more than 2 points above standard orders. OFF gives every customer today's flow.

**Per-option rule.** After 150 changed orders with one option, a return rate more than 10 points above made-to-order fits switches off that option only.

**Gray-zone rule.** If Speed is neither "not more than 2% lower" nor "significantly lower", extend the test by up to 2 weeks. Still unclear: do not roll out, and test a lighter variant.

## 2. Segments

The analytics tool computes segments per visit (`session_id`) from events.

| Segment | Event rule | Read first |
|---|---|---|
| 1. Skipped the fit picker | `keep_fit_tapped`, no `fit_picker_opened` | Speed |
| 2. Opened the fit picker, not in the bag | `fit_picker_opened`, no `added_to_bag` | Sheet exit, reset, flip |
| 3. Opened the fit picker, in the bag | `fit_picker_opened`, then `added_to_bag` | Adoption, Keep |

- Segments end at the bag. Orders and returns join through the order line, where `has_opened_fit_picker` splits segment 1 from segment 3.
- Both groups have all three segments, because the fit picker exists today.
- Known customers (identified or signed in) are a layer, not a segment. Cut them by `identity_tier` for Release 1.1.

## 3. Test design

| Item | Rule |
|---|---|
| Exposure | `prompt_shown`. Users who never reach it stay out of the analysis. |
| Split | 50/50 by user, one market |
| Length | 4 full weeks or more [1]. Read success once, at the planned sample [2]. |
| Volume | 31,000 users × 50% bag × 60% order × 5% Adoption ≈ 465 changed orders |
| Keep read | 300 changed orders with a closed 30-day window, about 45 days after the last order. Fewer: a signal, not proof, and the rollout stays in one market. |
| Sample ratio check | Chi-squared test on group sizes. If p < 0.0005, find the cause first [3]. |
| Start date | After the holiday period. Purchases from 1 November to 25 December have 60 days to return, which stretches the Keep read. |

Sample size for Speed (a), one-sided, 5% error rate, 80% power: n = 6.183 × 2p(1 − p) ÷ (0.02p)².

| Add-to-bag rate today (D2) | Users per group |
|---|---|
| 30% | About 72,000 |
| 50% | About 31,000 |
| 70% | About 13,000 |

Rollout stages:

0. The team, before launch: test orders go through production.
1. 10% of one market, 1 week: find errors only [5].
2. Same market, 50/50, about 4 weeks: Experiment 1.
3. Same market: Adoption and Speed decide at week 4. Keep decides at about day 45.
4. More markets, one at a time. Experiment 2 starts.

| | Experiment 2 (R1.1) | Experiment 3 (R1.2) |
|---|---|---|
| Flag | `trousers-details-shortcuts` | `trousers-details-pdp-entry` |
| Exposure | `prompt_shown`, known customers | `size_and_fit_expanded` |
| Decides on | Higher add-to-bag rate, time to bag | Adoption |
| When | After Experiment 1 holds | Only if Adoption is 2% to 5%. Under 2%: remove the options. |

## 4. Events

Naming rule: `fit_flow:` then the object, then the past-tense action, in snake case. `_shown` is a system action, `_tapped` a customer action. Yes-or-no properties start with `is_` or `has_`. If an add-to-bag event exists, add these properties to it.

| Event | Fires when | Own properties |
|---|---|---|
| `prompt_shown` | The prompt shows | None |
| `keep_fit_tapped` | "Keep [fit]" | None |
| `fit_picker_opened` | "Choose fit" or "Choose fit and details" | None |
| `fit_changed` | The main fit or leg shape changes, by a tap or by "Continue" restoring a saved fit | `fit_part`: `main_fit`, `leg_shape`. `value`. `reverted_option_count`: 0 to 3. |
| `details_sheet_opened` | The details sheet opens | `step`: `main_fit`, `leg_shape`, `size_step`, `product_page`. `opened_from`: `details_row`, `photo_thumb`, `edit_link`, `product_page_link`. |
| `detail_value_tapped` | An option tile | `option`: `waistband`, `hem`, `front`. `value`, `previous_value`, `is_standard_value`. |
| `details_reset_tapped` | "Reset to standard" | `reset_detail_count`: 1 to 3 |
| `size_step_shown` | The size step shows | `entry`: `keep_fit`, `fit_picker`, `suggestion`, `product_page` |
| `added_to_bag` | The enabled "Add to bag" | `size`, `size_length`, `has_changed_detail`, `is_made_to_order`, `price`, `currency`, `size_prefill_outcome` (`kept`, `changed`, `none`), `has_opened_fit_picker`, `tap_count` |
| `suggestion_shown` | A suggestion card shows (R1.1) | `suggestion_type`: `last_details`, `resume_selection`. `suggestion_source`: `order_history`, `device_storage`. |
| `suggestion_accepted` | "Use these details" or "Continue" (R1.1) | Same as `suggestion_shown` |
| `size_and_fit_expanded` | "Size & Fit" opens on the product page (R1.2) | `has_details_link` |

`tap_count` checks the reference paths: fast path 4, one changed detail 9, known customer with a standard pair 3, last details 4, resume 4, product page entry 7.

Every event carries these 17 super properties. The page sets context once per visit. Flow state shows the state after the action.

| Property | Group | Values |
|---|---|---|
| `product_id` | Context | `B2248` |
| `flag_variant` | Context | `test`, `control` |
| `customer_state` | Context | `anonymous`, `identified`, `signed_in_size`, `signed_in_details` |
| `fit` | Flow state | `slim_tapered`, `slim_straight`, `straight`, `wide_tapered`, `wide_straight` |
| `is_fit_in_stock` | Flow state | true, false |
| `waistband` | Flow state | `belt_loops`, `side_adjusters` |
| `hem` | Flow state | `plain`, `turn_up` |
| `front` | Flow state | `flat`, `pleated` |
| `changed_detail_count` | Flow state | 0 to 3 |
| `delivery_estimate` | Flow state | `1_2_days`, `1_2_weeks` |
| `size_prefill` | Flow state | `none`, `preselected`, `suggested` |
| `identity_tier` | Context | `anonymous`, `identified`, `signed_in` |
| `consent_state` | Context | `analytics_only`, `analytics_and_personalisation` |
| `market` | Context | `nl` |
| `locale` | Context | `en-nl` |
| `device_type` | Context | `mobile`, `tablet`, `desktop` |
| `session_id` | Context | Random ID, no personal data |

- Details hold the effective value: the customer's choice, or else the fit standard.
- `size_prefill`: preselect only what can go back into stock, and suggest what cannot. `preselected` needs a signed-in Size Passport size and `changed_detail_count` 0.
- Without consent for personalization, `identity_tier` is `anonymous`. Without analytics consent, no events fire.

Early signals arrive in days. Only the two early-warning conditions can stop the test.

| Signal | Watch level |
|---|---|
| Flip rate: 3 or more taps on one option | Rising weekly |
| Reset rate: `details_reset_tapped` ÷ `details_sheet_opened` | Above 20% |
| Sheet exit: `details_sheet_opened`, no `added_to_bag` | Above the exit rate of picker users without the sheet |
| 24-hour cancellations of changed orders | Stop rule |
| Expectation match: "Did the details match what you expected?" | Stop rule |

## 5. Order data

Adoption, Keep, safety, and money read the order system, so customers who decline analytics still count. Each order line carries `fit`, `waistband`, `hem`, `front`, `changed_detail_count`, `is_made_to_order`, `flag_variant`, `identity_tier`, `market`, `size_prefill`, `has_opened_fit_picker`, and `opened_from`, so the team can cut every outcome by group, segment, and detail.

| Record | Fields | Feeds |
|---|---|---|
| Return | Date, reason | Keep |
| Cancellation or query | Date, reason | 24-hour cancellations |
| Expectation answer | Yes or no | Expectation match |
| Delivery | Promised and real date | Late |
| Production error | Type | Made wrong |
| Cost | Stock, made to order, return handling | Money |

Returns, cancellations, and expectation answers join to the test through `flag_variant` on the order line.

## 6. Money

Euros per pair. The €179 price is real [6]. Every cost is invented (F1, P6).

| Pair | Kept | Returned | At 20% returns |
|---|---|---|---|
| Standard, from stock | +109 | −10, back to stock | 85.2 |
| Changed, made to order | +99 | −90, written off | 61.2 |

- Break-even: about 3 in 10 changed orders must be new orders (gap 24 ÷ 85.2 = 28%).
- At the Keep limit (25% against 20% returns): about 4 in 10.
- A €30 surcharge closes the gap. It still sits below full Custom Made at €229 in this cloth.

Limits:

- Every cost is a placeholder.
- Per order, the test cannot tell a new order from a moved one. Compare margin per exposed user.
- The model ignores repeat buying.

## 7. Numbers Suitsupply must supply

- **D1, D2:** weekly users per step, and today's rates from the prompt to the picker, bag, and order.
- **D3:** today's share of orders with a non-base fit.
- **D4, E5:** the analytics, test, and flag tools, the share of users who accept analytics, and how fast OFF works.
- **C1, C5, C6:** return rates, reasons, and timing. Today's cancellation and late rates.
- **F1, P6:** margins, return cost, and the cost of a detail.
- **L1, L3:** the return rule for changed pairs, and what happens to a returned pair.
- **I4, I5:** the mix of identified, signed-in, and anonymous buyers.
- **M3:** the business goal.

## Sources

1. Springer (2015). Online controlled experiments and A/B tests.
2. Evan Miller (2010). How not to run an A/B test.
3. Microsoft Research (2020). Diagnosing sample ratio mismatch in A/B testing.
4. Spotify, on arXiv (2024). Risk-aware product decisions in A/B tests with multiple metrics.
5. PostHog. Best practices for production-ready flags.
6. Suitsupply. Returns policy and product pages, 23 and 24 November 2026.
