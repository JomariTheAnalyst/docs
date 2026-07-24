# Image capture guide

This file is excluded from publishing. Add an image to MDX only after the reviewed file exists.

## Storefront home

- **Target page:** `index.mdx`
- **Proposed filename:** `/images/overview/sixthgear-storefront-home.webp`
- **Capture:** Verified production or local SixthGear homepage
- **Show:** Recognizable hero and main navigation
- **Hide or blur:** Personal account information, preview toolbars and development overlays
- **Crop:** Wide 16:9, approximately 1200 x 675
- **Caption:** "SixthGear storefront homepage covered by this guide."
- **Alt text:** "SixthGear storefront homepage with the main hero and navigation."
- **Light/dark versions:** No; use one representative storefront capture

## System architecture

- **Target page:** `website/system-architecture.mdx`
- **Proposed filename:** `/images/overview/system-architecture.png`
- **Create:** Export the reviewed Mermaid diagram if a static asset is needed
- **Show:** Browser, Next.js, Shopify, hosted checkout, Sanity, Strapi, Redis, Vercel and GitHub
- **Hide or blur:** No account or environment identifiers
- **Crop:** Tight landscape diagram with readable labels
- **Caption:** "How SixthGear storefront services connect."
- **Alt text:** "Architecture diagram connecting the customer browser to Next.js, Shopify, Sanity, Strapi, Redis and Vercel."
- **Light/dark versions:** Consider both if the exported background is not transparent

## Add a Shopify product

- **Target page:** `shopify/add-edit-products.mdx`
- **Proposed filename:** `/images/shopify/add-product.png`
- **Capture:** Shopify Admin > **Products** > **Add product**
- **Show:** Title, description, media, pricing, inventory, variants and metafields
- **Hide or blur:** Account avatar, store identifiers, billing information, tokens and private product data
- **Crop:** Product editor area used by the procedure
- **Caption:** "The Shopify product editor contains product, variant and metafield data."
- **Alt text:** "Shopify product editor with product details, pricing, inventory, variants and metafields."
- **Light/dark versions:** No

## Collections

- **Target page:** `shopify/collections.mdx`
- **Proposed filename:** `/images/shopify/collections-list.png`
- **Capture:** Shopify Admin > **Products** > **Collections**
- **Show:** Collection list and manual or automatic collection controls
- **Hide or blur:** Unrelated store information and account controls
- **Crop:** Collection list and type or condition area
- **Caption:** "Collections control product groupings used by the storefront."
- **Alt text:** "Shopify Collections page showing collection records and collection controls."
- **Light/dark versions:** No

## Metafield definitions

- **Target page:** `shopify/metafields.mdx`
- **Proposed filename:** `/images/shopify/metafield-definitions.png`
- **Capture:** Shopify Admin > **Settings** > **Custom data** > **Products**
- **Show:** SixthGear product metafield definitions and namespace/key information
- **Hide or blur:** Secret values, unrelated accounts and private store information
- **Crop:** Definition list and selected definition fields
- **Caption:** "Product metafield definitions must match the namespaces and keys used by code."
- **Alt text:** "Shopify product metafield definitions with field names, namespaces and keys."
- **Light/dark versions:** No

## Product import

- **Target page:** `shopify/bulk-import.mdx`
- **Proposed filename:** `/images/shopify/product-import-dialog.png`
- **Capture:** Shopify Admin product import workflow
- **Show:** CSV upload and import review interface
- **Hide or blur:** Local file paths with personal information, emails and store identifiers
- **Crop:** Import dialog and review controls
- **Caption:** "Review the CSV import summary before starting a bulk product import."
- **Alt text:** "Shopify product CSV import dialog with upload and review controls."
- **Light/dark versions:** No

## Sanity Studio navigation

- **Target page:** `sanity/open-cms.mdx`
- **Proposed filename:** `/images/sanity/studio-navigation.png`
- **Capture:** SixthGear CMS at `/studio`
- **Show:** Blog, Homepage Settings, Marketing, Services Page, About Page and Services
- **Hide or blur:** Personal account menu and unnecessary project identifiers
- **Crop:** Studio navigation and document workspace
- **Caption:** "SixthGear CMS organizes core singletons and content lists in Studio."
- **Alt text:** "Sanity Studio navigation for SixthGear CMS."
- **Light/dark versions:** Only if both Studio modes are part of the standard workflow

## Sanity content editor

- **Target page:** `sanity/edit-publish-content.mdx`
- **Proposed filename:** `/images/sanity/edit-homepage-content.png`
- **Capture:** A representative homepage document
- **Show:** Editable fields, image field, validation and **Publish** control
- **Hide or blur:** Personal account menu, private project IDs and confidential campaign content
- **Crop:** Fields used by the procedure and publish status
- **Caption:** "Review fields and validation before publishing a Sanity document."
- **Alt text:** "Sanity document editor with homepage fields, validation and Publish control."
- **Light/dark versions:** No

## SixthGear CMS invitation walkthrough

### Invitation in the inbox

- **Target page:** `sanity/get-access.mdx`
- **Final filename:** `/images/sanity/access/01-invitation-inbox.png`
- **Where to capture it:** The inbox for the account that received the Sanity invitation
- **What to show:** The Sanity sender, invitation subject, and enough inbox context to identify the message
- **What to hide:** Personal email addresses, unrelated messages, avatars, labels, and notifications
- **Recommended crop:** The invitation row with limited surrounding inbox context
- **Caption:** "Reserved screenshot: Inbox showing the Sanity project invitation"
- **Alt text:** "Reserved screenshot location for an inbox with the Sanity project invitation highlighted"

### Opened invitation email

