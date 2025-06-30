# Port Harcourt Market Sales Fulfillment Analysis

## 📌 Problem Statement
Rapid sales growth in the Port Harcourt market has exposed critical bottlenecks in product availability and order fulfillment. These issues are threatening customer retention, particularly among newly acquired customers.

## 👥 Stakeholders
- Country Manager
- Head of E-commerce Operations (Nigeria)

## 🌟 Stakeholder Objectives & Analytical Approach

### 1. **Pinpoint Key Product Availability Gaps**
**Objective:** Identify which of the 200 products contribute significantly to cancelled orders, particularly for shipments to Port Harcourt and surrounding areas.

**Approach:**
- Filtered orders with `DeliveryRegion == 'Port Harcourt'` and examined `CancelReason` and `ProductID`.
- Calculated frequency of cancellations per product.
- Ranked products by their cancellation impact in the region.

**Note:** No inventory log data was available, so stockout frequency could not be calculated directly. Cancellations were used as a proxy for product availability issues.

**Findings:**
- Identified top 10 products most frequently associated with cancellations in the Port Harcourt region.
- These products contribute disproportionately to customer dissatisfaction and fulfillment failures.

**Approach:**
- Filtered orders with `DeliveryRegion == 'Port Harcourt'` and examined `StockStatus`, `CancelReason`, and `ProductID`.
- Calculated frequency of stockouts and cancellations per product.
- Ranked products by their cancellation impact in the region.

**Findings:**
- Identified top 10 products most frequently associated with stockouts and cancellations in the Port Harcourt region.
- These products contribute disproportionately to customer dissatisfaction and fulfillment failures.

### 2. **Assess Impact on Recent Customer Cohorts**
**Objective:** Determine if fulfillment issues disproportionately affect customers acquired after March 1, 2024, and if this correlates with lower repeat purchase rates.

This objective was broken down into two parts:

#### Part A: Fulfillment Issues Among New Customers

**Goal:** Check if customers acquired after March 1, 2024 experience higher rates of order fulfillment failure (i.e., cancellations or returns) compared to earlier customers.

**Null Hypothesis (H0):** The proportion of unfulfilled orders (Cancelled or Returned) among new customers is less than or equal to that of existing customers.

**Alternative Hypothesis (H1):** The proportion of unfulfilled orders among new customers is significantly higher than that of existing customers.

**Methodology:**
- Defined unfulfilled orders based on `OrderStatus` in ['Cancelled', 'Returned'].
- Created the binary flag `IsUnfulfilled`.
- Grouped customers by `CustomerType` and calculated the total orders and number of unfulfilled orders.
- Applied a one-sided z-test using `proportions_ztest()` with the alternative hypothesis that the new cohort has a higher unfulfilled rate.

**Test Result:**
- **Z-statistic:** 0.2168
- **P-value:** 0.4142

**Interpretation:**
- Since the p-value (0.4142) is greater than 0.05, we fail to reject the null hypothesis. There is no significant evidence that new customers experience more unfulfilled orders than existing ones.

#### Part B: Repeat Purchase Behavior in New Customers

**Goal:** Investigate whether fulfillment issues are leading to lower repeat purchase rates among the new customer cohort.

**Null Hypothesis (H0):** The repeat purchase rate for customers acquired after March 1, 2024 is greater than or equal to that of earlier customers.

**Alternative Hypothesis (H1):** The repeat purchase rate for customers acquired after March 1, 2024 is significantly lower.

**Methodology:**
- Defined repeat purchase as customers with more than one distinct `OrderID`.
- Counted repeat and total customers by cohort.
- Performed a one-sided z-test using `proportions_ztest()` with the alternative hypothesis that the new cohort has a lower repeat purchase rate.

**Test Result:**
- **Z-statistic:** -1.0045
- **P-value:** 0.1576

**Interpretation:**
- The negative z-statistic indicates that the repeat purchase rate among new customers is *lower* than that of existing customers.
- However, the p-value (0.1576) is greater than 0.05, so we fail to reject the null hypothesis.
- This means that while there is a downward trend, the observed difference is not statistically significant at the 5% level. More data may be needed to confirm a genuine decline in loyalty or purchasing behavior among new customers.

### 3. **Identify Top Supplier-Related Fulfillment Constraints**
**Objective:** Determine which suppliers (from the set of 15) are associated with the most problematic products in terms of availability or returns.

**Approach:**
- Merged order data with supplier data.
- Analyzed `ReturnStatus`, `StockStatus`, and `CancelReason` by supplier.
- Ranked suppliers by the frequency of product issues linked to their SKUs.

**Findings:**
- Identified 3 suppliers with a high incidence of stockouts and returns.
- These suppliers may require immediate operational review or renegotiation of terms.

---

## 🔧 Next Steps (To Be Finalized)
- Include visualizations and final test statistics from the notebook.
- Add recommendations based on findings.
- Validate hypotheses with broader dataset if needed.

---

*This README will be updated iteratively as we refine the analysis.*
