# SQL Noir: Case Files
## Individual Group 2 Project 1 — Mohammad Mustafa

**Database:** WideWorldImporters

---

## Introduction

Hello, I'm Mohammad Mustafa, and this is my analysis of the WideWorldImporters database case files. I'll be presenting six investigative cases where I used SQL queries to uncover anomalies and suspicious patterns in the data.

---

## Case #001 — After-Hours Hand-Off

**Logline:** A signature appears in the dead of night. The name doesn't ring any bells.

In this case, I was investigating suspicious delivery patterns. Let me walk you through my findings.

### Task 1 — After-hours invoices

First, I searched for invoices with confirmed delivery times outside normal business hours—specifically, before 7 AM or after 9 PM. To my surprise, the data revealed no actual after-hours deliveries. The records I found were for deliveries between 7 and 8 AM, which is still within normal business hours. This suggested either the data was clean, or the anomaly lay elsewhere.

### Task 2 — Unknown receivers

Next, I investigated who was receiving these deliveries. I cross-referenced the "Confirmed Received By" field against the Application.People table. I discovered several invoices where the receiver's name didn't match any person in the system—these were flagged as "Unknown Receivers."

### Task 3 — Expected delivery dates

I then examined the expected delivery dates per order to provide context. I calculated the days difference between the expected delivery date and the actual confirmed delivery time. This helped identify any patterns in delayed or early deliveries.

### Task 4 — Atypical delivery methods

Finally, I compared each delivery against the customer's typical delivery method. I created a query that identified customers using unusual delivery methods—ones that deviated from their most common method. This could indicate irregular shipping practices or data entry errors.

### Consolidated Findings

My consolidated query combined all these flags, creating a comprehensive view of suspicious invoices that featured early morning deliveries, unknown receivers, and atypical delivery methods.

---

## Case #002 — The Cut-Rate Conspiracy

**Logline:** Someone is slipping prices beneath the deal floor—sometimes with no deal at all.

This case investigated price anomalies where items were sold below their proper pricing.

### Task 1 — Line-level facts table

I built a comprehensive facts table at the invoice line level, capturing the date, customer information, item details, and price. This included customer categories and buying groups to properly match against special deals.

### Task 2 — Applicable deals by date and audience

I analyzed the special deals table to understand which discounts were available. I found deals targeted at specific customers, buying groups, customer categories, and general deals available to everyone.

### Task 3 — Baseline prices

I established a baseline price for each item by calculating the average unit price from historical sales. This gave me the normal price point for comparison.

### Task 4 — Flagging suspicious pricing

I developed a query that flagged invoice lines where the sale price was below either the lowest applicable special deal price or, when no deal existed, below the baseline average price. This identified potentially fraudulent or erroneous pricing.

### Consolidated Findings

The final analysis showed that some items were indeed sold at suspiciously low prices—either below their deal floor or discounted when no special deal was in effect for that customer or item.

---

## Case #003 — The Empty-Crate Riddle

**Logline:** Orders marked "complete." Pallets still full. Who closed the books?

I investigated orders that were marked as picked and completed, but where items were never actually picked.

### Task 1 — Orders with picking completion

I identified all orders where the "Picking Completed When" field was not null, indicating the order was marked as fully picked.

### Task 2 — Zero-picked lines

Shockingly, I found numerous order lines with zero picked quantity—meaning items were ordered but never physically removed from inventory. Yet the overall order was marked complete.

### Task 3 — Invoicing anyway

Even more concerning, I discovered that these orders were still invoiced to customers, despite never having been picked. This represented a serious operational and financial discrepancy.

### Task 4 — Zero-pick tally per order

I calculated the percentage of zero-picked lines per completed order, showing that some orders had a significant portion—or even all—of their items unpicked, yet were marked complete and invoiced.

### Consolidated Findings

The analysis revealed a pattern where orders could be marked complete and invoiced even when no items were actually picked, creating potential inventory shrinkage and customer relationship issues.

---

## Case #004 — The Warehouse Wraith

**Logline:** Stock walks out without a paper trail. The cameras caught a shadow; the ledger caught nothing.

