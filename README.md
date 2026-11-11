# AdventureWorks Power BI Business Intelligence Dashboard

![Power BI](https://img.shields.io/badge/Power%20BI-Business%20Intelligence-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Data%20Analysis-0078D4?style=for-the-badge)
![Power Query](https://img.shields.io/badge/Power%20Query-ETL-217346?style=for-the-badge)
![GitHub](https://img.shields.io/badge/GitHub-Repository-181717?style=for-the-badge&logo=github)

## Project Overview

This project presents an end-to-end Business Intelligence solution developed using **Microsoft Power BI** and the **AdventureWorks dataset**.

The solution transforms raw sales, customer, product, returns, calendar, and territory data into an interactive business dashboard. It allows stakeholders to monitor company performance, identify sales and profitability trends, evaluate product performance, understand customer behavior, and track product returns.

The project demonstrates the complete Power BI development workflow:

```text
Raw Data
   ↓
Power Query ETL
   ↓
Data Cleaning and Transformation
   ↓
Relational Data Modeling
   ↓
DAX Calculations
   ↓
Interactive Dashboard Development
   ↓
Business Insights
```

---

## Business Problem

AdventureWorks requires a centralized reporting solution that allows management to monitor business performance and answer questions such as:

- How much revenue and profit has the company generated?
- How is business performance changing over time?
- Which products and categories generate the most revenue?
- Which products have the highest return rates?
- Which customers contribute the most revenue?
- How does performance differ across geographic regions?
- Are monthly orders, revenue, and profit meeting their targets?
- What are the recent trends in sales and profitability?

This Power BI dashboard provides an interactive solution for analyzing these areas.

---

## Business Objectives

The main objectives of the project are to:

- Track revenue, profit, orders, customers, and returns
- Compare current performance with previous periods
- Monitor business performance against defined targets
- Analyze sales and profitability trends over time
- Identify high-performing and underperforming products
- Compare product categories and subcategories
- Monitor product return volumes and return rates
- Identify high-value customers
- Analyze customer demographics and purchasing patterns
- Compare business performance across geographic regions
- provide stakeholders with an interactive decision-support dashboard

---

## Technologies Used

| Technology | Purpose |
|---|---|
| Microsoft Power BI Desktop | Dashboard development and data visualization |
| Power Query | Data extraction, cleaning, and transformation |
| DAX | Measures, calculated columns, KPIs, and time intelligence |
| Power BI Data Model | Table relationships and analytical modeling |
| CSV | Source data storage |
| Git | Local version control |
| GitHub | Repository hosting and project documentation |
| Visual Studio Code | Repository and documentation management |

---

## Repository Structure

```text
AdventureWorks-PowerBI-Dashboard/
│
├── AdventureWorks.pbip
│
├── AdventureWorks.Report/
│   └── Power BI report definitions
│
├── AdventureWorks.SemanticModel/
│   └── Power BI semantic model definitions
│
├── AdventureWorks Images/
│   └── Images and icons used in the report
│
├── AdventureWorks Raw Data/
│   └── CSV source files used by the project
│
├── screenshots/
│   ├── executive-dashboard.png
│   ├── customer-detail.png
│   ├── product-detail.png
│   └── data-model.png
│
├── documentation/
│   ├── data-model.md
│   ├── data-transformation.md
│   └── dax-measures.md
│
├── .gitignore
├── LICENSE
└── README.md
```

> The exact folder structure may vary depending on the Power BI Project format and the location of supporting data files.

---

## Dashboard Preview

### Executive Dashboard

The executive dashboard provides a consolidated view of overall business performance, including revenue, profit, orders, returns, monthly trends, product performance, and target comparisons.

![Executive Dashboard](screenshots/executive-dashboard.png)

### Customer Analysis

The customer analysis page presents customer-level insights, customer segmentation, purchasing behavior, revenue contribution, and high-value customer identification.

![Customer Analysis](screenshots/customer-detail.png)

### Product Analysis

The product analysis page allows users to examine product sales, profitability, orders, returns, and performance against targets.

![Product Analysis](screenshots/product-detail.png)

### Data Model

The Power BI data model connects sales and returns fact tables with customer, product, calendar, and territory dimensions.

![Data Model](screenshots/data-model.png)

> If an image is not displayed, confirm that the corresponding PNG file exists in the `screenshots` folder and that its filename matches the name used above.

---

## Data Sources

The project uses structured CSV files containing AdventureWorks business data.

The main data areas include:

- Sales transactions
- Product returns
- Customer information
- Product information
- Product categories
- Product subcategories
- Sales territories
- Calendar and date information

These source files are imported and transformed using Power Query before being loaded into the Power BI data model.

---

## Data Preparation and Transformation

Power Query is used to clean and prepare the raw data before analysis.

The transformation process includes:

- Importing multiple CSV files
- Combining sales files from a folder
- Promoting headers
- Assigning appropriate data types
- Removing unnecessary columns
- Renaming columns for readability
- Handling null and blank values
- Creating date-related columns
- Standardizing text values
- Creating conditional columns
- Combining related lookup data
- Validating column quality and consistency
- Preparing fact and dimension tables for modeling

The transformed data is then loaded into the Power BI semantic model.

---

## Data Model

The project uses a relational analytical model containing fact and dimension tables.

### Fact Tables

#### Sales Data

Contains transactional sales information such as:

- Order date
- Stock date
- Order number
- Product key
- Customer key
- Territory key
- Order quantity

#### Returns Data

Contains product-return information such as:

- Return date
- Product key
- Territory key
- Return quantity

### Dimension Tables

The principal dimension tables include:

- Calendar
- Customers
- Products
- Product Subcategories
- Product Categories
- Territories

### Model Design

The model primarily uses **one-to-many relationships**, with the dimension tables filtering the fact tables.

A simplified representation is shown below:

```text
                         Calendar
                            │
                            │
Customers ─────────────── Sales ─────────────── Products
                            │                     │
                            │                     │
                       Territories          Subcategories
                                                  │
                                                  │
                                              Categories


Products ────────────── Returns ───────────── Territories
```

This design supports efficient filtering, reusable measures, drill-down analysis, and consistent reporting across report pages.

---

## Key DAX Measures

The project uses DAX to calculate business KPIs, comparisons, targets, and time-based metrics.

### Revenue and Profitability

```DAX
Total Revenue =
SUMX(
    Sales,
    Sales[Order Quantity] * RELATED(Products[Product Price])
)
```

```DAX
Total Cost =
SUMX(
    Sales,
    Sales[Order Quantity] * RELATED(Products[Product Cost])
)
```

```DAX
Total Profit =
[Total Revenue] - [Total Cost]
```

```DAX
Profit Margin =
DIVIDE(
    [Total Profit],
    [Total Revenue],
    0
)
```

### Orders and Customers

```DAX
Total Orders =
DISTINCTCOUNT(Sales[Order Number])
```

```DAX
Total Customers =
DISTINCTCOUNT(Sales[Customer Key])
```

```DAX
Average Order Value =
DIVIDE(
    [Total Revenue],
    [Total Orders],
    0
)
```

### Returns

```DAX
Total Returns =
SUM(Returns[Return Quantity])
```

```DAX
Return Rate =
DIVIDE(
    [Total Returns],
    [Quantity Sold],
    0
)
```

### Time Intelligence

```DAX
Previous Month Revenue =
CALCULATE(
    [Total Revenue],
    DATEADD(
        Calendar[Date],
        -1,
        MONTH
    )
)
```

```DAX
Previous Month Orders =
CALCULATE(
    [Total Orders],
    DATEADD(
        Calendar[Date],
        -1,
        MONTH
    )
)
```

```DAX
Previous Month Profit =
CALCULATE(
    [Total Profit],
    DATEADD(
        Calendar[Date],
        -1,
        MONTH
    )
)
```

```DAX
90-Day Rolling Profit =
CALCULATE(
    [Total Profit],
    DATESINPERIOD(
        Calendar[Date],
        MAX(Calendar[Date]),
        -90,
        DAY
    )
)
```

> Measure names and table references may differ depending on the final semantic model.

---

## Dashboard Features

The Power BI report demonstrates the following capabilities:

- KPI cards
- Revenue and profit trend analysis
- Order and return tracking
- Product-level analysis
- Customer-level analysis
- Geographic analysis
- Category and subcategory drill-down
- Interactive slicers
- Cross-filtering between visuals
- Drill-through functionality
- Conditional formatting
- Report page tooltips
- Dynamic field parameters
- Bookmarks
- Page navigation
- Target comparisons
- Time-intelligence calculations
- Mobile layout concepts
- Performance optimization

---

## Key Performance Indicators

The dashboard tracks evenue remaining after product costs |
| Profit Margin | Profit as a percentage of revenue |
| Total Orders | Number of distinct customer orders |
| Total Customers | Number of distinct customers |
| Quantity Sold | Total number of products sold |
| Total Returns | Total quantity of returned products |
| Return Rate | Returned quantity relative to quantity sold |
| Average Order Value | Average revenue generated per order |
| Monthly Revenue | Revenue generated during the selected month |
| Monthly Profit | Profit generated during the selected month |
| Monthly Orders | Orders recorded during the selected month |

---

## Report Pages

### 1. Executive Summary

Provides a high-level view of:

- Total revenue
- Total profit
- Total orders
- Return rate
- Performance against targets
- Revenue trends
- Product performance
- Recent business activity

### 2. Product Analysis

Provides detailed analysis of:

- Product sales
- Product revenue
- Product profit
- Product return volume
- Product return rate
- Category and subcategory performance
- Product performance against targets

### 3. Customer Analysis

Provides insights into:

- Total customers
- Unique customers
- Revenue per customer
- Customer income levels
- Customer occupations
- Customer purchasing behavior
- Top customers by revenue
- Customer order frequency

### 4. Geographic Analysis

Supports regional comparison through:

- Territory analysis
- Country-level performance
- Continent-level performance
- Regional sales distribution
- Revenue and orders by location

---

## Example Business Insights

The dashboard is designed to help stakeholders identify insights such as:

- Products generating high revenue but relatively low profit
- Products with unusually high return rates
- Regions contributing the most revenue
- Categories driving company profitability
- Monthly changes in orders, revenue, and profit
- Customers with the highest lifetime revenue
- Differences in purchasing behavior across customer segments
- Business areas performing above or below target

The exact findings depend on the active report filters and selected reporting period.

---

## How to Open the Project

### Prerequisites

Install the following:

- Microsoft Power BI Desktop
- Git
- Visual Studio Code, recommended
- GitHub Desktop, optional

### Clone the Repository

```bash
git clone https://github.com/PratyakshSinghal/AdventureWorks-PowerBI-Dashboard.git
```

Move into the repository:

```bash
cd AdventureWorks-PowerBI-Dashboard
```

### Open the Power BI Project

Open the following file using Power BI Desktop:

```text
AdventureWorks.pbip
```

Power BI Desktop should load the associated report and semantic model folders.

---

## Data Source Configuration

After cloning the repository, the source file paths may still point to the original local folder.

If Power BI reports a missing-file or invalid-path error:

1. Open the project in Power BI Desktop.
2. Select **Home → Transform data → Data source settings**.
3. Select the relevant CSV or folder data source.
4. Choose **Change Source**.
5. Browse to the local `AdventureWorks Raw Data` directory.
6. Confirm the new path.
7. Select **Refresh Preview** in Power Query.
8. Select **Close & Apply**.
9. Refresh the report.

For better portability, Power Query parameters or relative folder conventions can be used instead of user-specific absolute paths.

---

## Version Control Workflow

After making changes in Power BI Desktop, save the project and use:

```bash
git status
```

Stage the updated files:

```bash
git add .
```

Commit the changes:

```bash
git commit -m "Updated Power BI dashboard"
```

Push the changes to GitHub:

```bash
git push
```

Example of a more descriptive commit:

```bash
git commit -m "Added customer segmentation and product return analysis"
```

---

## Project Limitations

- The dashboard was developed using the AdventureWorks sample dataset.
- The report is intended for learning, demonstration, and portfolio purposes.
- Results should not be interpreted as current performance for a real organization.
- Power BI Service features may require a Microsoft organizational account and an appropriate Power BI or Fabric license.
- Local file paths may need to be updated after cloning the repository.
- Large or confidential source datasets should not be committed to a public GitHub repository.

---

## Future Enhancements

Potential future improvements include:

- Adding a dedicated inventory analysis page
- Implementing advanced forecasting
- Adding year-over-year comparisons
- Introducing dynamic metric selection
- Creating additional report page tooltips
- Implementing row-level security
- Publishing the report to Power BI Service
- Connecting the report to a SQL database
- Automating refresh through a supported gateway
- Adding incremental refresh for larger datasets
- Creating automated data-quality checks
- Adding detailed DAX and Power Query documentation
- Implementing deployment pipelines for development, testing, and production environments

---

## Skills Demonstrated

This project demonstrates practical experience in:

- Business Intelligence
- Data analysis
- Data cleaning
- ETL development
- Power Query
- DAX
- Data modeling
- Star and snowflake schema concepts
- Time-intelligence calculations
- KPI development
- Dashboard design
- Data visualization
- Business insight generation
- Git version control
- GitHub project documentation

---

## Disclaimer

This repository was created for educational and portfolio purposes.

AdventureWorks is a sample dataset commonly used for demonstrating Microsoft data and analytics technologies. Any third-party names, datasets, trademarks, or materials remain the property of their respective owners.

Course materials or copyrighted training documents are not redistributed through this repository.

---

## Author

**Pratyaksh Singhal**

Software Engineer with experience and interests in:

- Power BI
- SQL
- Python
- Data Analytics
- Data Engineering
- Business Intelligence
- Data Validation and Automation

### GitHub

https://github.com/PratyakshSinghal

---

## Repository Link

[AdventureWorks Power BI Dashboard](https://github.com/PratyakshSinghal/AdventureWorks-PowerBI-Dashboard)

---

## Support

If this project is useful, consider giving the repository a star.

Contributions, suggestions, and feedback are welcome.
