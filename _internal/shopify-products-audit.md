# Shopify Products audit

Internal working document. Do not publish.

Audit date: 2026-07-22  
Repository reviewed: `sixthgear-shopify`  
Shopify Admin access used: No  
Shopify data or settings changed: No

## Evidence labels

- **Verified in code**: Confirmed in the current repository.
- **User-provided but unverified**: Supplied in the documentation prompt but not checked in Shopify Admin.
- **Requires dashboard verification**: Cannot be established from the repository.
- **Not found**: No matching repository reference was found in the searched source.
- **Recommended editorial standard**: A documentation recommendation based on verified frontend behavior, not a Shopify setting.

## A. Product editor field inventory

The repository establishes what the Storefront API requests and what the application consumes. It does not establish the exact current Shopify Admin layout, field values, product rules, or publication settings.

| Field | Shopify location | Data type | Required | Used by Next.js | Notes |
| --- | --- | --- | ---: | ---: | --- |
| Product ID | System-managed product record | Shopify global ID | System-generated | Yes | Used as a stable identifier. Not a staff-entered field. |
| Title | Product editor, title | Single-line text | Yes for a normal Shopify product | Yes | Queried and rendered across product cards, search, metadata, and product pages. |
| Description | Product editor, description | Rich text represented as plain text and HTML through GraphQL | Not confirmed as a SixthGear rule | Yes | The detail page prefers `descriptionHtml` and falls back to plain `description`. |
| Media | Product editor, media | Ordered media collection | Not confirmed | Yes, for images | The active product query requests `featuredImage`, the first 20 product images, and variant images. It does not request Shopify native video media. |
| Category | Product editor, category | Shopify taxonomy reference | Not confirmed | Not as a native category field | The query requests several `shopify.*` taxonomy metafields, but no native category field was found in the active product query. |
| Product type | Product editor, product organization | Single-line text | Not confirmed | Yes | Used in product data and custom recommendation ranking. Current values require dashboard verification. |
| Vendor | Product editor, product organization | Single-line text | Not confirmed | Yes | Displayed and used in recommendation ranking. Some list UI falls back to `Sixthgear` when vendor is absent. |
| Tags | Product editor, product organization | List of text values | Not confirmed | Yes | Used in product data and custom recommendation ranking. No approved tag naming convention was found. |
| Collections | Product organization and collection membership | Product-to-collection relationship | Not confirmed | Yes, indirectly | Collection queries determine which products appear in collection views. The product-detail query does not request collection membership. |
| Status | Product status | Shopify status enum | Operationally required to publish; exact rule unverified | Indirectly | The storefront query does not request status. Status affects whether Shopify exposes a product, but its exact production interaction requires dashboard verification. |
| Sales-channel availability | Publishing or sales-channel availability | Channel publication relationships | Operationally required for at least one production channel; channel unknown | Indirectly | The repository confirms Storefront API usage but not which named Shopify sales channel supplies production data. |
| SEO title | Search engine listing | Optional text | No code-level requirement | Yes | Used for page metadata when present; falls back to product title. |
| SEO description | Search engine listing | Optional text | No code-level requirement | Yes | Used for page metadata when present; falls back to product description. |
| URL handle | Search engine listing and product URL | Handle string | Required by routing; Shopify can generate it | Yes | The route is `/products/[handle]`; handles are also used in cards, search results, and recommendation links. Redirect behavior requires dashboard verification. |
| Options | Variants section | List of option names and values | Only for products with variants | Yes | Size and color receive specialized controls; arbitrary option names use the general selector. Current option names require dashboard verification. |
| Variants | Variants section | Product variant objects | At least Shopify's default variant exists | Yes | The detail query requests the first 100 variants; card data requests the first 50. |
| Price | Pricing and variant pricing | Money value with currency code | Required for sale; no SixthGear rule confirmed | Yes | Variant price and product price ranges are rendered. |
| Compare-at price | Pricing and variant pricing | Optional money value | No | Yes | Rendered when present and higher than the selling price. |
| Availability | Variant and product availability | Boolean | System-derived | Yes | `availableForSale` controls purchase availability and out-of-stock UI. |
| SKU | Variant inventory details | Optional text | No verified SixthGear rule | Requested, not visibly consumed | Present in the detail query and TypeScript type. No React rendering or business-rule consumer was found. |
| Barcode | Variant inventory details | Optional text | No verified SixthGear rule | No | Not requested, mapped, or rendered in the searched source. |
| Inventory quantity | Inventory by location | Integer by inventory item and location | No code-level requirement | Not directly | `quantityAvailable` exists in a type but is omitted from the active GraphQL query. The UI derives a proxy from availability. |
| Variant image | Variant media selection | Image reference | No | Yes | A selected variant image is moved to the front of the product gallery. |
| Product metafields | Metafields | Typed namespace/key values | Definition-specific; none confirmed mandatory | Yes, selected keys | The query requests explicit keys. Existence, type, Storefront access, and business requirements still require dashboard verification. |
| Recommendations | Shopify recommendation service plus local rules | Product connections | No | Yes | Shopify `RELATED` recommendations and a custom vendor/type/tag fallback are both implemented. |

