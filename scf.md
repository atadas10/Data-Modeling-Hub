# Slowly Changing Fact (SCF)

In the world of data warehousing and dimensional modeling, particularly in the Kimball approach, the concept of **Slowly Changing Fact (SCF)** plays a crucial role. SCFs help track fact table rows that change gradually over time, such as product prices, commission rates, or employee salaries.

### 🔑 Key Differences: SCF vs. SCD
- **SCF**: Focuses on fact tables and measures that change slowly.
- **SCD**: Focuses on dimension tables and attributes that change over time (e.g., customer addresses).

### 🛒 Use Case: E-commerce Example
Imagine an e-commerce platform where order statuses and related fields (e.g., shipping address, delivery date) evolve over time. SCFs allow us to track these changes for accurate historical analysis. For instance:

| OrderID | OrderStatus | ShippingAddress      | EstimatedDeliveryDate | EffectiveDate | Active_Flag |
|---------|-------------|----------------------|-----------------------|---------------|-------------|
| 201     | Placed      | 456 Elm St, NY       | 2023-10-15            | 2023-10-01    | False       |
| 201     | Placed      | 123 Main St, NY      | 2023-10-20            | 2023-10-03    | False       |
| 201     | Shipped     | 123 Main St, NY      | 2023-10-20            | 2023-10-10    | False       |
| 201     | Delivered   | 123 Main St, NY      | 2023-10-20            | 2023-10-20    | True        |

In this case, the order status for `OrderID 201` evolves from `Placed` to `Shipped` and finally to `Delivered`. Each change is recorded as a new row in the fact table. The `EffectiveDate` column indicates when the change occurred, and the `Active_Flag` column identifies the current state of the order. For example:
- On `2023-10-01`, the order was placed with the shipping address `456 Elm St, NY`.
- On `2023-10-03`, the shipping address was updated to `123 Main St, NY`, while the status remained `Placed`. This impacted the estimated delivery date, which changed to `2023-10-23`.
- On `2023-10-10`, the order status changed to `Shipped`.
- On `2023-10-20`, the order was marked as `Delivered`, and this row is marked as active (`Active_Flag = True`).

This approach ensures all historical changes are preserved for analyzing order trends and lifecycle.

### ⚠️ Limitations of SCF
1. Increased storage requirements.
2. Complex queries due to multiple rows for the same entity.
3. Performance overhead during ETL processes.

### ✅ Best Practices for SCF
1. Use effective start and end dates to track row validity.
2. Partition fact tables by date for better performance.
3. Archive older rows to manage storage.
4. Document SCF implementation for team clarity.

By implementing SCFs effectively, organizations can unlock valuable insights into trends and changes over time while maintaining historical accuracy. 💡