I investigated negative stock movements—inventory reductions with no supporting documentation.

### Task 1 — Negative stock movements

I identified all stock transactions with negative quantities, representing items leaving inventory.

### Task 2 — No linked paperwork

I filtered for transactions where all documentation fields were null—no invoice ID, purchase order ID, customer ID, or supplier ID. These were "ghost" transactions with no supporting paperwork.

### Task 3 — Transaction details

I decorated these records with item names and transaction type names to make the data more readable and actionable.

### Task 4 — Scoreboard analysis

Finally, I created a scoreboard showing which items had the most ghost transactions—items that were leaving inventory without any documented reason.

### Consolidated Findings

The investigation revealed systematic stock movements with no paper trail, representing potential theft, error, or data quality issues in the warehouse management system.

---

## Case #005 — The Cold-Room Conspiracy

**Logline:** The cooler coughs. The thermometer jumps. Trucks idle after dark.

This case investigated temperature spikes in cold storage, potentially indicating compromised product quality.

### Task 1 — Temperature spike days

I analyzed cold room temperature archives and flagged days where the maximum temperature exceeded 6 degrees Celsius—beyond safe storage ranges for refrigerated items.

### Task 2 — Chiller volume per day

I calculated the volume of chiller items being handled each day by summing the chiller item quantities from invoices.

### Task 3 — After-hours deliveries

I identified late-night or very early morning deliveries, tracking delivery times outside normal business hours.

### Task 4 — Correlation analysis

I joined these datasets to look for correlations—were temperature spikes happening on days with high chiller volumes? Or on days with unusual after-hours delivery schedules?

### Consolidated Findings

The analysis revealed specific dates where cold storage temperatures spiked, potentially jeopardizing product quality. I documented these incidents alongside the corresponding shipping volumes and delivery patterns to identify root causes.

---

## Case #006 — The Credit-Hold Caper

**Logline:** Finance freezes the line. Sales thaw it out with a smile.

I investigated customers with high credit limits and their invoice patterns.

### Task 1 — Customers on credit hold

I identified customers with substantial credit limits—focusing on those with limits above 50,000 dollars, which represents significant exposure.

### Task 2 — Invoice retrieval

I pulled all invoices for these high-credit-limit customers to analyze their purchasing patterns.

### Task 3 — Invoice amount calculation

I computed total invoice amounts from line items, ensuring accurate financial totals. This involved multiplying quantities by unit prices and summing across all lines.

### Task 4 — Sorting and prioritization

Finally, I sorted the results by most recent invoices and largest amounts to prioritize the highest-value, most recent transactions.

### Consolidated Findings

The analysis provided a clear picture of which high-credit customers had the most recent and largest transactions, helping identify potential credit risk or unusual spending patterns.

---

## Case #007 — The Banking Switcheroo

**Logline:** A supplier's bank digits dance, and a fat wire follows the beat.

I investigated supplier banking information changes and coinciding large payments.

### Task 1 — Supplier change moments

I identified when supplier records changed by using the "Valid From" field as the timestamp of supplier information updates—potentially indicating banking information changes.

### Task 2 — Supplier transactions with types

I retrieved all supplier transactions and joined them with transaction type names to understand the nature of each payment or receipt.

### Task 3 — Same-day high-value payments

I looked for transactions occurring on the exact same day as supplier information changes, focusing on high-value transactions above 10,000 dollars—potential red flags for fraud.

### Task 4 — Payment-specific types

I narrowed the results to payment and receipt transaction types, filtering out other types of transactions to focus on actual money movements.

### Consolidated Findings

The investigation identified instances where supplier banking information changed and large payments occurred on the same day—a classic pattern that could indicate fraud or payment redirection schemes.

---

## Conclusion

Each case demonstrated different types of anomalies in the WideWorldImporters database: from delivery anomalies and pricing discrepancies to inventory shrinkage and potential fraud indicators. Through systematic SQL analysis, I uncovered patterns that would require further investigation by management, auditors, or compliance teams.

The queries I developed could be automated to run regularly as part of ongoing fraud detection and operational monitoring programs.

Thank you for your attention.