## B. Variant and inventory audit

### Product options

- **Verified in code:** `src/modules/products/components/product-actions/index.tsx` supports any Shopify option name and gives specialized controls to option names containing `size`, `color`, or `colour`.
- **Verified in code:** An option with variant images can use a visual image selector.
- **Requires dashboard verification:** Which option names and values current SixthGear products actually use.

### Variant fields and behavior

| Item | Repository finding | Verification status |
| --- | --- | --- |
| Variant fields requested | ID, title, SKU, availability, selected options, price, compare-at price, and image | Verified in code |
| SKU usage | Requested and typed; no visible React consumer or validation rule found | Verified in code; convention requires owner decision |
| SKU uniqueness | No enforcement found in the storefront | Do not claim; requires Shopify/business verification |
| Barcode usage | Not requested or consumed | Not found |
| Price | Variant price and product price ranges are displayed | Verified in code |
| Compare-at price | Optional; displayed when applicable | Verified in code |
| Availability | `availableForSale` controls purchase state | Verified in code |
| Inventory quantity | Not requested by the active product query | Verified in code |
| Inventory proxy | The product route maps an available variant to `10` and an unavailable variant to `0` when quantity is missing | Verified in code; this is not actual stock |
| Inventory policy | Product route UI mapping sets `allow_backorder` to `false`; Shopify's configured policy is not queried | UI behavior verified; dashboard policy unverified |
| Location names | No current location names found in the product source | Requires dashboard verification |
| Variant-specific media | Variant image is requested and can become the first gallery image after selection | Verified in code |
| Weight and shipping fields | Native variant weight and shipping fields are not requested | Not consumed by the active query |
| Out-of-stock behavior | Unavailable variants map to zero, show an out-of-stock purchase state, and cannot be added to cart | Verified in code |
| Low-stock behavior | The UI can show a low-stock message, but the current empty inventory map and availability proxy do not supply real counts | Verified limitation |

`src/lib/shopify/queries/collection.ts` explicitly omits `quantityAvailable` because the configured token lacks the needed inventory permission. `src/lib/data/products.ts` currently returns an empty inventory map and relies on Shopify availability.

## C. Product media audit

