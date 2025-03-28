# BI-Analysis
Business Intelligence Analytics

# Loan Disbursement Performance Analysis Report

### 1. Introduction

This report presents an analysis of loan disbursement and repayment data to evaluate key lending product features, repayment behaviors, credit risk exposure, and financial performance. 
The insights derived will inform risk management strategies and product improvements.

### 2. Analysis Methodology

#### 2.1 Data Sources

Two datasets were analyzed:

Disbursement Dataset: Contains loan issuance details such as customer_id, disbursement_date, loan_amount, loan_fee, and tenure (7, 14, or 30 days).

Repayment Dataset: Captures repayment details including date_time, customer_id, amount, repayment_month, and repayment_type (automatic or manual).

#### 2.2 Key Steps in Analysis

- Data Cleaning & Preparation (imported pyspark dependencies as detailed in the notebook)

- Merged datasets using customer_id.

- Standardized date formats and handled missing values.

##### Identifying Defaulted Loans

- Defined default as repayments occurring after the due date.

- Computed due dates based on disbursement_date + tenure.

- Time-Series Analysis (TSA) KPIs

- Repayments by date and type

- Repayments by month

- Disbursements by date

 #### Profit & Loss Calculation

- Profit = Monthly Repayment - Monthly Disbursement

- Loans repaid beyond due dates were considered losses.
---------------------------------------------------------------------------------
- Repayments are generally higher than disbursements (This I've visualized in the jupyter notebook as well as powerbi report.)
- This suggests that loan collections exceed new loans issued, which is good for liquidity.
--------------------------------------------------------------------------------
- March to May 2024 saw peak repayment and disbursement levels
- This could indicate higher loan activity during this period.
--------------------------------------------------------------------------------
- A noticeable decline in August 2024: This could indicate a decrease in lending activity or a seasonal effect.
- Repayments and disbursements were nearly equal in July 2024: Stable loan cycle in that month
--------------------------------------------------------------------------------
 #### Overall Trend on the profit linegraph: 
- While there's an overall upward trend from April to June and a huge spike in August, the volatility with the July drop raises concerns about the consistency and predictability of profit.
- However, the drop in July and the spike in August is attributed to the methodology used in profit calculation
- The difference in value of monthly repayments to the loans disbursed in july is minimal, resulting in lower profit.
- In August, the difference is significantly larger, despite the low volume of transactions. This creates the appearance of a sharp profit increase


## Potential Insights:
- If this trend continues, the institution may need to increase loan disbursements to sustain its portfolio.
- The sharp drop in August could be due to policy changes, economic conditions, or seasonal lending trends.

 #### Credit Exposure & Risk Analysis

- Expected Credit Loss (ECL) Model:

- PD (Probability of Default): Historical default rate.

- LGD (Loss Given Default): 40% of outstanding loans.

- EAD (Exposure at Default): Total loans yet to be repaid.

- Used SQL to compute ECL for risk provisioning.

- Forecasting 3-Month Profit/Loss

- Applied Exponential Smoothing Average for trend analysis. (The data spanning 8 months wasn't sufficient for accurate predictions)

- Assumed seasonal variation in repayments.

#### Portfolio Triggers & Alerts

- High Default Rate Alert: Triggered if PD > 5%.

- Late Payment Alert: If loans are overdue by >10 days.

-- Liquidity Risk Alert: If monthly profit is negative for 3 consecutive months.

### 3. Assumptions

- Loan repayments occur daily unless a borrower defaults.

- Repayment type affects default risk (manual payments have higher risk).

- Credit risk provisioning is based on a 40% loss assumption.

- Future profit/loss follows historical patterns, allowing for forecasting.

### 4. Key Findings

#### 4.1 Loan Product Features & Disbursement Trends

The loan tenure significantly impacts repayment behavior, with longer tenures (30 days) showing a higher default rate.

Loan fees are fixed ratios based on tenure:

7-day tenure: Fee = 15% rate

14-day tenure: Fee = 12% rate 

30-day tenure: Fee = 10% rate 

4.2 Repayment Analysis

Logic used: 
 - Only defaulted loans older than 90 days will be "Written Off" (33)
 - Loans repaid on time will be "Active Loans" (26332)

Repayments are highest on automatic payments, reducing default risk.

Seasonal repayment trends show spikes around salary payment periods.

#### 4.3 Profit/Loss Analysis

- Changed the code such that loans are only marked as defaulted if not fully repaid
- Late repayments (but not fully repaid loans) are not falsely classified as defaults
- This is because some borrowers may have repaid late but still fully repaid

PD 0.12% of loans are defaulting, which is very low.

LGD (Loss Given Default) 40% of total outstanding loans	Still correct, assuming 40% loss given default.

EAD (Exposure at Default) Total outstanding loans	Makes sense, showing loans yet to be repaid.

Expected Loss PD × LGD × EAD	Very small loss amount, since PD is so low.

Monthly fluctuations exist

Negative profits align with default spikes, indicating a need for improved credit risk management.

Projected profit for the next 3 months shows a moderate decline, warranting intervention.(Forecasting is based on a large amount of historical data, spanning over 2 years)



#### 4.5 Portfolio Triggers & Alerts Suppgustions

High Default Alert triggered due to PD exceeding 5%.

Liquidity Risk Alert activated due to projected losses.

Late Payment Alerts are frequent, necessitating borrower engagement strategies.

### 5. Recommendations

#### 5.1 Lending Product Adjustments

Introduce risk-based pricing to offer lower fees to reliable borrowers.

Reduce manual repayment dependency by encouraging automated payments.

Enhance borrower screening to reduce high-risk lending.

#### 5.2 Risk Management Strategies

Implement proactive collections for loans overdue by 5+ days.

Increase provisioning based on the ECL model.

Monitor and adjust credit limits based on past repayment behavior.

#### 5.3 Enhancing Forecast Accuracy

Use machine learning models for better default prediction.

Refine seasonal adjustments for improved trend detection.

Introduce real-time monitoring dashboards to track financial health.

6. Conclusion

This analysis highlights key insights into loan disbursement, repayment behaviors, credit risk, and financial performance. 
By implementing risk-based pricing, enhancing automated payments, and strengthening risk provisioning, the institution can optimize profitability while minimizing defaults.


---
**Report by:** Austine Mandela 
