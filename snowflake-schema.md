## 2.3. Snowflake Schema

Snowflake schema is another data modeling technique used in data warehousing, similar to the [star schema](start-schema.md). However, in a snowflake schema, dimension tables are normalized, meaning they are broken down into multiple related tables, creating a more complex structure resembling a snowflake rather than a star.

### 2.3.1. Characteristics and Structure

In a snowflake schema, dimension tables are normalized, which means that redundant attributes are removed, and the dimension hierarchy is stored in separate tables. This normalization reduces data redundancy and improves data integrity but results in a more complex schema structure with additional join operations required for querying.

### 2.3.2. Advantages

- Improved Data Integrity: Normalization reduces data redundancy and ensures consistency across dimension tables.
- Space Efficiency: By eliminating redundant attributes, snowflake schema can be more space-efficient compared to star schema, especially for large dimension tables.
- Easier Maintenance: Changes to dimension attributes only need to be made in one place, reducing the risk of data inconsistency.

### 2.3.3. Disadvantages

- Increased Query Complexity: Snowflake schema requires additional join operations compared to star schema, leading to potentially slower query performance, especially for complex analytical queries.
- Query Performance: While snowflake schema may save space, the additional join operations can impact query performance, especially for large datasets.
- Complexity: Snowflake schema introduces additional complexity due to the normalized structure, making it more challenging to understand and maintain.

### 2.3.4. Example

Consider a "Product" dimension in a snowflake schema, where the dimension attributes are normalized into multiple tables. The main "Product" table may contain core attributes such as ProductID, ProductName, and CategoryID. The "Category" table stores additional information about product categories, such as CategoryID and CategoryName. Similarly, the "Brand" table contains details about product brands, such as BrandID and BrandName. Each of these tables is related through foreign key relationships, creating a snowflake-like structure.

![data-modeling-snowflake](https://github.com/atadas10/Learn-Data-Modeling/assets/84840069/f26cc82b-1553-48da-ae76-7f88a6b88250)

#
## Next Read:
  1. [Introduction to data modeling](README.md)
  2. [Star schema design example](FoodDeliveryApplicationSchema.md)
  3. [Data Vault 2.0](data-vault.md)


##
> © 2024 [atanuconsulting.in](https://www.atanuconsulting.in "Atanu Consulting")  | [LinkedIn](https://www.linkedin.com/in/dasatanu10 "LinkedIn Page") | [email](mailto:atanu10.yt@gmail.com "Send mail")