| Area | Finding | Evidence class |
| --- | --- | --- |
| Product-card aspect ratio | Square container (`aspect-square`) | Verified frontend requirement |
| Product-card fit | `object-contain` with internal padding; the second image can appear on hover | Verified frontend requirement |
| Product-card dimensions | Responsive image sizes target roughly 50vw, 33vw, and 25vw by breakpoint | Verified frontend behavior |
| Product-detail aspect ratio | Mobile main image uses 4:5; desktop uses approximately 3.5:4 | Verified frontend requirement |
| Product-detail fit | Main images use `object-contain` with padding | Verified frontend requirement |
| Thumbnails | Square, `object-cover`, about 72px on mobile and 88px on desktop; six are shown before a remaining-count indicator | Verified frontend behavior |
| Mobile gallery | Swipe navigation and position dots replace the desktop thumbnail rail | Verified frontend behavior |
| Lightbox | Uses `object-contain`, keyboard arrows, and zoom behavior | Verified frontend behavior |
| Native Shopify video | The active query requests images, not Shopify native product media videos | Not consumed |
| Custom video | `custom.product_video_url` can render a 16:9 iframe; YouTube and Vimeo links are transformed to embed URLs | Verified frontend behavior; definition and values require dashboard verification |
| Alternative text | Shopify image `altText` is used. The detail route can fall back to vendor plus product title. Some decorative thumbnails use empty alt text | Verified frontend behavior |
| Missing image | Cards and the product gallery render placeholders; failed gallery images move to another valid image where possible | Verified frontend behavior |
| Source dimensions | Missing GraphQL width or height defaults to 1600×1600 in code, but that is a rendering fallback, not an upload standard | Verified frontend behavior; not an editorial requirement |

### Recommended editorial standard

Use a square or near-square high-resolution primary image with the product centered and enough clear margin around it. This recommendation follows the square product cards and `object-contain` behavior. Keep a consistent subject scale across the catalog. Preserve the highest practical source quality for gallery zoom.

Do not publish a fixed pixel requirement yet. The frontend contains several layout and zoom thresholds, but the repository does not establish an approved Shopify upload dimension. The owner must approve the staff-facing image dimensions.

## D. Sales-channel audit

The six names below are user-provided dashboard information. The repository confirms only that the application uses Shopify's Storefront API; it does not identify the named channel tied to the production token.

| Channel | Visible in dashboard | Intended use | Required for live Next.js storefront | Publishing rule | Verification status |
| --- | ---: | --- | ---: | --- | --- |
| Online Store | User-provided, not independently verified | Shopify-hosted online storefront or theme channel | Unknown | Do not assume it controls the headless storefront | Requires dashboard verification |
| Point of Sale | User-provided, not independently verified | In-person sales | Unknown | Product availability should follow approved POS operations | Requires dashboard verification |
| Sixthgear Moto | User-provided, not independently verified | Name suggests a custom app or channel; purpose unknown | Unknown | No rule can be derived from the repository | Requires dashboard verification |
| Sixthgear Moto Headless 02 | User-provided, not independently verified | Name suggests a headless storefront integration; current role unknown | Unknown | Do not mark required until the production token or channel is confirmed | Requires dashboard verification |
| Inbox | User-provided, not independently verified | Shopify Inbox product sharing or chat workflow | Unknown | Do not assume every product must publish here | Requires dashboard verification |
| Facebook & Instagram | User-provided, not independently verified | Meta commerce channel | Unknown | Product eligibility and publishing scope require an owner-approved rule | Requires dashboard verification |

Repository-confirmed facts:

- The Storefront API version defaults to `2025-10` in `src/lib/env.ts` and `src/lib/shopify/client.ts`.
- The application requires Shopify store-domain and Storefront API token configuration names.
- No repository evidence maps the production credentials to one of the six displayed channel names.

## E. Metafield definition audit

Code use does not prove that a definition currently exists, has the expected Shopify type, is exposed to the Storefront API, or is safe for staff to edit.

### Proposed custom definitions

