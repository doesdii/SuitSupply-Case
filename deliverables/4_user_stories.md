# Design options for trousers: user stories

These 21 stories tell the developers what to build from the prototype. Release 1 adds waistband, hem, and front style to the fit picker, Release 1.1 shortens the path for known customers, and Release 1.2 tests a product page entry if Adoption lands between 2% and 5%.

Quotes are exact screen texts. Events, properties, and flags come from `3_tracking_plan.md`. Every number is an assumption.

| ID | Title | Release | Priority |
| --- | --- | --- | --- |
| US-01 | Prompt text | R1 | Must |
| US-02 | Fast path stays the same | R1 | Must |
| US-03 | Details row | R1 | Must |
| US-04 | Details sheet | R1 | Must |
| US-05 | Details stay when the fit changes | R1 | Must |
| US-06 | Picture of the choice | R1 | Should |
| US-07 | Delivery time at the choice | R1 | Must |
| US-08 | Reset to standard | R1 | Must |
| US-09 | Summary with "Edit" | R1 | Must |
| US-10 | Order data for production | R1 | Must |
| US-11 | Feature flag and stop control | R1 | Must |
| US-12 | Tracking events | R1 | Must |
| US-13 | Accessibility | R1 | Must |
| US-14 | Values that are not available | R1 | Should |
| US-19 | Details in the bag panel | R1 | Must |
| US-20 | Turn off one detail on its own | R1 | Must |
| US-15 | Size prefill for signed-in customers | R1.1 | Should |
| US-16 | Reuse the details of the last pair | R1.1 | Should |
| US-17 | Pick up on this device | R1.1 | Could |
| US-18 | Consent and fail-open for personalization | R1.1 | Must |
| US-21 | Entry from "Size & Fit" on the product page | R1.2 | Could |

Every Release 1 to 1.2 criterion is shown in the prototype, unless its story or the criterion is marked *not in prototype*.

## US-01 Prompt text

As a customer, I want to know that details can change, so that I can look at them.

- Given the flag is ON, when the prompt opens, then it shows "Want a different fit or details?", "These trousers come in 5 fits.", and "Choose fit and details".
- Given the flag is OFF, when the prompt opens, then it shows the texts of today: "Want a different fit?", "These trousers come in 5 other fits.", and "Choose fit".

## US-02 Fast path stays the same

As a customer who wants the standard pair, I want the path of today, so that I can buy fast.

- Given the product page, when the customer keeps the fit, then 4 taps put the pair in the bag: "Select size and fit", "Keep Tapered Leg", a size, and "Add to bag".
- Given standard details, when the size step or the bag shows, then it looks as today, with no details line and no "Edit".

## US-03 Details row

As a customer, I want to see the details of each fit, so that I know what I get.

- Given the flag is ON, when a fit screen shows, then a "Details" row names the details, for example "Side adjusters, plain hem, pleated front", and shows "Changed" when one differs from the standard.
- Given the row, when the customer taps it, then the sheet opens. A close-up in the row opens it with that option open.

## US-04 Details sheet

As a customer, I want two clear values per option, so that I can choose with no expert knowledge.

- Given the flag is ON, when the customer taps a row of the sheet ("Waistband", "Hem", or "Front style"), then it opens into two values with a close-up, a name, and a benefit line. "Standard for this fit" marks the standard.
- Given the sheet, when the customer taps "Done", taps outside it, or presses Escape, then it closes and keeps the values.
- Given the product page, when the customer changes one detail, then 9 taps put the pair in the bag: "Select size and fit", "Choose fit and details", the hem close-up, "Turn-up", "Done", "Next", "Next", a size, and "Add to bag".

## US-05 Details stay when the fit changes

As a customer, I want my details to stay when the fit changes, so that I do not choose again.

- Given a fit change, when an option is not set, then it takes the new standard. When it is set, it stays, unless the new fit cannot have it (US-14).
- Given "Turn-up" on Slim Fit, Tapered Leg (its standard), when the customer selects Wide Fit, Tapered Leg, then turn-up stays, with "Changed" and "Delivery 1-2 weeks".

