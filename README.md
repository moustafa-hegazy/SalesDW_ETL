# SSIS Demo Project - ETL for Sales Data

## Project Overview
This project demonstrates the ETL (Extract, Transform, Load) process using SQL Server Integration Services (SSIS) to transform and load sales data into a data warehouse. The process involves extracting sales data, performing various transformations, and loading it into the final destination table `Processed_Sales`.

## Data Sources
- **FactSales**: Contains the main sales data such as `SalesAmount`, `Quantity`, `ProductKey`, etc.
- **DimProduct**: Contains product details, such as `ProductName`, `ProductKey`, `ProductCategory`.
- **DimTime**: Contains date and time-related details like `Month`, `Year`, `Date`.

## Steps Involved in the ETL Process

### 1. **OLE DB Source**
   - The ETL process begins by extracting data from the `FactSales` table, which includes `SalesAmount`, `Quantity`, `ProductKey`, and other relevant fields.
   - This data serves as the source for further transformations.

### 2. **Data Conversion - Quantity**
   - The `Quantity` column's data type is converted from its original format to a **4-byte signed integer (DT_I4)**.
   - This ensures consistency in data types, making the transformation process smoother.

### 3. **Derived Column - TotalRevenue Calculation**
   - A derived column named `TotalRevenue` is created, calculated as:
     ```
     TotalRevenue = SalesAmount * Quantity
     ```
   - This derived column will be included in the final output to represent the total revenue for each sale.

### 4. **Lookup - Product Name from DimProduct**
   - A **Lookup Transformation** is used to match the `ProductKey` in `FactSales` with the corresponding `ProductKey` in the `DimProduct` dimension table.
   - The output of this transformation includes a new column `ProductName`, which is used in the final destination.

### 5. **Data Conversion - MonthYear Conversion**
   - The `MonthYear` field in the `FactSales` table is originally a **Unicode (DT_WSTR)** string.
   - A **Data Conversion Transformation** is applied to convert `MonthYear` from **Unicode (DT_WSTR)** to **non-Unicode string (DT_STR)**, ensuring compatibility with the destination table schema.
     
### 6. **OLE DB Destination - Loading into Processed_Sales**
   - The transformed data, now containing `TotalRevenue`, `ProductName`, and the converted `MonthYear`, is loaded into the `Processed_Sales` table.
   - Only rows with matching product data from the `DimProduct` table are loaded (i.e., matched rows from the lookup transformation).

## Project Components

- **SSIS Package**: The ETL process is implemented as an SSIS package using the following key transformations:
  - **Data Conversion**
  - **Derived Column**
  - **Lookup**
  - **OLE DB Destination**
  
## How to Run the Package

1. **Setup**:
   - Ensure SQL Server and SSIS are installed and configured on your machine.
   - The source tables (`FactSales`, `DimProduct`, `DimCustomer`, `DimTime`).
   - to make the installation less of a pain a .bak backup file was provided in the repo just restore and it will work

2. **SSIS Configuration**:
   - Open the project in **SQL Server Data Tools (SSDT)**.
   - Ensure the database connection strings in the package point to the correct server and database where your tables are located.

3. **Execution**:
   - Press **F5** or click **Start Debugging** to run the SSIS package.
   - The package will execute the ETL process, performing data conversion, lookups, and transformations, and then loading the processed data into the `Processed_Sales` table.

4. **Validation**:
   - After the package runs successfully, validate the results by querying the `Processed_Sales` table to ensure the transformations and lookup worked as expected.

