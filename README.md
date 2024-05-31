# Financial Analytics Dashboard

## Project Overview

The **Financial Analytics Dashboard** project aims to provide a comprehensive financial analysis for a set of companies. The dashboard is built using Power BI and incorporates various financial metrics, allowing users to visualize and filter data interactively.

### Project Goals

- Visualize the total spend and income for each company.
- Compare spending versus income and profit.
- Track financial trends over time.
- Analyze the geographical distribution of customers and identify major contributors to income.

### Tools and Technologies

- **Power BI Desktop**: For data visualization and dashboard creation.
- **Excel**: Source of raw data.
- **DAX (Data Analysis Expressions)**: For creating measures and calculated columns.
- **Power Query**: For data transformation and cleaning.

## Data Sources

1. **SpendCategories.xlsx**:
    - **Spend Categories** sheet: Defines parent-level categories.
   <img width="647" alt="Знімок екрана 2024-05-31 о 12 50 31" src="https://github.com/IraSafonik/Reebit_Financial-Analytics_Dashboard/assets/32171563/cf7b42da-4f09-4cb3-8e51-81934fac0368">

    - **Spend Subcategories** sheet: Defines detailed subcategories and references parent categories.
<img width="902" alt="Знімок екрана 2024-05-31 о 12 50 22" src="https://github.com/IraSafonik/Reebit_Financial-Analytics_Dashboard/assets/32171563/1ba3e2dc-4a96-41b6-b784-62b208e9b185">

2. **CompanySpends.xlsx**:
    - **Spend Transactions** sheet: Contains detailed spending transactions.
<img width="1109" alt="Знімок екрана 2024-05-31 о 12 52 24" src="https://github.com/IraSafonik/Reebit_Financial-Analytics_Dashboard/assets/32171563/72d3471e-f6d8-43f7-bc1c-451a12da7df7">

    - **Company Budgets** sheet: Contains company spend budgets broken down by quarters.
<img width="902" alt="Знімок екрана 2024-05-31 о 12 50 40" src="https://github.com/IraSafonik/Reebit_Financial-Analytics_Dashboard/assets/32171563/a5d84e72-46df-462c-b68e-81fbe1812972">


3. **CompanyIncomes.xlsx**:
    - **Income Transactions** sheet: Contains detailed income transactions.
   <img width="732" alt="Знімок екрана 2024-05-31 о 12 47 38" src="https://github.com/IraSafonik/Reebit_Financial-Analytics_Dashboard/assets/32171563/19d1b950-f229-40d9-a7ce-37844937dc56">

    - **Customers** sheet: Contains customer data, including Country and City.
<img width="821" alt="Знімок екрана 2024-05-31 о 12 48 01" src="https://github.com/IraSafonik/Reebit_Financial-Analytics_Dashboard/assets/32171563/95e7ab66-ec53-45d5-b586-24ae9af3779e">


## Process and Steps

### Step 1: Data Loading and Cleaning

1. **Load Data**:
    - Open Power BI Desktop.
    - Use the Excel connector to load the data from the provided Excel files.

2. **Clean Data in Power Query**:
    - Remove any empty rows and unnecessary columns.
    - Ensure all data types are correctly set (e.g., dates, numbers).

### Step 2: Data Modeling

1. **Create Relationships**:
    - Define relationships between fact tables (spending and income transactions) and dimension tables (categories, companies, customers).
    - Use Star Schema to structure the data model.

2. **Rename Tables**:
    - Follow naming conventions: `Dim_{TableName}` for dimension tables and `Fact_{TableName}` for fact tables.

### Step 3: Creating Measures

1. **Total Income**:
    ```DAX
    TotalIncome = SUM('Fact_CompanyIncomes'[AmountReceivedUsd])
    ```

2. **Total Spend**:
    ```DAX
    TotalSpend = SUM('Fact_CompanySpends'[AmountSpentUsd])
    ```

3. **Profit**:
    ```DAX
    Profit = [TotalIncome] - [TotalSpend]
    ```

4. **Customer Income Percentage**:
    ```DAX
    CustomerIncomePercentage = 
    DIVIDE (
        SUM ( 'Fact_CompanyIncomes'[AmountReceivedUsd] ),
        CALCULATE ( [TotalIncome], ALL ( 'Dim_Customers' ) )
    )
    ```