## US-06 Picture of the choice

As a customer, I want to see my choice, so that I am sure about it.

- Given a changed detail, when the fit screen shows, then a close-up of each changed value shows under the photo, with "The photo shows the standard details of this fit."
- Given the open sheet, when the customer selects a value, then its close-up changes in 100 milliseconds or less.

## US-07 Delivery time at the choice

As a customer, I want to see the delivery time when I choose, so that I get no surprise later.

- Given a fit in stock, when the customer selects a value that is not the standard, then the sheet shows "Delivery 1-2 weeks" and "Delivery changes to 1-2 weeks. We make these trousers for you." A fit with no stock shows "Delivery stays 1-2 weeks."
- Given a changed pair, when a later screen shows, then the time stays the same, up to "Delivery: 1 - 2 weeks" in the bag, and the price never changes.

## US-08 Reset to standard

As an unsure customer, I want one way back to the standard, so that I can try with no risk.

- Given no option is set, when the sheet shows, then "Reset to standard" is disabled.
- Given an option is set, when the customer taps "Reset to standard", then all options take the standard, "Changed" disappears, and `fit_flow:details_reset_tapped` fires.

## US-09 Summary with "Edit"

As a customer, I want to check and change my details before the bag, so that I order what I want.

- Given a changed pair, when the size step shows, then a line such as "Your details: side adjusters, turn-up hem, pleated front" and the link "Edit" show above "Add to bag".
- Given the line, when the customer taps "Edit", then the sheet opens, and "Done" returns to the size step with the updated line.

## US-10 Order data for production

*Not in prototype.* As a production planner, I want each order line to hold the details, so that we make the correct pair.

- Given an order, when the order line is saved, then it holds the fit, the three details, `is_made_to_order`, `flag_variant`, and the order fields of the tracking plan.
- Given a changed pair, when the order goes out, then it takes the made-to-order route, also for a fit in stock. A standard pair in stock takes the stock route.

## US-11 Feature flag and stop control

As the Custom Made PM, I want to switch the feature with no release, so that I can stop it within one working day.

- Given `trousers-design-options` is `test`, when the flow opens, then the Release 1 stories apply. Given `control` or OFF, then the flow of today shows.
- Given a rollout stage, when the team sets a market and a share, then only these users get `test`, each in a fixed group (*not in prototype*).
- Given the flag service fails, when the flow opens, then it is the flow of today (*not in prototype*).

## US-12 Tracking events

As an analyst, I want the tracking plan events, so that I can measure the result.

- Given a tracked action, when it happens, then its event of the 12 fires with its own properties and the 17 super properties. No property holds personal data.
- Given either group, when the prompt shows, then the exposure event `fit_flow:prompt_shown` fires.
- Given an add to bag, when `fit_flow:added_to_bag` fires, then it carries `has_opened_fit_picker` and `tap_count`.

## US-13 Accessibility

As a customer with a keyboard or a screen reader, I want to use the options, so that I can buy like other customers.

- Given a keyboard, when the customer goes through the flow, then each control works and has a visible focus ring.
- Given the sheet or the bag panel, when it opens, then the focus moves in and stays in. Escape closes it, and the focus goes back to its control.
- Given the complete feature, when the team runs a WCAG 2.2 AA check and a VoiceOver and TalkBack pass, then no failure shows (*not in prototype*).

## US-14 Values that are not available

As a customer, I want to see only values I can order, so that production can make my order.

- Given production cannot make a value for the fit, when the sheet shows, then the value is disabled with "Not available for this fit". Example: a pleated front on Slim Fit, Straight Leg (gap P1).
- Given a chosen value, when the customer selects a fit that cannot have it, then the option goes back to the standard with "Pleated front is not available for this fit. Front style is back to flat front." `fit_flow:fit_changed` carries `reverted_option_count`.

## US-19 Details in the bag panel

As a customer, I want to see my details in the bag, so that I can check what I order.

