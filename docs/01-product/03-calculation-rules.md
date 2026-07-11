# ProScientist Calculation Rules

## 1. Calculation Principles

- User-entered values take priority over catalog estimates.
- Weight loss during cooking and protein loss are not treated as the same value.
- Calculations retain full decimal precision internally.
- Displayed values should be rounded consistently for readability.
- Results are estimates and may vary depending on product condition, trimming, and cooking method.

---

## 2. Input Priority

The application uses pricing data in the following order:

1. User-entered total price and user-entered weight
2. User-entered price per kilogram
3. Product catalog reference price per kilogram

When both total price and weight are entered, no price estimation is required.

---

## 3. Planning Period

```text
planningDays = nextShoppingDate - purchaseDate
```

The next shopping date marks the beginning of the next grocery cycle and is not included in the current period.

Example:

```text
Purchase date: July 1
Next shopping date: July 15
Planning period: 14 days
```

---

## 4. Effective Price per Kilogram

```text
effectivePricePerKg =
    userPricePerKg ?? catalogDefaultPricePerKg
```

---

## 5. Estimate Weight from Total Price

Used when the user enters total package price but not weight:

```text
estimatedWeightKg =
    totalPackagePrice / effectivePricePerKg
```

---

## 6. Estimate Total Price from Weight

Used when the user enters weight but not total package price:

```text
estimatedTotalPrice =
    packageWeightKg × effectivePricePerKg
```

---

## 7. Usable Raw Weight

```text
usableRawWeightKg =
    purchasedWeightKg × (1 - trimmingLossRate)
```

---

## 8. Cooked Weight

```text
cookedWeightKg =
    usableRawWeightKg × (1 - cookingLossRate)
```

---

## 9. Raw Protein Before Retention Adjustment

```text
rawProteinG =
    usableRawWeightKg × 10 × proteinPer100GRaw
```

---

## 10. Retained Protein

```text
retainedProteinG =
    rawProteinG × proteinRetentionRate
```

Cooking weight loss and protein loss must not be treated as the same value.

Most cooking weight loss comes from water and fat, so the application uses a separate protein-retention assumption.

---

## 11. Daily Cooked Meat

```text
dailyCookedWeightG =
    cookedWeightKg × 1,000 / planningDays
```

---

## 12. Daily Protein

```text
dailyProteinG =
    retainedProteinG / planningDays
```

---

## 13. Daily Cost

```text
dailyCost =
    totalPrice / planningDays
```

---

## 14. Cost per Gram of Protein

```text
costPerProteinG =
    totalPrice / retainedProteinG
```

---

## 15. Daily Protein Balance

When a target is entered:

```text
dailyProteinBalanceG =
    dailyProteinG - dailyProteinTargetG
```

A positive value represents an estimated surplus.

A negative value represents an estimated deficit.

---

## 16. Combined Plan Totals

For a plan with multiple products:

```text
planTotalCost = sum(itemTotalPrice)
planCookedWeightKg = sum(itemCookedWeightKg)
planRetainedProteinG = sum(itemRetainedProteinG)
planDailyCost = planTotalCost / planningDays
planDailyProteinG = planRetainedProteinG / planningDays
```

---

## 17. Rounding Rules

- Preserve the original decimal precision during calculations.
- Round only when values are displayed.
- Use consistent rounding across item-level and plan-level results.
- Do not store rounded values as the source of truth.