| Display name | Namespace and key | Shopify type | Definition exists | Storefront API access | Used in code | Managed by | Safe to edit |
| --- | --- | --- | ---: | ---: | ---: | --- | ---: |
| Size chart | `custom.size_chart` | Requires dashboard verification; resolver supports image/reference/URL-shaped values | Unknown | Unknown | Yes: requested, resolved, and rendered | Proposed custom field; owner unconfirmed | No, until definition and ownership are verified |
| Size chart data | `custom.size_chart_data` | Requires dashboard verification; code expects structured JSON | Unknown | Unknown | Yes: requested, parsed, and rendered | Proposed custom field; owner unconfirmed | No, until schema and ownership are verified |
| What's in the box | `custom.what_is_in_the_box` | Requires dashboard verification | Unknown | Unknown | Yes: requested, resolved, and rendered | Proposed custom field; owner unconfirmed | No, until definition and format are verified |
| Product video URL | `custom.product_video_url` | Requires dashboard verification; code expects a URL-like value | Unknown | Unknown | Yes: requested and rendered in a video tab | Proposed custom field; owner unconfirmed | No, until supported providers and ownership are verified |
| Specifications | `custom.specifications` | Requires dashboard verification; renderer supports structured rich-text-like content | Unknown | Unknown | Yes: requested, parsed, and rendered | Proposed custom field; owner unconfirmed | No, until schema and ownership are verified |
| Shipping | `custom.shipping` | Requires dashboard verification | Unknown | Unknown | Yes: requested and rendered with fallback shipping content | Proposed custom field; owner unconfirmed | No, until definition and ownership are verified |

### Other custom keys requested by the storefront

The active product query also requests the following `custom.*` keys:

| Namespace and key | Request status | Render status | Dashboard verification needed |
| --- | --- | --- | --- |
| `custom.care_instructions` | Requested | Available to the generic product-information renderer | Definition, type, access, ownership, and safe-edit rule |
| `custom.size_guide` | Requested | No dedicated resolver confirmed | Definition, type, access, and whether this duplicates the size-chart fields |
| `custom.material` | Requested | Available to product-information rendering | Definition, type, access, and overlap with `shopify.material` |
| `custom.weight` | Requested | Available to product-information rendering | Definition, unit, type, and whether native variant weight is also maintained |
| `custom.dimensions` | Requested | Available to product-information rendering | Definition, format, and unit |
| `custom.height` | Requested | Available to product-information rendering | Definition, type, and unit |
| `custom.width` | Requested | Available to product-information rendering | Definition, type, and unit |
| `custom.length` | Requested | Available to product-information rendering | Definition, type, and unit |
| `custom.color` | Requested | Available to product-information rendering | Definition, type, and overlap with options or Shopify taxonomy fields |
| `custom.gender` | Requested | Available to product-information rendering | Definition, type, and overlap with Shopify taxonomy fields |
| `custom.age_group` | Requested | Available to product-information rendering | Definition, type, and overlap with Shopify taxonomy fields |
| `custom.brand` | Requested | Available to product-information rendering | Definition, type, and overlap with vendor |
| `custom.country_of_origin` | Requested | Available to product-information rendering | Definition, type, and approved content |
| `custom.protection_level` | Requested | Available to product-information rendering | Definition, type, and approved terminology |
| `custom.certification` | Requested | Available to product-information rendering | Definition, type, and evidence requirements |

An alternate query in `src/lib/shopify/queries/products.ts` requests `custom.details`. It is not part of the active detail query reviewed for the product route. Confirm whether that query remains in use before documenting the field.

### Shopify-standard proposed definitions

| Display name | Namespace and key | Shopify type | Definition exists | Storefront API access | Used in code | Managed by | Safe to edit |
| --- | --- | --- | ---: | ---: | ---: | --- | ---: |
| Color | `shopify.color` | Shopify taxonomy metafield type unverified | Unknown | Unknown | Requested; available to product-information rendering | Shopify-standard proposal | Requires dashboard verification |
| Accessory size | `shopify.accessory_size` | Unverified | Unknown | Unknown | Requested; available to rendering | Shopify-standard proposal | Requires dashboard verification |
| Material | `shopify.material` | Unverified | Unknown | Unknown | Requested; available to rendering | Shopify-standard proposal | Requires dashboard verification |
| Age group | `shopify.age_group` | Unverified | Unknown | Unknown | Requested; available to rendering | Shopify-standard proposal | Requires dashboard verification |
| Target gender | `shopify.target_gender` | Unverified | Unknown | Unknown | Requested; available to rendering | Shopify-standard proposal | Requires dashboard verification |
| Handwear material | `shopify.handwear_material` | Unverified | Unknown | Unknown | Requested; available to rendering | Shopify-standard proposal | Requires dashboard verification |
| Size | `shopify.size` | Unverified | Unknown | Unknown | Requested; available to rendering | Shopify-standard proposal | Requires dashboard verification |
| Gender | `shopify.gender` | Unverified | Unknown | Unknown | Requested; available to rendering | Shopify-standard proposal | Requires dashboard verification |

