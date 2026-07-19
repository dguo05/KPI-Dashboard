# Pizza Sales Dashboard

## Project Overview

This project is an end-to-end **Power BI sales dashboard** built to analyse pizza sales performance, customer ordering patterns, and product-level performance.

The dashboard is designed for a pizza business that wants to quickly understand:

- How much revenue the business generated
- How many orders were placed
- How ordering behaviour changes by month, day, and time slot
- Which pizza categories and sizes perform best
- Which pizzas are the top and bottom performers by sales
- How slicers can be used to explore the data interactively

The project follows a typical business intelligence workflow: gathering business requirements, cleaning and transforming data, modelling tables, creating DAX measures, and designing an interactive report.

---

## Dashboard Preview


![Dashboard Preview](images/dashboard-preview.png)

---

## Business Requirements

The dashboard was built to answer the following business questions:

1. **Sales performance**
   - What is the total sales revenue?
   - How many total orders were placed?
   - What is the average number of pizzas per order?
   - What is the average number of orders per day?

2. **Customer demand patterns**
   - Which days of the week receive the most orders?
   - Which time slots are busiest?
   - Are there monthly or seasonal trends in order volume?

3. **Product performance**
   - Which pizza categories generate the most orders?
   - Which pizza sizes are ordered most often?
   - Which pizzas are the top 5 performers by total sales?
   - Which pizzas are the bottom 5 performers by total sales?

4. **Interactive analysis**
   - Can users filter the dashboard by pizza category?
   - Can users filter the dashboard by pizza size?
   - Can users switch between top and bottom performing pizzas?

---

## Tools Used

- **Power BI Desktop** - dashboard creation and data modelling
- **Power Query** - data cleaning and transformation
- **DAX** - calculated measures and time-based analysis
- **Excel/CSV data source** - raw sales dataset
- **GitHub** - project documentation and portfolio hosting

---

## Dataset Description

The dataset contains pizza sales transactions, including order information, pizza details, pricing, quantity sold, categories, and sizes.

The model uses the following main tables:

| Table | Description |
|---|---|
| `Orders` | Contains order-level information such as order ID, order date, order time, and time slot |
| `Order Details` | Contains line-level transaction data, including order detail ID, order ID, pizza ID, and quantity |
| `Pizzas` | Contains pizza-level information such as pizza ID, size, and price |
| `Pizza Type` | Contains pizza names, categories, and ingredients/type information |
| `Date Table` | Custom DAX date table used for time-based analysis |
| `All Measures` | Dedicated measure table used to organise DAX measures |

---

## Data Cleaning and Transformation

The data was prepared in **Power Query** before building the dashboard.

Key cleaning and transformation steps included:

- Imported the raw sales dataset into Power BI
- Promoted headers where required
- Renamed tables and columns into a cleaner reporting format
- Checked and corrected data types, especially date, time, quantity, and price fields
- Removed or handled null and missing values
- Created cleaner categorical fields for reporting
- Prepared the data for relationship modelling in Power BI

---

## Data Model

The dashboard uses a relational model with transaction tables connected to lookup/dimension tables.

Expected relationship structure:

```text
Date Table[Date]          1 --- *  Orders[Date]

Orders[Order Id]          1 --- *  Order Details[Order Id]

Pizzas[Pizza Id]          1 --- *  Order Details[Pizza Id]

Pizza Type[Pizza Type Id] 1 --- *  Pizzas[Pizza Type Id]
```

This structure allows the dashboard to analyse order volume, revenue, product performance, category performance, size performance, and time-based trends.

> Add a screenshot of the model view here.

![Data Model](images/data-model.png)

---

## DAX Measures

### Total Orders

Counts the number of unique orders.

```DAX
Total Orders =
DISTINCTCOUNT('Order Details'[Order Id])
```

---

### Total Sales

Calculates total revenue by multiplying the quantity sold in each order-detail row by the related pizza price, then summing the result.

```DAX
Total Sales =
SUMX(
    'Order Details',
    'Order Details'[Quantity] * RELATED(Pizzas[Price])
)
```

---

### Average Pizzas per Order

Calculates the average quantity of pizzas sold per order.

```DAX
Avg Pizzas per Order =
DIVIDE(
    SUM('Order Details'[Quantity]),
    DISTINCTCOUNT(Orders[Order Id])
)
```

> Note: If using `DISTINCTCOUNT('Order Details'[Order Details Id])` instead of `SUM('Order Details'[Quantity])`, the measure calculates average order lines per order, not the true average number of pizzas sold per order.

---

### Average Orders per Day

Calculates the average number of orders received per day.

