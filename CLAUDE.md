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

**Services, carwash, café, and hauling (real menus in Shopify).** Name items and choices only. Never copy prices into the docs.

- Services with changing prices: a ₱1 item, with the amount entered as the quantity.
- Carwash and detailing: staff choose the package, then the size. Car sizes: S (Jazz, Vios), M (Altis, Civic, Tucson), L (Fortuner, Montero, Innova), XL (Land Cruiser, Raptor, Expedition), XXL (HiAce, Alphard, NV350). Motorcycle sizes: MC-S (below 200cc), MC-M (200 to 399cc), MC-L (400cc and up). Panel buffing is per panel and mags detailing is per mag: the quantity is the number of panels or mags. Jobs not on the chart use "Other Detailing" at ₱1 with the amount as the quantity.
- Café (vendor Sixthgear Cafe): drinks use Hot 12oz, Hot 16oz, and Cold 16oz (only the sizes on the menu). Espresso drinks use Single or Double. Fruit frappes use Cream or Yogurt. One-size drinks have the size in the name, for example "Coffee Frappe (16oz)". Extra Shot Espresso and Oat Milk are separate add-on items.
- Hauling: Local Hauling and Long Distance Hauling have one option per destination. Destinations with a price range are set at the highest price, and a supervisor lowers it. Towing Services, Event Logistics, and Other Hauling use ₱1 with the amount as the quantity.
- Space Mission car freshener: 6 scents, sold under Carwash & Detailing.
- Service Department items are not set up yet. Prices are pending.
- Imports: overwrite imports can clear fields that are not in the file, including labels and product type. For changes to existing products, use the bulk editor. Always export a backup before any import.

**Discounts:** 10%, 15%, and 20% are approved by a supervisor (provisional, owner to confirm). The executive discount is approved and set by the owner only. No other discounts are offered. Do not write about Senior Citizen or PWD discounts, except as an item on the Open decisions page.

**Staff never use Custom sale.** It has no category and breaks the daily sales report.

**Hardware:** Samsung Galaxy Tab S10 series tablet, Epson TM-m30III (Wi-Fi + Bluetooth) receipt printer, and the bank's card terminal.

**Online orders (confirmed):**

- Order numbers start with SG, for example SG1001.
- Fulfillment is manual. Staff mark every order as fulfilled.
- Delivery: shipping within the Philippines (Standard rate), or free store pickup at "Shop location", ready in about 4 hours.
- Shopify inventory adjustment reasons: Correction, Count, Received, Return restock, Damaged, Theft or loss, Promotion or donation.

**Reporting:** Shopify saved report "Sixthgear Sales Reporting" (by business unit and category). A Google Sheet Daily Sales Report with Cash / Non-Cash per category is planned. Separately, daily cashflow logs already exist as Excel files covering the five business units. Do not document the Excel files as a procedure yet.

**Opening:** the store opening is planned for around 22 November 2026. The date is not final. The date appears only here and on `launch/checklist` (marked "not final"). Every other page says "before opening" and links to `/launch/checklist`.

## Pending decisions (do not document as final)

- Refund, return, and exchange process (BLOCKING, needed before opening; owner decides). The page `at-the-counter/refunds-and-returns` is hidden until it is decided.
- How the five business units are separated in Shopify (BLOCKING). Do not assume the current structure.
- Staff training: who runs it and when.
- Daily cashflow Excel logs: keep separate or merge into the Daily Sales Report.
- POS Lite or POS Pro. The Shopify plan is Basic with one location, "Shop location" (Sidekick, September 2026). POS Pro status is not verified.
- Which shipping and pickup emails Shopify sends customers.
- Who approves each discount (the current draft says a supervisor approves 10% to 20%).
- The exact name of the website's sales channel.
- Which courier ships online orders.
- Refunds page: the Care of Boss row says 'Nothing to give back'. Update it when the refund process is decided.

## Never

- Add secrets or personal data (see `AGENTS.md`).
- Delete a page that has content. Hide it or add a redirect instead, and ask first.
- Change the navigation of existing tabs without listing every change in the pull request.
