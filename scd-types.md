# Slowly Changing Dimensions (SCDs) in Data Modeling

## Table of Contents

1. [What is SCDs?](#1-what-is-scds)
2. [Different Types of SCDs](#2-different-types-of-scds)
3. [Example & Use Case of SCDs](#3-example--use-case-of-scds)
    - [Initial Load](#31-initial-load)
    - [Incremental Load](#32-incremental-load)
        - [Changes in Customer Data](#321-changes-in-customer-data)
        - [No Changes in Customer Data](#322-no-changes-in-customer-data)
        - [Deletions](#323-deletions)
4. [Points to Consider](#4-points-to-consider)
5. [Conclusion](#5-conclusion)


## 1. What is SCDs?

Slowly Changing Dimensions (SCDs) are a common concept and methodology used in data warehousing to manage and track changes in dimension data over time. Dimension tables in a data warehouse contain descriptive attributes or fields that are used for analyzing business data contained in fact tables. However, the attributes of these dimension tables may change over time. SCDs offer a strategy to handle such changes, ensuring that historical data remains accurate and is not lost, while also accommodating the current data state.

## 2. Different Types of SCDs

SCDs are categorized into several types, each handling changes in data differently:

1. **Type 0 - Fixed Dimension**: No changes are allowed; the data is static.
2. **Type 1 - No History**: When a change occurs, the old data is overwritten with the new data. This method does not track historical changes.
3. **Type 2 - Row Versioning**: Each time a change occurs, a new record is added to the table. This method involves adding new columns to track changes, such as version numbers or valid time ranges, thereby maintaining a full history of changes.
4. **Type 3 - Previous Value Column**: This method tracks changes by adding new columns to store the previous values of the changed attributes. However, it is limited in tracking history as it typically only keeps the immediate last value.
5. **Type 4 - History Table**: Changes are managed by creating a separate history table for the dimension. The original table holds the current data, while the history table stores changes and historical data.
6. **Type 5 - Hybrid (Combination of Types 1 and 2)**: This type combines the features of Types 1 and 2, where a new record is added to the table for each change, but the old record is retained as well, effectively maintaining a full history while also updating the current state.
7. **Type 6 - Hybrid (Combination of Types 1, 2, and 3)**: Sometimes referred to as the "Type 6 SCD", it combines aspects of Types 1, 2, and 3 to track changes, maintain a current view, and keep some history by having current and previous values in the same table along with a flag to indicate the latest record.

![SCDs](https://github.com/atadas10/Learn-Data-Modeling/assets/84840069/a6c77319-90f4-45fe-9613-d2c5daaaf626)


## 3. Example & Use Case of SCDs

Consider a retail business that tracks sales data. The business has a `Customers` dimension table that includes customer information such as `CustomerID`, `Name`, `Email`, and `Address`.

### 3.1. Initial Load

On Day-0, we have the initial load of the `Customers` table with the following data:

| CustomerID | Name     | Email                | Address       | StartDate | EndDate   | IsActive |
|------------|----------|----------------------|---------------|-----------|-----------|----------|
| 1          | John Doe | johndoe@example.com  | 123 Main St   | 2024-01-01| 9999-12-31| Yes      |

### 3.2. Incremental Load

#### 3.2.1. Changes in Customer Data

On Day-1, let's say customer John Doe moves to a new address "456 Elm St" and his email changes to "john.d@example.com".

| CustomerID | Name     | Email                | Address       | StartDate | EndDate   | IsActive |
|------------|----------|----------------------|---------------|-----------|-----------|----------|
| 1          | John Doe | john.d@example.com  | 456 Elm St   | 2024-01-01| 9999-12-31| Yes      |

- **Type 1 (No History):** This type would simply update John Doe's record with the new address and email. The history of his previous address and email is lost.

  | CustomerID | Name     | Email               | Address      | StartDate | EndDate   | IsActive |
  |------------|----------|---------------------|--------------|-----------|-----------|----------|
  | 1          | John Doe | john.d@example.com  | 456 Elm St   | 2024-01-01| 9999-12-31| Yes      |

- **Type 2 (Row Versioning):** This type would add a new row for John Doe with the new address and email, close the old record by setting its EndDate to the current date, and flag the new record as active.

  | CustomerID | Name     | Email               | Address      | StartDate | EndDate   | IsActive |
  |------------|----------|---------------------|--------------|-----------|-----------|----------|
  | 1          | John Doe | johndoe@example.com | 123 Main St  | 2024-01-01| 2024-02-01| No       |
  | 1          | John Doe | john.d@example.com  | 456 Elm St   | 2024-02-02| 9999-12-31| Yes      |

- **Type 3 (Previous Value Column):** This type would update John Doe's record with the new address and email and add the previous address in a new column `PreviousAddress`. The history of his previous email is lost unless a similar column is added for the email.

  | CustomerID | Name     | Email               | Address      | PreviousAddress | StartDate | EndDate   | IsActive |
  |------------|----------|---------------------|--------------|-----------------|-----------|-----------|----------|
  | 1          | John Doe | john.d@example.com  | 456 Elm St   | 123 Main St     | 2024-01-01| 9999-12-31| Yes      |

- **Type 5 (Combination of Types 1 and 2):** For Type 5, both the old and new records are retained, effectively maintaining a full history while updating the current state.

  | CustomerID | Name     | Email               | Address      | StartDate | EndDate   | IsActive |
  |------------|----------|---------------------|--------------|-----------|-----------|----------|
  | 1          | John Doe | johndoe@example.com | 123 Main St  | 2024-01-01| 2024-02-01| No       |
  | 1          | John Doe | john.d@example.com  | 456 Elm St   | 2024-02-02| 9999-12-31| Yes      |

- **Type 6 (Hybrid of Types 1, 2, and 3):** This type combines aspects of Types 1, 2, and 3. It maintains a current view of the data while also keeping some historical information.

  | CustomerID | Name     | Email               | Address      | PreviousAddress | StartDate | EndDate   | IsActive |
  |------------|----------|---------------------|--------------|-----------------|-----------|-----------|----------|
  | 1          | John Doe | johndoe@example.com | 123 Main St  |                 | 2024-01-01| 2024-02-01| No       |
  | 1          | John Doe | john.d@example.com  | 456 Elm St   | 123 Main St     | 2024-02-02| 9999-12-31| Yes      |


#### 3.2.2. No Changes in Customer Data

For customers with no changes, all SCD types would simply leave the records untouched.

#### 3.2.3. Deletions

Handling deletions in SCDs typically depends on the business requirements.

### 4. Points to Consider

- **Data Volume**: Implementing certain SCD types, especially Type 2, can significantly increase the size of dimension tables due to the addition of multiple records for a single entity over time.
- **Complexity**: The complexity of implementing and maintaining SCDs can vary significantly. Type 2 and Type 5, for instance, require more sophisticated ETL (Extract, Transform, Load) processes compared to Type 1.
- **Performance**: The choice of SCD can impact query performance. More complex types may slow down query execution due to larger table sizes or the need for additional joins.
- **Historical Accuracy vs. Storage**: Balancing the need for historical data accuracy against storage costs and system performance is crucial. Deciding which SCD type to use depends on business requirements and how critical historical data is to decision-making processes.

## 5. Conclusion

SCDs are essential for maintaining accurate and comprehensive historical data in a data warehouse. The choice of SCD type should be guided by the specific needs of the business and its data analysis requirements. While Type 1 may suffice for businesses with simple requirements or limited storage capacity, Type 2 or Type 5 may be better suited for those needing detailed historical data for analysis. Understanding the implications of each SCD type on data storage, system performance, and data retrieval is critical for effective data warehouse design and operation