- Given a made-to-order pair in the bag, when the line shows, then it has the link "Details" and "Delivery: 1 - 2 weeks".
- Given the customer taps "Details", when "Custom made details" opens, then it lists Delivery, Size, Fit, Waistband, Hem, and Front style, with "Changed" on each changed row.

## US-20 Turn off one detail on its own

As the Custom Made PM, I want to switch off one option, so that one weak option does not stop the feature.

- Given an option is false in `trousers-design-options-enabled`, when the flow opens, then that option is absent from the prompt, the sheet, the details lines, and the bag panel, and orders get its standard.
- Given all three options are off, when the flow opens, then it is the flow of today.

## US-15 Size prefill for signed-in customers

As a signed-in customer, I want my size filled in, so that I can buy in fewer taps.

- Given a Size Passport size and a standard pair, when the size step opens, then the size is selected with "Size 50 Regular is selected, from your Size Passport." Keeping the fit takes 3 taps, with no size tap.
- Given a changed pair, when the size step opens, then the size is outlined, not selected, with "Changed trousers are made for you, so tap to confirm." "Add to bag" waits for a size tap.
- Given a size the flow preselected, when a detail changes, then the flow clears it and suggests it. A reset to standard preselects it again.

## US-16 Reuse the details of the last pair

As a returning customer, I want to reuse my last details, so that I do not choose them again.

- Given a signed-in customer whose last kept pair is not standard, when the prompt shows, then a card shows "Your last pair: side adjusters, turn-up hem, pleated front." and "Use these details", and `fit_flow:suggestion_shown` fires.
- Given the card, when the customer taps "Use these details", then `fit_flow:suggestion_accepted` fires, and the size step opens with the details applied and the size suggested. A size and "Add to bag" make 4 taps, against 9 or more by hand.

## US-17 Pick up on this device

As an identified customer, I want to continue my last selection on this device, so that I do not start again.

- Given a selection saved here in the last 30 days, with consent, when the prompt shows, then a card shows "Pick up where you left off", "Continue", and "Start fresh".
- Given the card, when the customer taps "Continue", then the selection comes back, with the size outlined, not selected. A size and "Add to bag" make 4 taps. Events: `suggestion_accepted`, then `fit_changed` if the saved fit differs, then `size_step_shown`.
- Given no "Continue", when the size step opens, then it outlines the last size with "Last time on this device: 48 Regular".

## US-18 Consent and fail-open for personalization

As a customer, I want my data choices respected and no wait, so that I can trust the flow.

- Given the customer declined personalization consent, when the flow opens, then the baseline shows, and nothing is saved on the device.
- Given a customer lookup that fails or takes more than 300 milliseconds, when the prompt opens, then the baseline shows, with no late card (*not in prototype*).
- Given analytics consent only, when an event fires, then `consent_state` is `analytics_only` and `identity_tier` is `anonymous`.

## US-21 Entry from "Size & Fit" on the product page

As a customer, I want to change the details where I read them, so that I find the options.

- Given `trousers-details-pdp-entry` is on, when the customer opens "Size & Fit", then a boxed control with "Change these details" follows the rows, and the page scrolls it into view.
- Given the control, when the customer taps it, then the sheet opens, and "Done" opens the size step. One changed detail takes 7 taps: "Size & Fit", "Change these details", the option, the value, "Done", a size, and "Add to bag".
- Given either variant, when "Size & Fit" opens, then `fit_flow:size_and_fit_expanded` fires with `has_details_link`.

## Definition of done

- All criteria pass in the phone and the desktop layout.
- With the flag OFF, a regression test shows the flow of today.
- The checks of US-12 and US-13 pass.
- The texts exist in each language of the test market.
- Test orders went through production.
- The baseline shows when the customer lookup is off.

## Out of scope

- A price for a detail.
- New return text. Returns stay allowed under today's policy, and every returned changed pair is a write-off (gap L1).
- A fourth option, free text, measurements, and leg length.
- Full Custom Made. The "More options" pill on the fit photo stays as on the live site.
- Changes after the purchase or from the bag, store alterations, and reorders.
- The app, if its flow differs (gap E1).
- Other garments.
