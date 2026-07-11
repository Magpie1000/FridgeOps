# ProScientist MVP Specification

## 1. Core Objective

The first version of ProScientist focuses on one narrow use case:

> Help Costco shoppers estimate usable cooked meat, daily protein availability, and daily food cost from bulk meat purchases.

The user does not manually enter nutrition data, trimming loss, cooking loss, or protein retention values.

Instead, ProScientist stores these values in a predefined Costco product catalog.

The user only needs to:

1. Select a Costco product
2. Enter the package weight or total package price
3. Select the next planned grocery-shopping date
4. Review the calculated protein and cost plan

---

## 2. Initial Product Coverage

The MVP supports a small catalog of commonly purchased Costco protein products, such as:

- Boneless skinless chicken breast
- Boneless chicken thighs
- Pork tenderloin
- Pork loin
- Lean ground beef
- Extra-lean ground beef
- Ground turkey
- Salmon

The final MVP catalog should remain intentionally small, preferably between five and ten products.

---

## 3. Geographic and Pricing Scope

The initial catalog is based on Costco Canada products.

Because prices can vary by warehouse and date:

- Each product has a preset reference price per kilogram
- The price update date is shown to the user
- The user can override the preset price when necessary
- The application clearly identifies calculated prices as estimates

The MVP does not require live Costco pricing or automatic price collection.

---

## 4. Result Specification

### 4.1 Primary Results

The summary view should emphasize:

1. **Estimated protein available per day**
2. **Estimated cooked meat available per day**
3. **Estimated food cost per day**

### 4.2 Secondary Results

The application may also display:

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

### 4.3 Item-Level Results

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

### 4.4 Estimate Labels

Estimated values should be visually identified using labels such as:

- `Estimated`
- `Based on preset yield`
- `Using reference Costco price`
- `Price last updated: YYYY-MM-DD`

The interface must not present estimated values as guaranteed results.

---

## 5. Basic User Flow

### Step 1 — Set the Next Costco Shopping Date

The user selects the purchase date and the next planned Costco trip.

### Step 2 — Select a Costco Product

The user selects a supported product from the predefined catalog.

### Step 3 — Enter Weight or Price

The user enters either:

- Package weight, or
- Total package price

The application estimates the missing value.

### Step 4 — Review Preset Assumptions

The application displays:

- Reference price per kilogram
- Price update date
- Estimated trimming loss
- Estimated cooking loss

The user may override the price when necessary.

### Step 5 — Add the Product

The product is added to the current grocery plan.

### Step 6 — Add More Products

The user can repeat the process for other Costco protein products.

### Step 7 — Review the Plan

The application shows the combined:

- Cooked meat per day
- Protein per day
- Food cost per day

### Step 8 — Adjust the Purchase Plan

The user changes quantities, prices, or products until the plan fits the desired budget and protein target.

### Step 9 — Save Locally

The current plan remains available after a page refresh.

---

## 6. UX Principles

### 6.1 Selection Before Data Entry

The user should select a known product rather than manually entering nutrition and loss assumptions.

### 6.2 One Required Purchase Value

The user should be able to continue after entering either weight or total price.

Requiring both values would add unnecessary friction.

### 6.3 Presets Must Remain Visible

Automatic calculations should not hide their assumptions.

Users should be able to see:

- Reference price
- Price update date
- Trimming loss
- Cooking loss

### 6.4 Actual Values Override Estimates

When the user knows the actual weight or price, the application should use it instead of the preset estimate.

### 6.5 Important Results First

The result screen should prioritize:

- Protein per day
- Cooked meat per day
- Cost per day

Technical details should remain available but secondary.

### 6.6 Mobile-Friendly Grocery Entry

The interface should support quick entry while shopping or immediately after returning home.

Recommended controls include:

- Searchable product selector
- Numeric keyboard
- Date picker
- Large add and calculate buttons
- Clearly displayed units

### 6.7 No False Precision

The application should use practical rounding and explain that cooking outcomes vary.

### 6.8 Easy Comparison Through Recalculation

Users should be able to change a product or quantity and immediately see how the result changes.

---

## 7. Out of Scope for the Costco MVP

The following features are excluded from the first release:

- Products from stores other than Costco
- User-created custom products
- Live Costco price collection
- Warehouse-specific inventory availability
- Automatic warehouse detection
- Barcode scanning
- Receipt OCR
- Nutrition-label scanning
- Meal-by-meal logging
- Calorie, carbohydrate, and fat tracking
- Vegetables, sauces, spices, and leftovers
- Recipes and cooking instructions
- Automatic inventory reduction
- Expiration-date tracking
- Household-member allocation
- User accounts
- Cloud synchronization
- Notifications
- AI recommendations
- Medical nutrition advice
- Blood-glucose prediction
- Store-to-store price comparison

The MVP should remain a focused Costco bulk-protein calculator.

---

## 8. Success Criteria

### 8.1 Usability

The MVP is successful when:

- A first-time user can understand the workflow without instructions
- A user can produce a result within one minute
- A product can be added in less than 20 seconds
- Entering only weight or only price is sufficient
- The three primary results are understandable at a glance

### 8.2 Calculation Quality

The MVP is successful when:

- All formulas pass independently verified unit tests
- Price-based and weight-based estimates produce consistent results
- Trimming loss and cooking loss are applied in the correct order
- Protein retention is calculated separately from cooking weight loss
- Multiple products are combined correctly
- Invalid values are rejected

### 8.3 Product Validation

The MVP is successful when target users confirm that:

- Selecting a known product is easier than entering full nutrition data
- The result helps them plan until the next Costco trip
- The daily protein estimate is useful for meal-prep decisions
- The daily cost estimate is useful for grocery budgeting
- They would use the tool again for a future Costco purchase

### 8.4 Scope Validation

The MVP should be considered correctly scoped when it can be completed without:

- Live retailer APIs
- Automatic price scraping
- User authentication
- AI features
- A large food database
- Multi-store support

---

## 9. Initial Technical Scope

### 9.1 Frontend-First MVP

The initial implementation should use:

- React
- TypeScript
- Static Costco product data
- Client-side calculation functions
- LocalStorage
- Responsive design
- Automated unit tests

A backend and database are not required to validate the first user flow.

### 9.2 Suggested Frontend Structure

```text
apps/frontend/src/
├─ data/
│  └─ costco-products.json
├─ features/
│  └─ protein-planner/
├─ calculations/
│  └─ protein-plan.ts
├─ storage/
│  └─ local-plan-storage.ts
├─ types/
│  └─ protein-plan.ts
└─ components/
```

---

## 10. Data Model

### 10.1 Product Data Model

```ts
interface StoreProduct {
  id: string;
  store: "costco";
  name: string;
  category:
    | "chicken"
    | "pork"
    | "beef"
    | "turkey"
    | "fish"
    | "other";

  defaultPricePerKg: number;
  proteinPer100GRaw: number;

  trimmingLossRate: number;
  cookingLossRate: number;
  proteinRetentionRate: number;

  priceUpdatedAt: string;
  sourceNote?: string;
}
```

### 10.2 Purchase Item Model

```ts
interface PurchaseItem {
  id: string;
  productId: string;

  weightKg?: number;
  totalPrice?: number;
  overridePricePerKg?: number;

  note?: string;
}
```

### 10.3 Grocery Plan Model

```ts
interface GroceryPlan {
  id: string;
  purchaseDate: string;
  nextShoppingDate: string;
  dailyProteinTargetG?: number;
  currency: "CAD";
  items: PurchaseItem[];
}
```

### 10.4 Item Estimate Model

```ts
interface ItemEstimate {
  productId: string;

  effectivePricePerKg: number;
  purchasedWeightKg: number;
  totalPrice: number;

  usableRawWeightKg: number;
  cookedWeightKg: number;

  retainedProteinG: number;
  dailyCookedWeightG: number;
  dailyProteinG: number;
  dailyCost: number;
  costPerProteinG: number;

  usedEstimatedWeight: boolean;
  usedEstimatedPrice: boolean;
}
```

### 10.5 Plan Summary Model

```ts
interface PlanSummary {
  planningDays: number;

  totalPrice: number;
  totalPurchasedWeightKg: number;
  totalCookedWeightKg: number;
  totalRetainedProteinG: number;

  dailyCookedWeightG: number;
  dailyProteinG: number;
  dailyCost: number;
  costPerProteinG: number;

  dailyProteinBalanceG?: number;
}
```