### App-managed and suspicious definitions

| Display name | Namespace and key | Shopify type | Definition exists | Storefront API access | Used in code | Managed by | Safe to edit |
| --- | --- | --- | ---: | ---: | ---: | --- | ---: |
| Review rating | `reviews.rating` | Unverified | Unknown | Unknown | Yes: requested and parsed for aggregate ratings | Likely review integration; live owner unconfirmed | No until app ownership is verified |
| Review count | `reviews.rating_count` | Unverified | Unknown | Unknown | Yes: requested and parsed for aggregate ratings | Likely review integration; live owner unconfirmed | No until app ownership is verified |
| Protective gear features | `shopify.proTECTIVE-GEAR-FEATURES` | Unknown | Unknown | Unknown | No source reference found | Unknown | No |

Keep `shopify.proTECTIVE-GEAR-FEATURES` unchanged in the audit until Shopify Admin confirms the exact namespace and key. Its casing is suspicious, but the repository provides no evidence for a correction.

## F. App-managed metafield audit

| Proposed owner | Code reference | Environment variable names | Storefront consumption | Manual-edit guidance | Dashboard verification |
| --- | --- | --- | --- | --- | --- |
| Judge.me | `src/lib/data/reviews.ts` implements Judge.me API reads and review-submission links; product review UI exists in `src/modules/products/components/product-reviews/` | `JUDGEME_PRIVATE_TOKEN`, `NEXT_PUBLIC_JUDGEME_PUBLIC_TOKEN`, `NEXT_PUBLIC_JUDGEME_SHOP_DOMAIN` are named in `.env.example` | `reviews.rating` and `reviews.rating_count` are queried and parsed; separate review data is fetched through the Judge.me API | Avoid manual edits until ownership is confirmed | Confirm installation, live configuration, metafield ownership, synchronization, and edit policy |
| Shopify Search & Discovery | No explicit app-name, package, or environment reference found | None found | The code calls Shopify `productRecommendations(intent: RELATED)` and also uses custom vendor/type/tag recommendation rules | No app-owned metafield edit rule can be established | Confirm installation, recommendation configuration, and whether any fields are app-managed |
| Goodchart | No source, package, environment, or namespace reference found | None found | No consumption identified | Do not claim ownership or edit fields attributed to it | Confirm installation, field ownership, and whether it is still used |

Environment variable **names only** were reviewed from `.env.example`. No environment value or secret file was read.

## G. Safe example product

The proposed example was not found in the searched repository and was not checked in Shopify Admin. Keep every value internal until an owner confirms the product is accurate, current, suitable for training, and free of sensitive supplier information.

| Value | Proposed data | Status |
| --- | --- | --- |
| Product name | AKRAPOVIC EVOLUTION TITANIUM&CARBON YZF-R1 15 FULLSYSTEM | User-provided but unverified |
| Product ID | `8233171058762` | User-provided but unverified |
| Vendor | AKRAPOVIC | User-provided but unverified |
| Product type | Parts and Accessories | User-provided but unverified |
| SKU | `S-Y10E2-HX2C` | User-provided but unverified |
| Price | ₱138,000.00 | User-provided but unverified |
| Inventory | 1 | User-provided but unverified |

Verification classification:

- **Verified:** None.
- **User-provided but unverified:** All proposed values above.
- **Not found:** No matching repository content was found.
- **Outdated:** Unknown until Shopify Admin is checked.

