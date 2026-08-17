# E-Commerce Sales Dashboard — Power BI

An interactive Power BI dashboard analyzing 5,000 e-commerce orders across product categories, regions, and payment methods — built end-to-end in Power BI Desktop with custom DAX measures and calculated columns.
▶️ [View the interactive dashboard](https://1drv.ms/v/c/9c49aa112f029279/IQCbieR8Y-9aSoW9OSRAXGwAAT3EtP36IrVy6UilwaUppSg?e=7y5RTs)

## 📁 Dataset
- **Source:** `E-Commerce Sales Analytics.csv`
- **Size:** 5,000 orders
- **Fields:** order_id, order_date, customer_id, product_category, region, quantity, unit_price, discount, payment_method, delivery_days, customer_rating, revenue

## 🛠 What I Built

**KPI Cards**
- Total Revenue: 5.11M
- Total Orders: 5K
- Average Order Value: 1.02K
- Avg Customer Rating: 2.97
- Avg Delivery Days: 6.1

**Visuals**
- Total Revenue by Product Category (column chart)
- Total Revenue by Region (donut chart)
- Total Revenue by Year/Quarter/Month/Day (drillable line chart)
- Total Orders by Payment Method (bar chart)
- Avg Delivery Days vs. Customer Rating by Category (scatter)
- Slicers: Order Date, Region, Product Category, Payment Method

**DAX Measures**
```dax
Total Revenue = SUM('Table'[revenue])
Total Orders = COUNTROWS('Table')
Average Order Value = DIVIDE([Total Revenue], [Total Orders])
Total Units Sold = SUM('Table'[quantity])
Avg Discount = AVERAGE('Table'[discount])
Avg Delivery Days = AVERAGE('Table'[delivery_days])
Avg Customer Rating = AVERAGE('Table'[customer_rating])
Total Net Revenue = SUM('Table'[Net Revenue])
Total Customers = DISTINCTCOUNT('Table'[customer_id])
```

**Calculated Columns**
```dax
Net Revenue = 'Table'[revenue] * (1 - 'Table'[discount])
Order Month = FORMAT('Table'[order_date], "YYYY-MM")
Rating Tier = SWITCH(TRUE(),
    [customer_rating] >= 4, "High",
    [customer_rating] >= 2.5, "Medium",
    "Low"
)
```

## 🔍 Data Quality Fix
While building the date-based visuals, I found that Power Query's locale-based type conversion on `order_date` was silently corrupting the data: the source dates are formatted M/D/YYYY (US-style), but the conversion used a mismatched locale. This caused **3,023 of 5,000 rows (60%) to fail parsing and turn into blanks**, and the remaining rows risked having day/month silently swapped.

**Fix:** reloaded the source and re-applied the type conversion using `Change Type → Using Locale → English (United States)`, which correctly parses the M/D/Y format. Verified afterward that all 5,000 rows converted with zero blanks and the date range and revenue totals matched the raw CSV exactly.

## 💡 Key Insights
- Electronics is the top-performing category by revenue, followed by Clothing.
- Revenue is fairly evenly split across the four regions (~24-26% each).
- Card is the leading payment method by order volume, ahead of COD and Wallet.
- Average customer rating sits around 2.97/5 across categories, with a modest inverse relationship visible between delivery time and rating.

## 🧰 Tools
Power BI Desktop · DAX · Power Query

## 📂 Files
- `dashboard.pbix` — the Power BI file
- `dashboard-screenshot.png` — static preview
- `dashboard-demo.gif` — short interaction demo