- **Target page:** `sanity/get-access.mdx`
- **Final filename:** `/images/sanity/access/02-open-invitation-email.png`
- **Where to capture it:** The opened email sent by Sanity
- **What to show:** The Sanity sender, SixthGear project name, and **Accept invitation** button
- **What to hide:** Personal inbox content, personal email addresses, other messages, avatars, and private browser information
- **Recommended crop:** The sender, project invitation text, and **Accept invitation** button
- **Caption:** "Reserved screenshot: Opened Sanity email with the SixthGear project invitation"
- **Alt text:** "Reserved screenshot location for the opened Sanity invitation showing the project name and Accept invitation button"

### Accept invitation email button

- **Target page:** `sanity/get-access.mdx`
- **Final filename:** `/images/sanity/access/03-accept-invitation-email-button.png`
- **Where to capture it:** The invitation email immediately before selecting **Accept invitation**
- **What to show:** The SixthGear project context and **Accept invitation** button
- **What to hide:** Personal email addresses, unrelated messages, avatars, notifications, and private browser information
- **Recommended crop:** The invitation text and **Accept invitation** control
- **Caption:** "Reserved screenshot: Accept invitation button in the Sanity email"
- **Alt text:** "Reserved screenshot location for the Accept invitation button inside the Sanity email"

### Sanity account check

- **Target page:** `sanity/get-access.mdx`
- **Final filename:** `/images/sanity/access/04-check-sanity-account.png`
- **Where to capture it:** The Sanity invitation page showing the current account
- **What to show:** The current account, SixthGear project name, and **Switch accounts** option
- **What to hide:** The full personal email where unnecessary, project ID, organization ID, account avatar, and private browser information
- **Recommended crop:** The account summary, project name, and **Switch accounts** control
- **Caption:** "Reserved screenshot: Sanity invitation screen showing the current account"
- **Alt text:** "Reserved screenshot location for the Sanity invitation account check and Switch accounts option"

### Sign-in method

- **Target page:** `sanity/get-access.mdx`
- **Final filename:** `/images/sanity/access/05-select-sign-in-method.png`
- **Where to capture it:** The Sanity sign-in method selection page
- **What to show:** The supported sign-in choices and the method used by the invited identity
- **What to hide:** Personal email addresses, account avatars, browser profiles, and private browser information
- **Recommended crop:** The sign-in controls and enough Sanity branding to identify the page
- **Caption:** "Reserved screenshot: Sanity sign-in method selection"
- **Alt text:** "Reserved screenshot location for choosing the Google or established Sanity sign-in method"

### Invited account selection

- **Target page:** `sanity/get-access.mdx`
- **Final filename:** `/images/sanity/access/06-select-invited-account.png`
- **Where to capture it:** The Google or authentication-provider account chooser
- **What to show:** Only the invited account and **Use another account** option when needed
- **What to hide:** Unrelated provider accounts, full personal email addresses where unnecessary, avatars, and private browser information
- **Recommended crop:** The invited account choice and relevant continuation control
- **Caption:** "Reserved screenshot: Account chooser with the invited identity selected"
- **Alt text:** "Reserved screenshot location for selecting the exact account that received the SixthGear invitation"

### Sign-in approval

- **Target page:** `sanity/get-access.mdx`
- **Final filename:** `/images/sanity/access/07-approve-google-sign-in.png`
- **Where to capture it:** The Google or authentication-provider approval screen
- **What to show:** The selected invited identity, Sanity sign-in request, and **Continue** or equivalent button
- **What to hide:** Full personal email where unnecessary, unrelated accounts, recovery details, avatars, and private browser information
- **Recommended crop:** The account confirmation and approval control
- **Caption:** "Reserved screenshot: Authentication approval for the invited account"
- **Alt text:** "Reserved screenshot location for approving Sanity sign-in with the invited account"

### Final invitation acceptance

- **Target page:** `sanity/get-access.mdx`
- **Final filename:** `/images/sanity/access/08-final-accept-invitation.png`
- **Where to capture it:** The final Sanity project invitation confirmation
- **What to show:** The SixthGear project name, correct account context, and **Accept invitation** button
- **What to hide:** Full personal email where unnecessary, project ID, organization ID, account avatar, and private browser information
- **Recommended crop:** The project confirmation, account context, and final acceptance control
- **Caption:** "Reserved screenshot: Final SixthGear project invitation confirmation"
- **Alt text:** "Reserved screenshot location for the final Sanity project invitation acceptance screen"

### First Studio view

- **Target page:** `sanity/get-access.mdx`
- **Final filename:** `/images/sanity/access/09-first-studio-view.png`
- **Where to capture it:** SixthGear CMS immediately after successful invitation acceptance
- **What to show:** The **Sixthgear CMS** title and expected content navigation
- **What to hide:** Personal email addresses, account avatar, project ID, dataset ID, unpublished content, and private browser information
- **Recommended crop:** The Studio title, navigation, and enough workspace context to confirm successful access
- **Caption:** "Reserved screenshot: First successful view of SixthGear CMS"
- **Alt text:** "Reserved screenshot location for the first successful SixthGear CMS view with content navigation visible"

## Vercel deployments

- **Target page:** `deployment/deploy-through-vercel.mdx`
- **Proposed filename:** `/images/deployment/vercel-deployments.png`
- **Capture:** Verified Vercel project deployment list
- **Show:** Production and Preview status, commit reference and deployment state
- **Hide or blur:** Account email, team billing, environment values and private URLs when required
- **Crop:** Deployment list and relevant status controls
- **Caption:** "Use the Vercel deployment list to confirm environment and release status."
- **Alt text:** "Vercel deployment list showing Production and Preview deployment states."
- **Light/dark versions:** No