Do not add product cost, supplier terms, customer information, or other sensitive operational data to the public guide.

## H. Product count and store snapshot

| Snapshot value | User-provided value | Verification date | Status |
| --- | ---: | --- | --- |
| Products | 364 | Not verified | Dashboard snapshot only |
| Collections | 19 | Not verified | Dashboard snapshot only |
| User accounts | 2 | Not verified | Dashboard snapshot only |
| Location | Shop location — 1 active | Not verified | Dashboard snapshot only |

These are changing operational totals, not permanent rules. Do not feature them in public Products guides unless the owner approves a dated snapshot and the values are rechecked.

## I. Required screenshots

All public pages must use `/images/placeholders/screenshot-reserved.svg` until reviewed screenshots are supplied.

| Target page | Final filename | Where to capture it | What to show | What to hide | Recommended crop | Caption | Alt text |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Products overview | `/images/shopify/products/products-list.png` | Shopify Admin → Products | Product list, status, inventory summary, vendor, and product type where visible | Account identity, customers, notifications, browser chrome | Main Products workspace | Shopify Products list | Shopify Admin Products list showing product status and organization columns |
| Add a product | `/images/shopify/products/add-product-button.png` | Shopify Admin → Products | Add product control and surrounding page heading | Account identity, notifications, browser chrome | Header and primary action | Add product button | Add product button in the Shopify Products workspace |
| Add a product | `/images/shopify/products/title-description.png` | New or approved example product | Title and description controls with safe example content | Supplier-sensitive details, personal data, unrelated panels | Core product information card | Product title and description | Product title and description fields in Shopify Admin |
| Product media | `/images/shopify/products/product-media.png` | Product editor → Media | Media grid, ordering controls, and approved assets | Upload dialogs containing local paths, unrelated product data | Media card | Product media workspace | Product media arranged in the Shopify product editor |
| Organization and sales channels | `/images/shopify/products/category-organization.png` | Product editor → Product organization | Category, product type, vendor, tags, and collections where available | Unrelated settings and personal data | Organization panel | Product organization fields | Category, product type, vendor, tags, and collections fields |
| Variants, pricing, and inventory | `/images/shopify/products/pricing.png` | Product or variant editor → Pricing | Price and compare-at price | Cost or margin data unless explicitly approved | Pricing card only | Product pricing | Price and compare-at price fields in Shopify Admin |
| Variants, pricing, and inventory | `/images/shopify/products/inventory.png` | Product or variant editor → Inventory | SKU and approved inventory controls | Supplier data, cost, unrelated locations | Inventory card only | Product inventory | SKU and inventory controls for a Shopify product variant |
| Variants, pricing, and inventory | `/images/shopify/products/variants.png` | Product editor → Variants | Option names, values, and variant list | Sensitive SKUs if the example is not approved | Variants card | Product variants | Product option values and variants in Shopify Admin |
| Variants, pricing, and inventory | `/images/shopify/products/shipping.png` | Product or variant editor → Shipping | Physical-product, weight, and shipping controls that exist in the dashboard | Cost and supplier information | Shipping card only | Product shipping fields | Shipping and weight fields for a Shopify product |
| Product metafields | `/images/shopify/products/metafields.png` | Product editor → Metafields | Only verified definitions included in the guide | App tokens, hidden fields, sensitive custom data | Metafields section | Product metafields | Verified product metafield fields in Shopify Admin |
| SEO and URL handles | `/images/shopify/products/search-engine-listing.png` | Product editor → Search engine listing | Preview, SEO title, description, and URL handle | Unrelated product information | Search engine listing card | Search engine listing | Shopify search engine listing with title, description, and URL handle |
| Organization and sales channels | `/images/shopify/products/status.png` | Product editor → Status | Status selector and current approved options | Account identity and unrelated panels | Status card | Product status | Product status control in Shopify Admin |
| Organization and sales channels | `/images/shopify/products/sales-channel-publishing.png` | Product editor → Publishing | Sales-channel availability names and controls after owner verification | Account identity and unpublished business details | Publishing panel | Sales-channel availability | Product publishing controls for verified Shopify sales channels |
| Bulk edit and import | `/images/shopify/products/bulk-editor.png` | Products → select approved sample products → Bulk edit | Bulk editor fields and safe sample rows | Cost, supplier, customer, and sensitive SKU data | Bulk editor workspace | Shopify bulk editor | Shopify bulk editor showing approved product fields |
| Bulk edit and import | `/images/shopify/products/csv-import.png` | Products → Import | Import control and validation options, without local filenames | Local file paths, personal data, existing sensitive exports | Import dialog | Product CSV import | Shopify product CSV import dialog |
| Product troubleshooting | `/images/shopify/products/storefront-verification.png` | Live SixthGear storefront → approved example product | Product title, gallery, price, options, and availability | Browser account details, analytics overlays, unrelated tabs | Product content area | Product storefront verification | Live SixthGear product page used to verify Shopify product data |

