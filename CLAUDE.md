# Claude Code instructions: SixthGear docs repo

Read and follow these two files first. They are the main rules for this repo.

@AGENTS.md
@STYLE_GUIDE.md

## Roles

- The project owner is the decision maker and reviews every change.
- Claude Code writes and edits the documentation.
- Never merge a pull request and never push to `main`.

## Workflow for every task

1. Start from the latest `main`: `git checkout main`, `git pull`, then `git checkout -b docs/<short-topic>`.
2. Before writing a page, search the repo for pages on the same topic. If one exists, improve and merge into it instead of creating a duplicate. List any duplicates you find in your report.
3. Preview with `mint dev` when layout matters.
4. After changes, run:
   - `mint validate`
   - `mint broken-links --check-anchors --check-redirects`
   - `mint a11y`
   Fix every new error. Report old errors you did not cause, but do not fix them unless asked.
5. Commit with a clear message, push the branch, and open a pull request. Then stop and report: pages added, pages changed, placeholders added, and anything left for a decision.

## Writing rules for staff pages

Applies to the **At the Counter**, **Products and Stock**, and **Online Orders** tabs.

- Follow "Plain language for store staff" in `STYLE_GUIDE.md`. No em dashes. No jargon. One action per step.
- Add a screenshot or video placeholder wherever one is needed, using the format and file naming in `STYLE_GUIDE.md`. Folders match the tab: `images/at-the-counter/`, `images/products-and-stock/`, `images/online-orders/`, and the same under `videos/`.
- Put anything not yet checked on the real screen, or still waiting for a decision, in a hidden MDX comment: `{/* ... */}`. Never present a guessed screen label as fact without that comment.
- Until its name is confirmed, call the website's channel "the website's sales channel."

## Business facts (decided)

**Business units and report categories.** Every product has two metafield labels:

- `custom.sixthgear_business_unit`: `Retail`, `Service Department`, `Carwash & Detailing`, `Sixth Gear Cafe`, `Bike Hauling & Towing`
- `sixthgear.report_category`: the 35 categories listed on `products-and-stock/overview`
- Label values use "Cafe" without the accent. Shopify rejects any value not on the list.

**Payment methods at the counter:** `Cash`, `QR PH`, `Card Terminal`, `Care of Boss`.

- Card payments run on the bank's terminal. Shopify only records them. Shopify Payments is not used.
- Care of Boss: the owner treats a guest and pays the store back later. It IS a sale, at full price, in its normal category. It needs the owner's approval and a note on the sale. If the owner pays back in cash into the drawer, record it on the cash drawer screen as cash added, with the reason 'Care of Boss reimbursement, order #____'.
- Mark unpaid: staff must never use it. The owner is still deciding whether to hide the button.
- Split payments are allowed.
- QR PH: staff complete the sale only after the money arrives in the store's app. Never accept a customer's screenshot as proof.

**Trial period:** every sale is recorded in both Loyverse and Shopify POS. Loyverse is the official record, and the customer keeps the Loyverse receipt.

**Services, carwash, and hauling:**

- Services with changing prices: a ₱1 item, with the amount entered as the quantity.
- Motowash has no variants. Carwash variants are Carwash Small, Carwash Medium, Carwash Large, and Carwash XL.
- Hauling destinations are variants. Destinations with a price range are set at the highest price, and a supervisor lowers it.

**Discounts:** 10%, 15%, and 20% are approved by a supervisor. The executive discount is approved and set by the owner only. No other discounts are offered. Do not write about Senior Citizen or PWD discounts.

**Staff never use Custom sale.** It has no category and breaks the daily sales report.

**Hardware:** Samsung Galaxy Tab S10 series tablet, Epson TM-m30III (Wi-Fi + Bluetooth) receipt printer, and the bank's card terminal.

**Online orders (confirmed):**

- Order numbers start with SG, for example SG1001.
- Fulfillment is manual. Staff mark every order as fulfilled.
- Delivery: shipping within the Philippines (Standard rate), or free store pickup at "Shop location", ready in about 4 hours.
- Shopify inventory adjustment reasons: Correction, Count, Received, Return restock, Damaged, Theft or loss, Promotion or donation.

**Reporting:** Shopify saved report "Sixthgear Sales Reporting" (by business unit and category). A Google Sheet Daily Sales Report with Cash / Non-Cash per category is planned.

## Pending decisions (do not document as final)

- Refund, return, and exchange process. The page `at-the-counter/refunds-and-returns` is hidden until management decides.
- Who approves each discount (the current draft says a supervisor approves 10% to 20%).
- The exact name of the website's sales channel.
- Which courier ships online orders.

## Never

- Add secrets or personal data (see `AGENTS.md`).
- Delete a page that has content. Hide it or add a redirect instead, and ask first.
- Change the navigation of existing tabs without listing every change in the pull request.
