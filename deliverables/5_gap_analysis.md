# Design options for trousers: gap analysis

Each row pairs a question for one team with the assumption all deliverables use until the answer comes.

## Top five

| ID | Question | Decision it blocks | Working assumption |
|---|---|---|---|
| P1 | Which details can each fit have? | The Release 1 scope | Every value on every fit |
| L1 | Does today's return policy cover a changed pair? | Keep and the money case | Yes. Every returned changed pair is a write-off. |
| D2 | What are today's rates at each step from the prompt? | Test size, length, and every threshold | 50% from the prompt to the bag |
| E2 | Where does a detail live on the order line? | The build and the order route | Attributes on the order line |
| B1 | Can the detail renders of the full Custom Made configurator be reused in the sheet? | The images in the sheet | The real photo shows the fit, and the configurator renders show each detail |

## By team

### Production

Ask the production and supply chain teams.

| ID | Question | Why it matters | Working assumption |
|---|---|---|---|
| P1, P8 | Which waistband, hem, and front can each fit have, and which is standard today? | The sheet offers only what production can make. Today's pair stays the default. | Every value on every fit. Standards as on the product pages. |
| P2, P4 | Is 1-2 weeks true for a changed pair? When does cutting start? | The flow promises the delivery time. Cutting sets any change window (L1). | 1-2 weeks. No change after the order. |
| P7 | How does a turn-up get its final length? | Trousers arrive long, and a tailor shortens them. A turn-up needs its length first. | 4 cm, at the standard inside leg of the size |

### Legal and customer service

Ask the legal, customer service, and returns teams. This is not legal advice.

| ID | Question | Why it matters | Working assumption |
|---|---|---|---|
| L1 | Does today's 30-day return policy cover a changed pair? Is there a change window? | The brief says a changed pair cannot go back into stock. It does not say the customer cannot return it. | Yes. Every returned changed pair is a write-off. The flow adds no return text. |
| L2 | How does legal classify a choice from preset options? | EU guidance says preset options are not "made to the consumer's specifications". | The 14-day withdrawal right probably applies. This agrees with L1. |
| L3 | What happens to a returned changed pair? | It sets the loss per return. The EU ban on destroying unsold clothing can apply. | A full loss, with a reuse route |
| C6 | Can service tag cancellations and contacts with the order line and a reason? | The early signals need a source and a baseline. | Yes |

### Data and analytics

Ask the data, analytics, and finance teams.

| ID | Question | Why it matters | Working assumption |
|---|---|---|---|
| D1, D2 | How many users reach each step each week, and at what rates? | At a 30% add-to-bag rate, the test needs about 72,000 users per group, not 31,000. | 50% from the prompt to the bag |
| D3 | What share of orders has a non-base fit? | It calibrates the 5% Adoption threshold. | Unknown. The 5% stands on three guesses. |
| C1, C5 | What is the return rate of made-to-order fits, and how soon do returns arrive? | Keep compares against made-to-order fits and reads late. | 20%, most within 30 days of delivery |
| D5 | How many trouser buyers order again within 90 days today? | It is the baseline for Come back, the later loyalty metric. | Unknown. Come back reads changed-pair buyers against standard buyers. |
| D4 | Which analytics, experiment, and flag tools exist? Can a return join back to `flag_variant`? | The 12 events must fit the system. Keep needs returns per test group. | The plan's own naming rule |
| F1, P6 | What are the margins, the cost of a return, and the cost of a changed detail? | Every cost in the money case is invented. | Example values in the tracking plan |
| I4, I5 | How many known users open the prompt each month? | The shortcut test (Release 1.1) needs about 9,800 known users per group. | One market reaches it within two months. |

### Engineering

Ask the web store, order system, and account teams.

| ID | Question | Why it matters | Working assumption |
|---|---|---|---|
| E2 | How does the order line store a changed detail, and how does production read it? | Today, each fit is its own product code. | Attributes on the order line. Hem is a new attribute. |
| E5 | Can the flag target one market, and does it fail open to today's flow? | The stop rule turns the flag off within one working day. | Yes, within 15 minutes |
| E8 | Can the bag, confirmation, and emails show the three details? | The customer checks the details after the size step. | Three new rows in the "Custom made details" panel |
| E1 | Is the app in scope? | The app can have its own flow. | Website only |
| I1, I2, I3, I7 | Can the flow read the Size Passport and kept order lines when the prompt opens? Which consent category covers a remembered device? | The shortcuts for known customers need them without blocking the page. | Yes, within 300 milliseconds, under functional or analytics consent |

### Content and scope

Ask the brand and content teams, and the Custom Made product lead.

| ID | Question | Why it matters | Working assumption |
|---|---|---|---|
| B1 | Can the detail renders of the full Custom Made configurator be reused in the sheet? | No image shows a changed combination. The customer buys from the fit photo and a detail render. | The real photo shows the fit, and the configurator renders show each detail. The prototype uses photo crops as stand-ins. |
| B2 | Which plain name and translation fits each value? Who words the expectation-match question? | Most customers do not know what a side adjuster is. | The product page words, plus a one-line benefit |
| M3 | What is the business goal: more orders, higher order value, fewer returns, or a step to full Custom Made? | The success metrics follow the goal. | More orders from customers who want a small change |
| M5 | Should the details cost extra? Do they take orders from full Custom Made? | Full Custom Made costs €229 in this cloth, with 2-3 weeks delivery. A surcharge of about €30 closes the money gap. | No surcharge in Release 1. |

## Sources

1. Suitsupply. Returns & Refunds, FAQ, and Terms and Conditions.
2. Suitsupply. Netherlands web store, own walkthroughs on 23 and 24 November 2026.
3. European Commission (2021). Guidance on Directive 2011/83/EU, section 5.11.2.
4. Autoriteit Consument & Markt (ACM). Bedenktijd.
5. European Union. Regulation (EU) 2024/1781, Article 25.