## Storefront product field source mapping

All paths below are relative to `sixthgear-shopify`.

| Shopify field | GraphQL query or fragment | Type mapping | React consumer | Website location | Fallback |
| --- | --- | --- | --- | --- | --- |
| Product title | `src/lib/shopify/queries/product.ts`; `src/lib/shopify/fragments.ts` | `src/lib/shopify/types.ts` → `ShopifyProduct.title` | Product detail template, product cards, search, metadata | Product pages, listings, predictive search | Metadata and image alt logic use the title; no content substitute for a missing title |
| Handle | Product query and `ProductCardFragment` | `ShopifyProduct.handle` | Product links and `[handle]` route | Product URLs, cards, search, recommendations | None; routing depends on the handle |
| Description | Product query and card fragment | `ShopifyProduct.description` | Product route and product tabs | Product details and metadata fallback | Product tabs can show “No description available” |
| Description HTML | `src/lib/shopify/queries/product.ts` | `ShopifyProduct.descriptionHtml` | `src/app/[countryCode]/(main)/products/[handle]/page.tsx` and product tabs | Product detail description | Falls back to plain description |
| Featured image | Product query and card fragment | `ShopifyImage` | Product route, cards, metadata | Listings, product page, social metadata | Gallery images, then placeholder depending on consumer |
| Gallery images | Product query requests first 20; card fragment requests first 2 | `ShopifyImage[]` | Product route and `src/modules/products/components/image-gallery/index.tsx` | Product detail gallery and card hover image | Featured image, then placeholder |
| Price | Product and card queries request product ranges and variant price | `ShopifyMoney` | Product detail pricing, cards, previews, actions | Product pages, listings, cart selection UI | No price rule beyond queried data |
| Compare-at price | Product and card queries | Nullable `ShopifyMoney` | Product price and preview components | Product pages and listings | Hidden when absent or not a valid higher comparison price |
| Currency | Money fields return `currencyCode` | `ShopifyMoney.currencyCode` | Price-formatting components | Anywhere a price appears | Uses the value returned with the money amount |
| Variants | Product query first 100; card fragment first 50 | `ShopifyProductVariant[]` | Product route and product actions | Product detail option and purchase controls | No purchasable selection when a valid variant is unavailable |
| Selected options | Variant selection in product query and fragment | `selectedOptions` name/value array | Product actions | Product detail option selectors | First available or route-selected behavior depends on current selection logic |
| Availability | `availableForSale` on product and variants | Boolean fields in Shopify types | Product route, actions, cards | Purchase CTA, stock messaging, listings | Missing quantity becomes proxy 10 when available or 0 when unavailable |
| Vendor | Product query and card fragment | `ShopifyProduct.vendor` | Product route, cards, information tabs, recommendations | Product page, cards, related products | Some list cards display `Sixthgear`; other consumers may leave it blank |
| Product type | Product query and card fragment | `ShopifyProduct.productType` | Product route, information UI, recommendations | Product page and related-product selection | Empty value receives no semantic substitute |
| Tags | Product query and card fragment | `ShopifyProduct.tags` | Product route, information UI, recommendations | Product details and recommendation ranking | Empty list |
| Collections | `src/lib/shopify/queries/collection.ts` | Collection and product connection types | Store and collection routes/components | Collection listing pages | A product absent from a collection query is absent from that collection view |
| SEO | Product detail query requests `seo.title` and `seo.description` | `ShopifyProduct.seo` | Product route `generateMetadata` | Browser metadata and social metadata | Product title and description |
| Metafields | Explicit identifiers in `src/lib/shopify/queries/product.ts` | `ShopifyMetafield[]` | Product route, tabs, size-chart resolvers, reviews | Product information tabs, size guide, video, ratings | Each resolver omits missing content or uses its own content fallback |
| Recommendations | `productRecommendations(intent: RELATED)` in product query; custom queries in `src/lib/shopify/recommendations.ts` | Product card types | `src/modules/products/components/you-may-like/` | Product-page recommendations | Vendor, product-type, tag, then recent-product fallback logic |
| SKU | Product detail query | Optional variant `sku` | No visible React consumer found | None confirmed | Not applicable |
| Inventory quantity | Not requested by active query | Optional `quantityAvailable` type field | Product route maps a proxy for product actions | Stock messaging and purchase state | 10 for available or 0 for unavailable when missing; not actual inventory |
| Barcode | Not requested | No active mapped field found | None | None | Not applicable |
| Native product video media | Not requested | No native media union mapping found | None | None | `custom.product_video_url` is the separate implemented video path |

