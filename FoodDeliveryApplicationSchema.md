# Food Delivery Application Data Model

## 1. Use Case

The data model is designed for a food delivery application, facilitating the efficient management of orders, delivery agents, customers, incentives, restaurants, menus, and payment methods.

### Functionality Overview

#### 1. Incentive Management
   - **Feature:** Provide incentives for delivery agents based on order count and effective dates.

#### 2. Driver Performance Tracking
   - **Feature:** Track and analyze the performance of delivery agents, including their current location, status, and delivery count.

#### 3. Customer Information
   - **Feature:** Store detailed customer information for profiling and targeted promotions.

#### 4. Restaurant and Menu Management
   - **Feature:** Manage restaurant details and menus, ensuring menu items are associated with their respective restaurants.

#### 5. Payment Method and Discounts
   - **Feature:** Centralize information about payment methods and associated discounts, supporting flexible payment options and discount strategies.

#### 6. Order Tracking and Status
   - **Feature:** Capture essential information about orders, including timestamps for order placement, pickup, and delivery. Track order status.

#### 7. Timestamps and Durations
   - **Feature:** Enable analysis of order processing times through timestamps and duration information.

#### 8. Rating System
   - **Feature:** Implement a rating system for agents and restaurants, facilitating feedback.

### 2. Schema Details
### `dim_agent`

| Column Name      | Data Type | Description                                |
| ---------------- | --------- | ------------------------------------------ |
| agent_id         | number    | Primary Key                                |
| name             | varchar   | Agent's name                               |
| address_1        | varchar   | Address line 1                             |
| address_2        | varchar   | Address line 2                             |
| city             | varchar   | City                                       |
| state            | varchar   | State                                      |
| pincode          | varchar   | Pincode                                    |
| base_location    | varchar   | Base location of the agent                 |
| rating           | decimal   | Rating of the agent                        |

### `dim_delivery_agent`

| Column Name      | Data Type | Description                                |
| ---------------- | --------- | ------------------------------------------ |
| delivery_agent_id| number    | Primary Key                                |
| agent_id         | number    | Foreign Key referencing dim_agent.agent_id |
| Name             | varchar   | Delivery agent's name                      |
| Current_location | varchar   | Current location of the delivery agent     |
| current_status   | varchar   | Current status of the delivery agent       |
| delivery_count   | number    | Count of deliveries completed by the agent |

### `dim_incentive`

| Column Name      | Data Type | Description                                |
| ---------------- | --------- | ------------------------------------------ |
| incentive_id     | number    | Primary Key                                |
| order_count      | number    | Count of orders required for the incentive |
| amount           | number    | Incentive amount                           |
| effective_date   | date      | Date when the incentive becomes effective |

### `fact_agent_incentive`

| Column Name      | Data Type | Description                                |
| ---------------- | --------- | ------------------------------------------ |
| agent_incentive  | number    | Primary Key                                |
| agent_id         | number    | Foreign Key referencing dim_agent.agent_id |
| incentive_id     | number    | Foreign Key referencing dim_incentive.incentive_id |
| delivery_date    | date      | Date of the delivery for incentive calculation |

### `dim_customer`

| Column Name      | Data Type | Description                                |
| ---------------- | --------- | ------------------------------------------ |
| customer_id      | number    | Primary Key                                |
| name             | varchar   | Customer's name                            |
| address_1        | varchar   | Address line 1                             |
| address_2        | varchar   | Address line 2                             |
| city             | varchar   | City                                       |
| state            | varchar   | State                                      |
| pincode          | varchar   | Pincode                                    |
| AverageOrderValue| number    | Average value of customer's orders         |

### `dim_restaurant`

| Column Name      | Data Type | Description                                |
| ---------------- | --------- | ------------------------------------------ |
| restaurant_id    | number    | Primary Key                                |
| name             | varchar   | Restaurant's name                          |
| location         | varchar   | Location of the restaurant                 |
| contact_information | varchar | Contact information for the restaurant    |
| rating           | decimal   | Rating of the restaurant                   |

