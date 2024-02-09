# Food Delivery Application Data Model

## 1. Use Case:

The data model is designed for a food delivery application, facilitating the efficient management of orders, delivery agents, customers, incentives, restaurants, menus, and payment methods.

   ### Functionality Overview
   
   #### 1.1 Incentive Management
   - **Feature:** Provide incentives for delivery agents based on order count and effective dates.
   
   #### 1.2. Driver Performance Tracking
   - **Feature:** Track and analyze the performance of delivery agents, including their current location, status, and delivery count.
   
   #### 1.3. Customer Information
   - **Feature:** Store detailed customer information for profiling and targeted promotions.
   
   #### 1.4. Restaurant and Menu Management
   - **Feature:** Manage restaurant details and menus, ensuring menu items are associated with their respective restaurants.
   
   #### 1.5. Payment Method and Discounts
   - **Feature:** Centralize information about payment methods and associated discounts, supporting flexible payment options and discount strategies.
   
   #### 1.6. Order Tracking and Status
   - **Feature:** Capture essential information about orders, including timestamps for order placement, pickup, and delivery. Track order status.
   
   #### 1.7. Timestamps and Durations
   - **Feature:** Enable analysis of order processing times through timestamps and duration information.
   
   #### 1.8. Rating System
   - **Feature:** Implement a rating system for agents and restaurants, facilitating feedback.

## 2. Schema Details
We are going to use start-schema approach to build the solution here. First, let us understand the area of the models as below:
- agent
- customer
- order
- incentive, payment method etc

To build the conceptual model out of these subject areas, we have to identify the high level relationship between them.
For simplicity, we'll consider a one-to-many relationship between Customer and Order, and a one-to-many relationship between Agent and Order. Incentive and PaymentMethod will be independent entities.

Here's a textual representation of the conceptual data model:

           +---------------+
           |     Agent     |
           +---------------+
           | agent_id (PK) |
           | name          |
           | location      |
           +---------------+
                  |
                  |
           +------+----------------+
           |                       |
      +------------------+  +------------------+
      |     Customer     |  |       Order      |
      +------------------+  +------------------+
      | customer_id (PK) |  | order_id (PK)    |
      | name             |  | order_date       |
      | address          |  | amount           |
      | phone_number     |  | status           |
      +------------------+  | agent_id (FK)    |
                            | customer_id (FK) |
                            +------------------+

Second steps is to define the entities in detail. Let's start with the process.
 - ### 2.1 `dim_agent`
   Stores information about delivery agents, including their name, address, base location, and rating.

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

### 2.2 `dim_delivery_agent`
   Tracks the status and performance of delivery agents, including their current location, delivery count, and overall    status.
   | Column Name      | Data Type | Description                                |
   | ---------------- | --------- | ------------------------------------------ |
   | delivery_agent_id| number    | Primary Key                                |
   | agent_id         | number    | Foreign Key referencing dim_agent.agent_id |
   | Name             | varchar   | Delivery agent's name                      |
   | Current_location | varchar   | Current location of the delivery agent     |
   | current_status   | varchar   | Current status of the delivery agent       |
   | delivery_count   | number    | Count of deliveries completed by the agent |

### 2.3 `dim_incentive`
   Manages incentives for delivery agents, specifying the order count required and the incentive amount.
   
   | Column Name      | Data Type | Description                                |
   | ---------------- | --------- | ------------------------------------------ |
   | incentive_id     | number    | Primary Key                                |
   | order_count      | number    | Count of orders required for the incentive |
   | amount           | number    | Incentive amount                           |
   | effective_date   | date      | Date when the incentive becomes effective |

### 2.4 `fact_agent_incentive`
   Associates delivery agents with specific incentives based on their performance and delivery dates.

   | Column Name      | Data Type | Description                                |
   | ---------------- | --------- | ------------------------------------------ |
   | agent_incentive  | number    | Primary Key                                |
   | agent_id         | number    | Foreign Key referencing dim_agent.agent_id |
   | incentive_id     | number    | Foreign Key referencing dim_incentive.incentive_id |
   | delivery_date    | date      | Date of the delivery for incentive calculation |

### 2.5 `dim_customer`
   Contains details about customers, such as their name, address, and average order value.

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

### 2.6 `dim_restaurant`
   Stores information about restaurants, including their name, location, contact details, and rating.
   | Column Name      | Data Type | Description                                |
   | ---------------- | --------- | ------------------------------------------ |
   | restaurant_id    | number    | Primary Key                                |
   | name             | varchar   | Restaurant's name                          |
   | location         | varchar   | Location of the restaurant                 |
   | contact_information | varchar | Contact information for the restaurant    |
   | rating           | decimal   | Rating of the restaurant                   |

### 2.7 `dim_menu`
   Manages the menu items offered by restaurants, specifying the item name, price, and description.

   | Column Name      | Data Type | Description                                |
   | ---------------- | --------- | ------------------------------------------ |
   | menu_id          | number    | Primary Key                                |
   | restaurant_id    | number    | Foreign Key referencing dim_restaurant.restaurant_id |
   | item_name        | varchar   | Name of the menu item                      |
   | price            | number    | Price of the menu item                     |
   | description      | varchar   | Description of the menu item               |

### 2.8 `payment_mode`
   Centralizes information about payment methods and associated discounts.
   
   | Column Name      | Data Type | Description                                |
   | ---------------- | --------- | ------------------------------------------ |
   | payment_mode_id  | number    | Payment mode identifier                    |
   | payment_method   | varchar   | Payment method (e.g., card, cash, UPI)     |
   | discount         | decimal   | Discount associated with the payment method|

### 2.9 `fact_order_delivery`
   Captures essential details about orders, including customer, agent, restaurant, menu, payment, timestamps, order status, and duration.

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

   ![Food Delivery System](https://github.com/atadas10/Learn-Data-Modeling/assets/84840069/a49d9d9e-3392-4480-a44f-ddff4ddc5398)


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


##
> © 2024 [atanuconsulting.in](www.atanuconsulting.in)  | [LinkedIn](www.linkedin.com/in/dasatanu10) | [email](mailto:atanu10.yt@gmail.com)