### Primary source paths inspected

- `src/lib/env.ts`
- `src/lib/shopify/client.ts`
- `src/lib/shopify/fragments.ts`
- `src/lib/shopify/queries/product.ts`
- `src/lib/shopify/queries/products.ts`
- `src/lib/shopify/queries/collection.ts`
- `src/lib/shopify/queries/search.ts`
- `src/lib/shopify/recommendations.ts`
- `src/lib/shopify/types.ts`
- `src/lib/shopify/metafield-resolvers.ts`
- `src/lib/size-chart.ts`
- `src/lib/data/products.ts`
- `src/lib/data/reviews.ts`
- `src/app/[countryCode]/(main)/products/[handle]/page.tsx`
- `src/modules/home/components/product-sections/product-card/index.tsx`
- `src/modules/products/components/image-gallery/index.tsx`
- `src/modules/products/components/product-actions/index.tsx`
- `src/modules/products/components/product-tabs/index.tsx`
- `src/modules/products/components/product-reviews/index.tsx`

## Owner decisions required

1. Which headless sales channel is the production source?
2. Is Sixthgear Moto Headless 02 still required?
3. Should every product publish to Online Store?
4. Should every product publish to Inbox?
5. Which products may publish to Facebook & Instagram?
6. What is the approved SKU convention?
7. What is the approved tag naming convention?
8. What product image dimensions should staff follow?
9. Which metafields are mandatory?
10. Is the AKRAPOVIC product approved as the documentation example?
11. Should dynamic counts appear in public documentation?

Additional decisions exposed by the audit:

12. Which Shopify location names should the inventory guide use?
13. Should staff manage `custom.*` fields that overlap Shopify-standard taxonomy fields?
14. Is `custom.details` still active or part of a legacy query path?
15. Who owns each `reviews.*` metafield, and should staff avoid all manual edits?
16. Is Shopify Search & Discovery installed and responsible for any current recommendation configuration?
17. Is Goodchart installed, and does it own any current product metafield?
18. Should native Shopify product video media be supported, or should video remain URL-metafield-only?
19. What inventory source should power accurate low-stock messaging on the headless storefront?

## Approval gate for detailed guides

Do not write the detailed Add a product, metafields, publishing, or inventory walkthroughs until the owner decisions and dashboard verification items above are resolved. The next documentation phase should use an approved example product and verified screenshots.
