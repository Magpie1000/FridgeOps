# ProScientist Future Development

## 1. Additional Grocery Stores

The first major expansion is support for other grocery retailers, including:

- Walmart
- T&T Supermarket
- Sobeys
- Real Canadian Superstore
- Other regional or local grocery stores

Each store can have its own product catalog, package format, and reference pricing.

---

## 2. Store Selection

Users will be able to choose a store before selecting a product.

Example:

```text
Select Store
→ Select Product
→ Enter Weight or Price
→ Compare Plan
```

---

## 3. Cross-Store Product Comparison

The application may compare similar products across stores using:

- Price per kilogram
- Cost per gram of retained protein
- Estimated cooked yield
- Estimated daily cost

---

## 4. Mixed-Store Grocery Plans

A single plan may include products purchased from multiple stores.

Example:

- Chicken from Costco
- Ground beef from Walmart
- Fish from T&T Supermarket
- Pork from Sobeys

---

## 5. User-Entered Store Products

Users may eventually create custom products by entering:

- Store
- Product name
- Price
- Weight
- Protein value
- Yield assumptions

---

## 6. Price History

Future versions may store:

- Price by store
- Price by date
- Price per kilogram
- Promotional price
- Regular price

---

## 7. Shopping Plan Optimization

The application may help users identify:

- The lowest-cost protein combination
- The best store for a selected product
- The amount required for a target period
- The expected budget for a multi-store grocery trip

---

## 8. Faster Input

Possible future input methods include:

- Receipt OCR
- Barcode scanning
- Nutrition-label scanning
- Reusing previous purchases
- Favorite products
- Recent product history

---

## 9. Household Planning

Possible future household features include:

- Multiple household members
- Individual protein targets
- Shared-food ratios
- Household grocery-budget estimates

---

## 10. Backend Expansion

When multi-store support or shared product data becomes necessary, the project may add a minimal backend.

Possible endpoints:

```text
GET  /api/stores
GET  /api/stores/{storeId}/products
POST /api/estimates
```

A future relational model may separate:

- Store
- Product
- StoreProductPrice
- YieldProfile
- GroceryPlan
- PurchaseItem

This backend expansion should happen only after the Costco-only workflow has been validated.