### `dim_menu`

| Column Name      | Data Type | Description                                |
| ---------------- | --------- | ------------------------------------------ |
| menu_id          | number    | Primary Key                                |
| restaurant_id    | number    | Foreign Key referencing dim_restaurant.restaurant_id |
| item_name        | varchar   | Name of the menu item                      |
| price            | number    | Price of the menu item                     |
| description      | varchar   | Description of the menu item               |

### `payment_mode`

| Column Name      | Data Type | Description                                |
| ---------------- | --------- | ------------------------------------------ |
| payment_mode_id  | number    | Payment mode identifier                    |
| payment_method   | varchar   | Payment method (e.g., card, cash, UPI)     |
| discount         | decimal   | Discount associated with the payment method|

### `fact_order_delivery`

| Column Name      | Data Type | Description                                |
| ---------------- | --------- | ------------------------------------------ |
| order_id         | number    | Primary Key                                |
| customer_id      | number    | Foreign Key referencing dim_customer.customer_id |
| agent_id         | number    | Foreign Key referencing dim_agent.agent_id |
| restaurant_id    | number    | Foreign Key referencing dim_restaurant.restaurant_id |
| menu_id          | number    | Foreign Key referencing dim_menu.menu_id   |
| payment_mode_id  | number    | Foreign Key referencing payment_mode.payment_mode_id |
| order_time       | datetime  | Timestamp of order placement               |
| pickup_time      | datetime  | Timestamp of order pickup                  |
| delivery_time    | datetime  | Timestamp of order delivery                |
| order_status     | varchar   | Status of the order (e.g., order, in transit, delivered) |
| duration         | number    | Duration of the order completion (in minutes) |
| timestamp        | datetime  | Overall timestamp for record tracking     |

## 3. Relationships

![image](https://github.com/atadas10/DataModelling/assets/84840069/cc717c00-94b1-47e0-9caf-7432f40e1299)

## 4. Implementation Details

The functionalities are achieved through the following components of the data model:

### Incentive Management
- **Tables:** `dim_incentive`, `dim_agent_incentive`
- **Details:** Incentives are stored in the `dim_incentive` table, and the `dim_agent_incentive` table associates incentives with delivery agents based on order count and effective dates.

### Driver Performance Tracking
- **Tables:** `dim_delivery_agent`
- **Details:** The `dim_delivery_agent` table contains information about delivery agents, including their current location, status, and delivery count.

### Customer Information
- **Table:** `dim_customer`
- **Details:** Customer details, including name, address, and average order value, are stored in the `dim_customer` table.

### Restaurant and Menu Management
- **Tables:** `dim_restaurant`, `dim_menu`
- **Details:** Restaurant details are stored in `dim_restaurant`, and the relationship with `dim_menu` ensures that menu items are associated with their respective restaurants.

### Payment Method and Discounts
- **Table:** `payment_mode`
- **Details:** Information about payment methods and associated discounts is centralized in the `payment_mode` table.

### Order Tracking and Status
- **Table:** `fact_order_delivery`
- **Details:** The `fact_order_delivery` table captures essential order information, including timestamps for order placement, pickup, and delivery. The `order_status` column tracks the status of each order.

### Timestamps and Durations
- **Table:** `fact_order_delivery`
- **Details:** The `fact_order_delivery` table includes timestamps for order-related events and a `duration` column for analyzing order processing times.

### Rating System
- **Tables:** `dim_agent`, `dim_restaurant`
- **Details:** Rating information for agents and restaurants is stored in the `dim_agent` and `dim_restaurant` tables.

## 5. Conclusion

The designed data model provides a comprehensive structure for managing various aspects of a food delivery application. It incorporates detailed information about agents, customers, incentives, restaurants, menus, and payment methods. The separation of delivery-specific details in `dim_delivery_agent` and the inclusion of incentive-related tables enhance the model's flexibility and analytical capabilities. The foreign key relationships ensure data integrity and support efficient querying and analysis.
