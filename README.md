# Port Harcourt Market Sales Fulfillment Analysis
---

## 📝 Project Description

This project was conducted as part of a collaborative group data challenge aimed at solving a real-world e-commerce problem.

Our team was tasked with uncovering operational issues behind poor customer retention in the Port Harcourt market. We approached the problem from multiple angles—product availability, order fulfillment delays, and supplier-related constraints—through rigorous data analysis.

The project was designed to simulate a stakeholder-driven analytics environment. Each team member contributed by owning different parts of the workflow including data wrangling, exploratory analysis, hypothesis testing, and business reporting.

We faced challenges typical of real-world data projects: incomplete inventory tracking, ambiguous cancellation reasons, and the need to simulate decisions using proxy variables. This made the work even more reflective of actual business scenarios, requiring creative problem-solving and clear communication among team members.

What started as a technical exercise turned into a full-blown business investigation. The insights generated here are designed to directly inform strategic decisions by regional leadership teams overseeing logistics, operations, and customer experience.

---

## 📌 Problem Statement
Rapid sales growth in the Port Harcourt market has exposed critical bottlenecks in product availability and order fulfillment. These issues are threatening customer retention, particularly among newly acquired customers.

## 👥 Stakeholders
- Country Manager
- Head of E-commerce Operations (Nigeria)

## 🌟 Objectives for the Stakeholder:

1. Pinpoint Key Product Availability Gaps: 
Identify which of the 200 products are most frequently out of stock or are major contributors to Cancelled orders, 
especially for shipments to Port Harcourt and surrounding areas, despite the high order volume
2. Assess Impact on Recent Customer Cohorts: 
Determine if fulfillment issues (e.g., significant delays where ActualDeliveryDate far exceeds ExpectedDeliveryDate, or high cancellation rates) are disproportionately affecting customers acquired since March 2024 (RegistrationDate > 2024-03-01), and if this correlates
with lower initial repeat purchase rates from these new customers
3. Identify Top Supplier-Related Fulfillment Constraints: 
For the limited set of 15 suppliers, determine which ones are linked to the products experiencing 
the most severe availability gaps or quality issues (inferred from ReturnStatus) that impede smooth order fulfillment to the Port Harcourt market.
---
##  Analytical Approach

## 1. **Pinpoint Key Product Availability Gaps**
**Objective:** Identify which of the 200 products contribute significantly to cancelled orders, particularly for shipments to Port Harcourt and surrounding areas.

**Approach:**
- Filtered orders with `DeliveryRegion == 'Port Harcourt,Warri,Aba,Calabar,Owerri,Uyo,' and examined `CancelReason` and `ProductID`.
- Calculated frequency of cancellations per product.
- Ranked products by their cancellation impact in the region.

**Note:** No inventory log data was available, so stockout frequency could not be calculated directly. Cancellations were used as a proxy for product availability issues.

**Findings:**
- Identified top 20 products most frequently associated with cancellations in the Port Harcourt region.
- These products contribute disproportionately to customer dissatisfaction and fulfillment failures.

  ![image](https://github.com/user-attachments/assets/8b6a6fdc-e69f-4d83-b646-6ba934f79c05)

  [⬇️ Download the dataset](https://raw.githubusercontent.com/Jcboy101/Port-Harcourt-E-commerce-Fulfillment-Audit-Group-Project-/main/TopCancelledProducts.csv)




---
## 2. **Assess Impact on Recent Customer Cohorts**
**Objective:** Determine if fulfillment issues disproportionately affect customers acquired after March 1, 2024, and if this correlates with lower repeat purchase rates.

This objective was broken down into two parts:

---
### Part A: Fulfillment Issues Among New Customers

**Goal:** Check if customers acquired after March 1, 2024 experience higher rates of order fulfillment failure (i.e., cancellations or returns) compared to earlier customers.

**Null Hypothesis (H0):** The proportion of unfulfilled orders (Cancelled or Returned) among new customers is less than or equal to that of existing customers.

**Alternative Hypothesis (H1):** The proportion of unfulfilled orders among new customers is significantly higher than that of existing customers.

**Methodology:**
- Defined unfulfilled orders based on `OrderStatus` in ['Cancelled', 'Returned'].
- Created the binary flag `IsUnfulfilled`.
- Grouped customers by `CustomerType` and calculated the total orders and number of unfulfilled orders.

![image](https://github.com/user-attachments/assets/9c9a4eda-89f3-4e2b-964c-b400bd86f8bd)

![image](https://github.com/user-attachments/assets/d8213909-f062-4562-8198-9e2404a37c2f)


- For delivery delay analysis, calculated the difference between ActualDeliveryDate and ExpectedDeliveryDate. Used the overall mean + 1 standard deviation as a threshold to flag delays as excessive. This helped ensure that only significantly delayed deliveries were considered in further impact analysis.
- Applied a one-sided z-test using `proportions_ztest()` with the alternative hypothesis that the new cohort has a higher unfulfilled rate.

**Test Result:**
- **Z-statistic:** 0.2168
- **P-value:** 0.4142

![image](https://github.com/user-attachments/assets/e64d4689-4832-433e-88a8-0a0df2e83d0f)


**Interpretation:**
- Since the p-value (0.4142) is greater than 0.05, we fail to reject the null hypothesis. There is no significant evidence that new customers experience more unfulfilled orders than existing ones.

---
## Part B: Repeat Purchase Behavior in New Customers

**Goal:** Investigate whether fulfillment issues are leading to lower repeat purchase rates among the new customer cohort.

**Null Hypothesis (H0):** The repeat purchase rate for customers acquired after March 1, 2024 is greater than or equal to that of earlier customers.

**Alternative Hypothesis (H1):** The repeat purchase rate for customers acquired after March 1, 2024 is significantly lower.

**Methodology:**
- Defined repeat purchase as customers with more than one distinct `OrderID`.
- Counted repeat and total customers by cohort.

![image](https://github.com/user-attachments/assets/5b56da48-ec23-4017-a474-21ed41120c92)

- Performed a one-sided z-test using `proportions_ztest()` with the alternative hypothesis that the new cohort has a lower repeat purchase rate.

**Test Result:**
- **Z-statistic:** -1.0045
- **P-value:** 0.1576

  ![image](https://github.com/user-attachments/assets/e8360ab1-8670-4654-8d04-f1696062eb82)


**Interpretation:**
- The negative z-statistic indicates that the repeat purchase rate among new customers is *lower* than that of existing customers.
- However, the p-value (0.1576) is greater than 0.05, so we fail to reject the null hypothesis.
- This means that while there is a downward trend, the observed difference is not statistically significant at the 5% level. More data may be needed to confirm a genuine decline in loyalty or purchasing behavior among new customers.

---

## 3. **Identify Top Supplier-Related Fulfillment Constraints**
**Objective:** Determine which suppliers (from the set of 15) are associated with the most problematic products in terms of availability or returns.

**Approach:**
- Merged order data with supplier data.
- Analyzed `ReturnStatus`, `StockStatus`, and `CancelReason` by supplier.
- Ranked suppliers by the frequency of top 20 product issues linked to their SKUs.

**Findings:**
- Identified suppliers with a high incidence of stockouts and returns.
- These suppliers may require immediate operational review or renegotiation of terms.

  ![Screenshot 2025-06-30 180205](https://github.com/user-attachments/assets/da90aaad-c725-4135-9c48-570b29878d83)


---


