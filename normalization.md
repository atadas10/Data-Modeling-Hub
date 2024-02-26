# Introduction to Normalization

# Table of Contents
1. [What is Normalization?](#1-what-is-normalization)
2. [Purpose and Importance of Normalization](#2-purpose-and-importance-of-normalization)
3. [Principles of Normalization](#3-principles-of-normalization)
4. [Types of Normal Forms](#4-types-of-normal-forms)
5. [Process of Normalization](#5-process-of-normalization)
6. [Practical Considerations and Trade-offs](#6-practical-considerations-and-trade-offs)
7. [Best Practices and Guidelines](#7-best-practices-and-guidelines)
8. [Example Dataset Progression](#8-example-dataset-progression)
9. [Conclusion](#9-conclusion)


## 1. What is Normalization?
Normalization is a systematic approach to organizing data in a database. Its primary goal is to reduce redundancy and dependency, thereby improving data integrity and minimizing anomalies during data manipulation.

## 2. Purpose and Importance of Normalization
- **Data Integrity:** By eliminating redundancy and dependency, normalization reduces the risk of data inconsistencies and anomalies.
- **Efficient Storage:** Normalized databases typically occupy less storage space compared to denormalized ones, optimizing resource utilization.
- **Simplified Updates:** With well-structured tables, updates and modifications become more straightforward, minimizing the risk of data corruption.
- **Enhanced Query Performance:** Normalization can improve query performance by reducing the need for complex joins and facilitating efficient indexing strategies.

## 3. Principles of Normalization
Normalization adheres to several key principles:
- **Atomicity of Data:** Each column in a table contains atomic values, ensuring no multi-valued attributes.
- **Functional Dependency:** Non-key attributes are fully functionally dependent on the primary key.
- **Elimination of Anomalies:** Update, insertion, and deletion anomalies are minimized through proper normalization.
- **Reduction of Redundancy:** Data redundancy is minimized to improve storage efficiency.
- **Dependency Preservation:** Dependencies between attributes are preserved and represented accurately.

## 4. Types of Normal Forms
1. **First Normal Form (1NF):** Each column contains atomic values, and there are no repeating groups.
2. **Second Normal Form (2NF):** Achieves 1NF and ensures non-key attributes are fully dependent on the primary key.
3. **Third Normal Form (3NF):** Builds upon 2NF by eliminating transitive dependencies.
4. **Boyce-Codd Normal Form (BCNF):** Ensures every determinant is a candidate key.
5. **Fourth and Fifth Normal Forms:** Address multi-valued dependencies and join dependencies, respectively.

## 5. Process of Normalization
Normalization involves the following steps:
- Analyzing data for normalization.
- Decomposing tables to eliminate anomalies and dependencies.
- Achieving higher normal forms through successive decomposition.

## 6. Practical Considerations and Trade-offs
While normalization offers numerous benefits, it's essential to balance normalization with performance considerations. Over-normalization can lead to increased join operations and complexity, impacting query performance. Conversely, under-normalization may result in data redundancy and update anomalies.

## 7. Best Practices and Guidelines
- Design databases for normalization from the outset.
- Document the normalization process for future reference and maintenance.
- Revisit normalization as data requirements evolve to ensure continued efficiency and integrity.

## 8. Example Dataset Progression
Here's how an example dataset progresses through normalization:
- **Initial Table:** Contains unnormalized data.
- **First Normal Form (1NF):** Breaks down multi-valued attributes and removes repeating groups.
- **Second Normal Form (2NF):** Ensures non-key attributes are fully dependent on the primary key.
- **Third Normal Form (3NF):** Eliminates transitive dependencies.
- **Boyce-Codd Normal Form (BCNF):** Ensures every determinant is a candidate key.
- **Fourth Normal Form (4NF):** Addresses multi-valued dependencies.
- **Fifth Normal Form (5NF):** Addresses join dependencies.

The following dataset represents orders placed by customers, with each order containing information such as customer details, product details, quantity, and order date.
We can now gradually break down this dataset through the normalization process, starting with 1NF and progressing to higher normal forms. Let me know if you need further assistance with the normalization process!

- **Sample Dataset**
  
  | OrderID | CustomerID | CustomerName | CustomerCity | ProductID     | ProductName     | ProductCategory   | Quantity | OrderDate   |
  |---------|------------|--------------|--------------|---------------|-----------------|-------------------|----------|-------------|
  | 1       | 101        | John Doe     | New York     | [001, 002]    | [Laptop, Smartphone] | [Electronics, Electronics] | [2, 1]   | 2024-02-20  |
  | 2       | 102        | Jane Smith   | Los Angeles  | [002, 003]    | [Smartphone, Headphones] | [Electronics, Electronics] | [1, 3]   | 2024-02-21  |
  | 3       | 103        | Alice Brown  | Chicago      | [003]         | [Headphones]   | [Electronics]     | [3]      | 2024-02-21  |
  | 4       | 104        | Bob Johnson  | Houston      | [004]         | [Book]         | [Books]           | [5]      | 2024-02-22  |
  | 5       | 105        | Emily Davis  | Miami        | [002]         | [Smartphone]   | [Electronics]     | [1]      | 2024-02-22  |


<h3> 8.1. 1-NF: </h3>

To convert the dataset into the first normal form (1NF), we need to ensure that each attribute contains atomic values and there are no repeating groups. We can achieve this by separating multi-valued attributes into separate rows.

  | OrderID | CustomerID | CustomerName | ProductID | ProductName | ProductCategory | Quantity | OrderDate   |
  |---------|------------|--------------|-----------|-------------|-----------------|----------|-------------|
  | 1       | 101        | John Doe     | 001       | Laptop      | Electronics     | 2        | 2024-02-20  |
  | 1       | 101        | John Doe     | 002       | Smartphone  | Electronics     | 1        | 2024-02-20  |
  | 2       | 102        | Jane Smith   | 002       | Smartphone  | Electronics     | 1        | 2024-02-21  |
  | 2       | 102        | Jane Smith   | 003       | Headphones  | Electronics     | 3        | 2024-02-21  |
  | 3       | 103        | Alice Brown  | 003       | Headphones  | Electronics     | 3        | 2024-02-21  |
  | 4       | 104        | Bob Johnson  | 004       | Book        | Books           | 5        | 2024-02-22  |
  | 5       | 105        | Emily Davis  | 002       | Smartphone  | Electronics     | 1        | 2024-02-22  |

<h3> 8.2. 2-NF: </h3>

To convert the dataset into the second normal form (2NF), we need to ensure that non-key attributes are fully functionally dependent on the primary key. This means breaking down the table into smaller tables to remove partial dependencies.

First, let's identify the functional dependencies:
- OrderID -> CustomerID, OrderDate
- CustomerID -> CustomerName, CustomerCity
- ProductID -> ProductName, ProductCategory
We can split the table into three tables: Orders, Customers, and Products.

Here's the dataset in 2NF:

- **Orders Table:**

  | OrderID | CustomerID | OrderDate   |
  |---------|------------|-------------|
  | 1       | 101        | 2024-02-20  |
  | 2       | 102        | 2024-02-21  |
  | 3       | 103        | 2024-02-21  |
  | 4       | 104        | 2024-02-22  |
  | 5       | 105        | 2024-02-22  |

- **Customers Table:**

  | CustomerID | CustomerName | CustomerCity |
  |------------|--------------|--------------|
  | 101        | John Doe     | New York     |
  | 102        | Jane Smith   | Los Angeles  |
  | 103        | Alice Brown  | Chicago      |
  | 104        | Bob Johnson  | Houston      |
  | 105        | Emily Davis  | Miami        |


- **Products Table:**

  | ProductID | ProductName | ProductCategory |
  |-----------|-------------|-----------------|
  | 001       | Laptop      | Electronics     |
  | 002       | Smartphone  | Electronics     |
  | 003       | Headphones  | Electronics     |
  | 004       | Book        | Books           |

<h3> 8.3. 3-NF: </h3>

To convert the dataset into the third normal form (3NF), **we need to eliminate transitive dependencies**. This involves further decomposing the tables to ensure that non-key attributes do not depend on other non-key attributes within the same table.

In the provided customer dataset, the "CustomerCity" column is functionally dependent on CustomerID but not on the entire primary key (CustomerID). This introduces a transitive dependency into the dataset while keeping the tables in the second normal form (2NF).
To resolve this, we need to separate CustomerCity into its own table.
To break it down:
- Create a new table for Customer information, including CustomerID and CustomerName.
- Create another table for City information, including CityID and CustomerCity.
- Modify the Customers table to reference the CityID instead of directly storing the CustomerCity.

- **Customers Table**: 

  | CustomerID | CustomerName | CityID |
  |------------|--------------|--------|
  | 101        | John Doe     | 1      |
  | 102        | Jane Smith   | 2      |
  | 103        | Alice Brown  | 3      |
  | 104        | Bob Johnson  | 4      |
  | 105        | Emily Davis  | 5      |

- **Cities Table:**

  | CityID | CustomerCity |
  |--------|--------------|
  | 1      | New York     |
  | 2      | Los Angeles  |
  | 3      | Chicago      |
  | 4      | Houston      |
  | 5      | Miami        |


<h3> 8.4. BCNF: </h3>

To ensure that the tables are in Boyce-Codd Normal Form (BCNF), we need to make sure that every determinant (attribute determining another attribute) is a candidate key. In other words, every non-trivial functional dependency in the table must be determined by a candidate key.
In the Customers table, both CustomerID and CityID are candidate keys, as they uniquely identify each row. However, CustomerName is not a candidate key, but it determines CityID. This means that there is a functional dependency (CustomerName -> CityID) where the determinant (CustomerName) is not a superkey. 
To bring the table into BCNF, we need to split the table to eliminate this dependency.Here's how we can achieve BCNF:

- **Customers Table:**

    | CustomerID | CustomerName |
    |------------|--------------|
    | 101        | John Doe     |
    | 102        | Jane Smith   |
    | 103        | Alice Brown  |
    | 104        | Bob Johnson  |
    | 105        | Emily Davis  |

- **Cities Table:**

    | CityID | CustomerCity |
    |--------|--------------|
    | 1      | New York     |
    | 2      | Los Angeles  |
    | 3      | Chicago      |
    | 4      | Houston      |
    | 5      | Miami        |

  - **CustomerCities Table:**
 
    | CustomerID | CityID |
    |------------|--------|
    | 101        | 1      |
    | 102        | 2      |
    | 103        | 3      |
    | 104        | 4      |
    | 105        | 5      |


<h3> 8.5. 4-NF: </h3>

To achieve the Fourth Normal Form (4NF), we need to ensure that there are **no multi-valued dependencies in the tables**. This means that each non-prime attribute (attributes not part of any candidate key) should be fully functionally dependent on the primary key.
Let's consider a new requirement where a customer can have multiple email addresses associated with them. This introduces a multi-valued dependency between CustomerID and EmailAddress.
Here's how we can modify the tables to accommodate this scenario:

- **Customers Table:**

  | CustomerID | CustomerName |
  |------------|--------------|
  | 101        | John Doe     |
  | 102        | Jane Smith   |
  | 103        | Alice Brown  |
  | 104        | Bob Johnson  |
  | 105        | Emily Davis  |

- **Emails Table:**

  | CustomerID | EmailAddress     |
  |------------|------------------|
  | 101        | john@example.com |
  | 101        | johndoe@gmail.com |
  | 102        | jane@example.com |
  | 103        | alice@example.com |
  | 105        | emily@example.com |

In this structure, the Emails table has a composite primary key consisting of both CustomerID and EmailAddress, ensuring uniqueness. This removes the multi-valued dependency and brings the tables into the Fourth Normal Form (4NF).

<h3> 8.5. 5-NF: </h3>

To achieve the Fifth Normal Form (5NF), also known as Project-Join Normal Form (PJNF), we need to address join dependencies. Join dependencies occur when a table can be reconstructed by joining two or more other tables.
Let's introduce a new requirement where customers can leave reviews for products they have purchased. Each customer can leave reviews for multiple products, and each product can have reviews from multiple customers.

Here's how we can represent this scenario using 5NF:

- **Customers Table:**

  | CustomerID | CustomerName |
  |------------|--------------|
  | 101        | John Doe     |
  | 102        | Jane Smith   |
  | 103        | Alice Brown  |
  | 104        | Bob Johnson  |
  | 105        | Emily Davis  |

- **Products Table:**

  | ProductID | ProductName | ProductCategory |
  |-----------|-------------|-----------------|
  | 001       | Laptop      | Electronics     |
  | 002       | Smartphone  | Electronics     |
  | 003       | Headphones  | Electronics     |
  | 004       | Book        | Books           |

- **Reviews Table:**

  | CustomerID | ProductID | Rating | Comment        |
  |------------|-----------|--------|----------------|
  | 101        | 001       | 4.5    | Great laptop!  |
  | 102        | 002       | 5.0    | Love the phone |
  | 103        | 003       | 4.0    | Good sound     |
  | 104        | 004       | 4.5    | Interesting read |
  | 105        | 002       | 4.0    | Nice features  |

After all the normalization performed, the final data model looks like below:

![guest-template (1)](https://github.com/atadas10/Learn-Data-Modeling/assets/84840069/18bdf381-8a25-49fe-91d8-77f9dcb3b570)


## 9. Conclusion
Normalization is a fundamental principle in database design, fostering data integrity, efficiency, and scalability. By adhering to the normalization process and understanding the nuances of each normal form, database architects can craft robust and optimized data models that serve as the backbone of modern applications.

By embracing normalization, organizations can unlock the full potential of their data infrastructure, laying the groundwork for scalable and resilient systems.

