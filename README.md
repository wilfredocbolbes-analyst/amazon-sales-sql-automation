# Amazon Store Net Profit & Return Analytics (SQL Automation)

## 📌 Project Overview
This project demonstrates how a Data Analyst can automate financial and inventory reporting for Amazon e-commerce sellers. Instead of manually combining multiple messy Excel files every week, this SQL system automatically centralizes data to calculate **True Net Profit** and track **Defective Returns**.

## 🛠️ The Problem
The client (an Amazon Seller) had their business data scattered across three different sources:
1. **Supplier COGS Sheet:** Cost of purchasing items from the manufacturer.
2. **Freight Logistics Invoice:** Shipping costs to the Amazon FBA warehouse.
3. **Amazon Seller Central Settlement Report:** Raw text files containing sales revenue and Amazon FBA fees.

Manually doing `VLOOKUP`s on thousands of rows in Excel caused lagging and human errors.

## 💡 The Solution (SQL Database Design)
I created an automated relational database with three tables (`supplier_cogs`, `shipping_costs`, and `amazon_raw_reports`) and used **SQL JOINs** to link the data dynamically via the **SKU (Stock Keeping Unit)**.

### The SQL Query:
```sql
SELECT 
    amz.order_id,
    amz.sku,
    amz.sales_amount AS revenue,
    amz.amazon_fba_fees AS amazon_fees,
    cogs.item_cost AS supplier_cost,
    ship.freight_cost AS shipping_cost,
    -- Computing the actual Net Profit per order
    (amz.sales_amount - amz.amazon_fba_fees - cogs.item_cost - ship.freight_cost) AS net_profit
FROM amazon_raw_reports amz
LEFT JOIN supplier_cogs cogs ON amz.sku = cogs.sku
LEFT JOIN shipping_costs ship ON amz.sku = ship.sku;
