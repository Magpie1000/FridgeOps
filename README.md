# ProScientist

### Costco-Based Protein Planning for Busy Lifters

> **Meal prep your protein. Skip the food diary.**

ProScientist is a lightweight planning tool that turns bulk grocery purchases into practical estimates of **cooked meat, daily protein, and daily food cost**.

The first MVP is intentionally focused on a small catalog of **Costco Canada protein products**. Users select a product, enter either its package weight or total price, choose their next shopping date, and let the application calculate the rest using preset price, nutrition, trimming-loss, cooking-loss, and protein-retention assumptions.

---

## Documentation

- [MVP Specification](./docs/01-product/01-mvp-spec.md)
- [MVP Requirements](./docs/01-product/02-mvp-requirements.md)
- [Calculation Rules](./docs/01-product/03-calculation-rules.md)
- [Future Development](./docs/01-product/04-future-development.md)

---

## Why This Project Exists

I usually shop for groceries once every two weeks.

I wanted to support healthier eating in my household while also getting enough protein for powerlifting and muscle gain. In practice, it was difficult to answer a few basic questions:

- How much usable food will remain after trimming and cooking?
- How much protein is available per day until the next shopping trip?
- How much does that protein cost per day?
- Am I buying too much, or not enough?

Existing diet apps usually require meal-by-meal logging. Refrigerator inventory apps require constant maintenance. Both approaches create more work than I wanted.

ProScientist takes a simpler approach:

> Select what you bought, enter its weight or price, and estimate the plan automatically.

---

## MVP

### User Input

1. Select a supported Costco product
2. Enter either:
   - Package weight, or
   - Total package price
3. Select the next planned grocery-shopping date
4. Optionally enter a daily protein target

### Preset Product Data

Each supported product contains predefined values for:

- Reference price per kilogram
- Raw protein per 100 g
- Trimming loss rate
- Cooking loss rate
- Protein retention rate
- Price update date

The user can override the reference price when the actual package or receipt price is available.

### Main Results

ProScientist estimates:

- Usable raw weight
- Cooked weight
- Cooked meat available per day
- Retained protein
- Protein available per day
- Food cost per day
- Cost per gram of protein
- Protein surplus or deficit against the optional target

All results are planning estimates. Actual outcomes vary by product, trimming, and cooking method.

---

## Example Workflow

```text
Select Costco Product
        ↓
Enter Weight or Package Price
        ↓
Select Next Costco Trip
        ↓
Review Daily Protein, Cooked Weight, and Cost
```

The MVP supports multiple products in one grocery plan and combines their estimated totals.

---

## Target Users

ProScientist is designed for:

- Recreational lifters and powerlifters
- People who meal prep and buy protein foods in bulk
- People who shop weekly or biweekly
- People who are tired of detailed diet tracking
- People who want a practical grocery-budget estimate
- Households trying to avoid both overbuying and running out of food

---

## Product Positioning

ProScientist is **not**:

- A meal-by-meal calorie tracker
- A complete refrigerator inventory system
- A recipe recommendation app
- A medical nutrition tool
- A precise record of actual food consumption

ProScientist **is**:

> A grocery-level planning tool that converts bulk protein purchases into daily food and cost estimates.

Traditional diet apps track meals.

Traditional refrigerator apps track inventory.

ProScientist tracks the relationship between:

- Product choice
- Purchase weight and price
- Preparation loss
- Protein availability
- Shopping interval
- Grocery cost

---

## Key Product Decisions

### Minimal Input

Users select a known product instead of manually entering nutrition and yield data.

### Weight or Price, Not Both

Entering either package weight or total price is enough. The application estimates the missing value from the current reference price.

### Separate Cooking Loss from Protein Loss

Cooking reduces weight mainly through water and fat loss. ProScientist therefore calculates cooking-weight loss and protein retention separately instead of treating them as the same value.

### Visible Assumptions

Reference prices, update dates, and yield assumptions remain visible so estimated values do not appear more precise than they are.

### Narrow First Release

The first version supports only a small Costco Canada catalog. Live prices, barcode scanning, receipt OCR, user accounts, and multi-store support are intentionally excluded.

---

## Initial Technical Direction

The MVP is designed as a frontend-first application using:

- React
- TypeScript
- Static Costco product data
- Client-side calculation functions
- LocalStorage
- Responsive design
- Automated unit tests

A backend and database are not required to validate the initial workflow.

The main engineering focus is the calculation engine, including:

- Price-to-weight and weight-to-price estimation
- Trimming and cooking yield calculations
- Protein-retention calculations
- Multiple-product aggregation
- Date and numeric validation
- Consistent decimal and rounding behavior

---

## Future Direction

After validating the Costco-only workflow, ProScientist may expand to:

- Walmart
- T&T Supermarket
- Sobeys
- Real Canadian Superstore
- Other regional grocery stores
- Mixed-store grocery plans
- Cross-store protein-cost comparisons
- Price history
- Receipt or barcode input
- Household-level planning

See [Future Development](./docs/01-product/04-future-development.md) for details.

---

## Core Idea

> Existing diet apps assume users will log every meal. ProScientist assumes busy people want useful protein and grocery planning with the least possible input.