```DAX
Avg Orders per Day =
DIVIDE(
    [Total Orders],
    DISTINCTCOUNT('Date Table'[Date])
)
```

---

### Date Table

Creates a dedicated date table for time-based reporting.

```DAX
Date Table =
ADDCOLUMNS(
    CALENDAR(MIN(Orders[Date]), MAX(Orders[Date])),
    "Year", YEAR([Date]),
    "Month", FORMAT([Date], "mmm"),
    "Month No", MONTH([Date]),
    "Quarter", "Q" & QUARTER([Date]),
    "Day", FORMAT([Date], "ddd"),
    "Day No", WEEKDAY([Date], 2)
)
```

The `Month No` and `Day No` columns are used to sort month and weekday names correctly in visuals.

---

## Dashboard Features

### KPI Cards

The report includes KPI cards for high-level performance tracking:

- Total Sales
- Total Orders
- Average Pizzas per Order
- Average Orders per Day

These cards give users an immediate overview of business performance.

---

### Monthly Order Trend

A line chart shows order volume by year and month.

This helps identify:

- Monthly trends
- Seasonal patterns
- Periods of higher or lower demand

---

### Orders by Day

A bar chart shows order volume by weekday.

This helps the business understand which days are busiest and may require more staff, stock, or preparation.

---

### Orders by Time Slot

A bar chart shows order volume by time slot.

This helps identify peak ordering periods, such as lunch or dinner rushes.

---

### Orders by Pizza Category

A donut chart shows the distribution of orders across pizza categories.

This helps compare category-level performance, such as Classic, Supreme, Veggie, or Chicken pizzas.

---

### Orders by Pizza Size

A donut chart shows the distribution of orders by pizza size.

This helps identify which sizes are most popular and can support pricing, promotion, and inventory decisions.

---

### Top 5 and Bottom 5 Pizza Analysis

The dashboard includes a table visual controlled by a bookmark navigator.

Users can switch between:

- **Top 5 pizzas by total sales**
- **Bottom 5 pizzas by total sales**

This helps identify high-performing products and products that may need review, promotion, or removal from the menu.

---

### Interactive Slicers

The dashboard includes dropdown slicers for:

- Pizza Category
- Pizza Size

These allow users to filter the dashboard and analyse specific segments of the business.

---

## Key Insights

Based on the dashboard design, the report is intended to uncover insights such as:

- Which months had the highest order volume
- Which days of the week are busiest
- Which time slots have the greatest customer demand
- Which pizza categories receive the most orders
- Which pizza sizes are most popular
- Which pizzas generate the highest and lowest sales

These insights can support decisions around staffing, stock planning, product promotions, menu optimisation, and sales strategy.

---

## How to Use This Project

1. Download or clone the repository.
2. Open the `.pbix` file in Power BI Desktop.
3. Review the data model relationships.
4. Check the DAX measures in the `All Measures` table.
5. Use the dashboard slicers to filter by pizza category and size.
6. Use the bookmark navigator to switch between top and bottom pizza performance.

---

## Skills Demonstrated

This project demonstrates the following data analytics and business intelligence skills:

- Data cleaning in Power Query
- Data transformation and preparation
- Relational data modelling
- Creating a custom date table
- Writing DAX measures
- Building KPI cards
- Creating line charts, bar charts, donut charts, tables, and slicers
- Using bookmarks and bookmark navigators
- Designing an interactive business dashboard
- Translating business questions into dashboard visuals
- Communicating insights through a portfolio project

---

## Project Workflow

### 1. Business Requirement Gathering

Defined the key questions the business needs to answer, including total sales, order trends, peak periods, best-selling products, and low-performing products.

### 2. Data Gathering

Imported the pizza sales dataset into Power BI and reviewed the available tables and columns.

### 3. Data Cleaning and Transformation

Used Power Query to clean the dataset, rename fields, check data types, and prepare the data for modelling.

### 4. Data Modelling

Created relationships between orders, order details, pizzas, pizza types, and the date table.

### 5. DAX Measure Creation

Created measures for total sales, total orders, average pizzas per order, and average orders per day.

### 6. Dashboard Design

Built an interactive dashboard using KPI cards, trend charts, bar charts, donut charts, slicers, and a bookmark navigator.

### 7. Insight Generation

Used the dashboard to identify sales trends, product performance, peak order periods, and opportunities for business improvement.

---

## Conclusion

This Power BI dashboard provides a clear overview of pizza sales performance and enables users to explore order trends, product performance, and customer demand patterns interactively.
The project demonstrates an end-to-end data analytics workflow, from business requirement gathering and data preparation to data modelling, DAX calculations, dashboard design, and insight communication.