5. **Create a Calendar Table**:
    - In Power BI, go to "Modeling" -> "New Table" and enter the following DAX formula:
    ```DAX
    Calendar = 

### Step 4: Create Relationships between tables

![Screenshot_7](https://github.com/IraSafonik/Reebit_Financial-Analytics_Dashboard/assets/32171563/3f39dade-0238-4981-9f40-df018d3fd77a)

### Step 5: Creating Visuals: 

#### Page 1: Basic Spend Overview

1. **Total Spend by Company**:
    - Create a clustered column chart.
    - Add `CompanyName` to the Axis.
    - Add `TotalSpend` to the Values.
    - Sort values from lowest to highest.

2. **Transaction Table**:
    - Create a table visual.
    - Add `CompanyName`, `TransactionDate`, `SubCategoryName`, `CategoryName`, and `AmountSpent` columns.

3. **Filters**:
    - Add slicers for Spend Category, Subcategory, and Date Range.
    - Add menu items for All, Budgeted, and Not Budgeted spends using bookmarks.

4. **Total Spend Card**:
    - Add a card visual to display `TotalSpend` (filter agnostic).

5. **Top 10 Spend Transactions**:
    - Create a table visual.
    - Add `Top 10 Spend Transactions` measure based on selected filters.

![Screenshot_1](https://github.com/IraSafonik/Reebit_Financial-Analytics_Dashboard/assets/32171563/6fe3aff1-f870-44e5-936b-fde285c641b3)

#### Page 2: Spend vs Income

1. **Total Spend vs Total Income by Company**:
    - Create a clustered column chart.
    - Add `CompanyName` to the Axis.
    - Add `TotalSpend` and `TotalIncome` to the Values.
    - Sort by company name ascending.

2. **Trends Over Time**:
    - Create a line chart.
    - Add `Date` to the Axis.
    - Add `TotalSpend` and `TotalIncome` to the Values.

3. **Date Range Filter**:
    - Add a date range slicer to affect all visuals on the page.

4. **Company Selector**:
    - Add a slicer for `CompanyName`.
    - Style it as a horizontal list with single selection enabled.

![Screenshot_2](https://github.com/IraSafonik/Reebit_Financial-Analytics_Dashboard/assets/32171563/83227fef-c0df-4df8-8fa3-da1f09f40690)

#### Page 3: Profitability

1. **Total Spend vs Total Income vs Profit by Company**:
    - Create a clustered column chart.
    - Add `CompanyName` to the Axis.
    - Add `TotalSpend`, `TotalIncome`, and `Profit` to the Values.
    - Sort by profit value from largest to lowest.

2. **Spend, Income, and Profit by Month/Quarter**:
    - Create a clustered column chart.
    - Add `Month/Quarter` to the Axis (use a switch parameter).
    - Add `TotalSpend`, `TotalIncome`, and `Profit` to the Values.
    - Add a slicer for `Month`/`Quarter`.

3. **Profitable Periods Table**:
    - Create a table visual.
    - Add a single column for profitable months/quarters.
    - Order by month (January to December) or quarter (Q1 to Q4).

![Screenshot_4](https://github.com/IraSafonik/Reebit_Financial-Analytics_Dashboard/assets/32171563/7ad1f16a-05ea-47b4-80c7-136befed97fc)


#### Page 4: Client Base (Optional)

1. **Map Visualization**:
    - Add a map visual to display customer locations.
    - Use `City` and `Country` fields from `Dim_Customers`.

2. **Company Selector**:
    - Add a slicer for `CompanyName` to the right side.

3. **Big Customers Table**:
    - Create a table visual.
    - Add `Customer`, `AmountReceivedUsd`, and `CustomerIncomePercentage` columns.
    - Filter to show customers contributing to 80% of the total income.

4. **Transactions Over Time**:
    - Create a line chart.
    - Add `TransactionDate` to the Axis.
    - Add `AmountReceivedUsd` to the Values.
    - Filter by selected customer from the map.

5. **Top 5 Countries by Customer Count**:
    - Create a table visual.
    - Add `Country` and `CustomerCount` columns.
    - Order by customer count descending.

![Screenshot_6](https://github.com/IraSafonik/Reebit_Financial-Analytics_Dashboard/assets/32171563/48472bee-10fe-4d2e-8e8e-254d9ae20dc1)

## Results and Conclusion

The Financial Analytics Dashboard provides a detailed view of company spending, income, and profitability. Users can interact with various filters and visuals to gain insights into financial performance and customer distribution. This project showcases the power of Power BI in transforming raw data into actionable insights, facilitating better decision-making and financial planning.

