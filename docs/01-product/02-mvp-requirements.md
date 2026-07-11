# ProScientist MVP Requirements

## 1. Functional Requirements

### FR-01 — Display a Predefined Costco Product Catalog

The application must provide a selectable list of supported Costco protein products.

Each catalog entry includes:

- Product name
- Product category
- Reference price per kilogram
- Raw protein per 100 g
- Trimming loss rate
- Cooking loss rate
- Protein retention rate
- Price update date
- Optional data-source note

### FR-02 — Select a Product

The user can select one product from the catalog before entering purchase information.

The application automatically loads the product's preset nutrition, price, and yield assumptions.

### FR-03 — Enter Weight or Package Price

The user must enter at least one of the following:

- Package weight in kilograms
- Total package price

The application estimates the missing value using the product's reference price per kilogram.

### FR-04 — Override the Reference Price

The user can replace the preset price per kilogram with the actual value shown on the package or receipt.

User-entered values take priority over catalog defaults.

### FR-05 — Add Multiple Products

The user can add multiple supported Costco products to the same grocery plan.

The application calculates both item-level results and the combined plan summary.

### FR-06 — Select a Planning Period

The user selects:

- Purchase date
- Next planned shopping date

The purchase date defaults to the current date.

### FR-07 — Calculate Usable and Cooked Weight

The application calculates:

- Purchased weight
- Estimated usable raw weight after trimming
- Estimated cooked weight after cooking loss
- Cooked meat available per day

### FR-08 — Calculate Protein Availability

The application calculates:

- Estimated total retained protein
- Estimated protein available per day
- Optional surplus or deficit against a user-entered daily protein target

### FR-09 — Calculate Cost Metrics

The application calculates:

- Estimated total purchase cost
- Average food cost per day
- Cost per kilogram of purchased food
- Cost per gram of retained protein

### FR-10 — Edit or Remove Products

The user can edit purchase values or remove an item without restarting the plan.

### FR-11 — Save the Current Plan

The application stores the current plan locally so that refreshing the page does not immediately remove the data.

### FR-12 — Show Calculation Assumptions

The result view must make it clear that:

- Loss rates are preset estimates
- Actual cooking methods may produce different results
- Actual Costco prices may vary
- Results represent planned food availability, not confirmed food consumption

---

## 2. Product Catalog Requirements

### 2.1 Product Catalog Entry

Each supported product should contain the following fields:

| Field | Type | Description |
|---|---|---|
| `id` | String | Unique product identifier |
| `store` | String | `Costco` for the initial MVP |
| `name` | String | Product display name |
| `category` | Enum | Chicken, pork, beef, turkey, fish, or other |
| `defaultPricePerKg` | Decimal | Reference Costco price per kilogram |
| `proteinPer100GRaw` | Decimal | Estimated protein in 100 g of raw product |
| `trimmingLossRate` | Decimal | Estimated proportion removed during trimming |
| `cookingLossRate` | Decimal | Estimated proportion of usable raw weight lost during cooking |
| `proteinRetentionRate` | Decimal | Estimated proportion of protein retained after preparation |
| `priceUpdatedAt` | Date | Date when the reference price was last checked |
| `sourceNote` | String, optional | Short note describing the source or assumption |

### 2.2 Catalog Rules

- Product data is maintained by the project, not entered by the user.
- Decimal rates are stored between `0` and `1`.
- Prices are stored in Canadian dollars.
- Catalog values should retain full precision internally.
- Displayed values may be rounded for readability.
- A product should not be published in the catalog without all values required by the calculation engine.
- Different cuts of meat should be separate catalog products when their yield assumptions differ meaningfully.

### 2.3 Price Override Priority

The application uses pricing data in the following order:

1. User-entered total price and user-entered weight
2. User-entered price per kilogram
3. Product catalog reference price per kilogram

When both total price and weight are entered, no price estimation is required.

---

## 3. User Input Requirements

### 3.1 Plan-Level Inputs

| Field | Required | Rules |
|---|---:|---|
| Purchase date | Yes | Defaults to the current date |
| Next shopping date | Yes | Must be later than the purchase date |
| Daily protein target | No | Must be greater than zero when provided |
| Currency | Yes | Fixed to CAD in the MVP |

### 3.2 Item-Level Inputs

| Field | Required | Rules |
|---|---:|---|
| Costco product | Yes | Selected from the predefined catalog |
| Package weight | Conditional | Required when package price is not entered |
| Total package price | Conditional | Required when package weight is not entered |
| Actual price per kilogram | No | Overrides the preset reference price |
| Note | No | Optional short memo |

### 3.3 Input Assumptions

- The entered package weight is the purchased raw weight.
- The selected catalog product reasonably matches the purchased product.
- The entire planned portion is expected to be consumed within the selected period.
- The MVP does not calculate household sharing or individual serving allocation.
- The user may enter either weight or price because Costco packages are commonly sold using weight-based pricing.

---

## 4. Validation Requirements

The application must reject or clearly flag:

- Empty product selections
- Zero or negative weight
- Zero or negative price
- Zero or negative price per kilogram
- A next shopping date that is the same as or earlier than the purchase date
- Numeric input that cannot be parsed
- A plan with no grocery items

---

## 5. Result Requirements

### 5.1 Primary Results

The application must prominently display:

- Estimated protein available per day
- Estimated cooked meat available per day
- Estimated food cost per day

### 5.2 Secondary Results

The application should support displaying:

- Planning period
- Estimated total purchase cost
- Purchased raw weight
- Usable raw weight
- Cooked weight
- Total retained protein
- Cost per gram of protein
- Cost per kilogram
- Daily protein surplus or deficit
- Price source and update date

### 5.3 Item-Level Results

Each selected product should display:

- Product name
- Purchased or estimated weight
- Actual or estimated price
- Applied price per kilogram
- Trimming loss assumption
- Cooking loss assumption
- Estimated cooked weight
- Estimated retained protein
- Daily protein contribution
- Daily cost contribution

### 5.4 Estimate Labels

Estimated values must be clearly labeled.

Examples:

- `Estimated`
- `Based on preset yield`
- `Using reference Costco price`
- `Price last updated: YYYY-MM-DD`

---

## 6. Persistence Requirements

- The application must save the current plan locally.
- Refreshing the page must not immediately remove the plan.
- LocalStorage is sufficient for the MVP.
- User authentication and cloud synchronization are not required.

---

## 7. Testing Requirements

Unit tests should cover:

- Weight estimation from total price
- Price estimation from weight
- User price override behavior
- Trimming-loss calculation
- Cooking-loss calculation
- Protein-retention calculation
- Daily result calculation
- Multiple-product aggregation
- Date validation
- Zero and negative input rejection
- Decimal and rounding behavior
