# Hotel Reservation Application Data Model

## 1. Use Case:
  The hotel reservation data model facilitates hotel management, reservation tracking, and customer information storage.

  ### Functionality Overview

   - **User Registration & Login:** Allow users to create accounts and securely log in.
   - **Search & Filter:** Enable users to search for hotels based on location, dates, price range, amenities, etc.
   - **Booking Management:** Allow users to view, modify, and cancel their reservations.
   - **Room Availability:** Display real-time room availability and allow users to book available rooms.
   - **Reviews & Ratings:** Let users view and submit reviews and ratings for hotels and rooms.
   - **Payment Integration:** Integrate payment gateways for secure booking transactions.
   - **Notifications:** Send notifications to users for booking confirmations, reminders, and updates.
   - **Customer Support:** Provide easy access to customer support for inquiries and assistance.
   - **User Profile:** Allow users to manage their profiles, preferences, and past bookings.
   - **Deals & Discounts:** Offer special deals, discounts, and loyalty programs to encourage bookings.

## 2. Schema Details
We are going to use Data-Vault 2.0 approach to build the solution here. First, let us identify the entities below as below:
- User
- Hotel
- Room
- Reservation
- Review
- Payment
- Notification
- Customer Support

### 2.1. Conceptual Model:
  Next steps is to build the conceptual model. In data Valut 2.0, the building blocks are **Hubs**, **Links** and   **Satellite**.

  - **Hubs**:Hubs represent the core business entities.
    - User Hub
    - Hotel Hub
    - Room Hub
    - Payment Hub
    - Customer Support Hub
      
  - **Links**
    - Reservation Link (connects User Hub, Hotel Hub, Room Hub, and Payment Hub)
    - Review Link (connects User Hub, Hotel Hub, and Room Hub)

  - **Satellite**
    - User Satellite (contains user details)
    - Hotel Satellite (contains hotel details)
    - Room Satellite (contains room details)
    - Reservation Satellite (contains reservation details)
    - Review Satellite (contains review details)
    - Payment Satellite (contains payment details)
    - Notification Satellite (contains notification details)
    - Customer Support Satellite (contains customer support details)
      
  For example, let's take the user & hotel hub and link them through UserHotelLink table. Here's a textual representation of the conceptual data model:

                       +---------------------+
                       |      User Hub       |
                       +---------------------+
                       | user_id (PK)        |
                       | username            |
                       | email               |
                       | password            |
                       +---------------------+
                                 |
                                 |
           +--------------+------+-----------------------+
           |                     |                       |
    +----------------+  +------------------+      +---------------------+
    |  User Details  |  |  UserHotelLink   |      |      Hotel Hub      |
    +----------------+  +------------------+      +---------------------+
    | user_id (FK)   |  | user_id (FK)     |      | hotel_id (PK)       |
    | first_name     |  | hotel_id (FK)    |      | name                |
    | last_name      |  +------------------+      | description         |
    | address        |                            +---------------------+
    | phone_number   | 
    +----------------+              

Final steps is to define all the entities in detail and create the relationship between them. Let's start with the process.

### 2.2 Table Details

  #### 2.2.1 User Hub
  - Description: Represents users of the hotel reservation app.
    
    | Name      | Data Type | Description  |
    |-----------|-----------|--------------|
    | user_id   | Primary Key| Unique identifier for the user. |
    | username  | Text      | Username of the user. |
    | email     | Text      | Email address of the user. |
    | password  | Text      | Encrypted password of the user. |
  
  #### 2.2.2 User Details Satellite
  - Description: Contains additional details about users.
  
  | Name          | Data Type | Description  |
  |---------------|-----------|--------------|
  | user_id       | Foreign Key| References the user in the User Hub. |
  | first_name    | Text      | First name of the user. |
  | last_name     | Text      | Last name of the user. |
  | address       | Text      | Address of the user. |
  | phone_number  | Text      | Phone number of the user. |
  
  #### 2.2.3 Hotel Hub
  - Description: Represents hotels available for reservation.
  
  | Name       | Data Type | Description  |
  |------------|-----------|--------------|
  | hotel_id   | Primary Key| Unique identifier for the hotel. |
  | name       | Text      | Name of the hotel. |
  | location   | Text      | Location of the hotel. |
  | rating     | Numeric   | Rating of the hotel. |
  
  #### 2.2.4 Hotel Details Satellite
  - Description: Contains additional details about hotels.
  
  | Name          | Data Type | Description  |
  |---------------|-----------|--------------|
  | hotel_id      | Foreign Key| References the hotel in the Hotel Hub. |
  | description   | Text      | Description of the hotel. |
  | amenities     | Text      | Amenities offered by the hotel. |
  | room_types    | Text      | Types of rooms available in the hotel. |
  
  #### 2.2.5 Room Hub
  - Description: Represents rooms available for reservation.
  
  | Name       | Data Type | Description  |
  |------------|-----------|--------------|
  | room_id    | Primary Key| Unique identifier for the room. |
  | type       | Text      | Type of the room. |
  | price      | Numeric   | Price of the room. |
  
  #### 2.2.6 Room Details Satellite
  - Description: Contains additional details about rooms.
  
  | Name          | Data Type | Description  |
  |---------------|-----------|--------------|
  | room_id       | Foreign Key| References the room in the Room Hub. |
  | capacity      | Integer   | Capacity of the room (number of guests). |
  | amenities     | Text      | Amenities available in the room. |
  
  #### 2.2.7 Payment Hub
  - Description: Represents payment transactions for reservations.
  
  | Name         | Data Type | Description  |
  |--------------|-----------|--------------|
  | payment_id   | Primary Key| Unique identifier for the payment. |
  | method       | Text      | Payment method used. |
  | status       | Text      | Status of the payment (e.g., paid, pending). |
  
  #### 2.2.8 Payment Details Satellite
  - Description: Contains additional details about payments.
  
  | Name          | Data Type | Description  |
  |---------------|-----------|--------------|
  | payment_id    | Foreign Key| References the payment in the Payment Hub. |
  | amount        | Numeric   | Amount of the payment. |
  | transaction_id| Text      | Transaction ID of the payment. |
  
  #### 2.2.9 Reservation Link
  - Description: Connects users, hotels, rooms, and payments for reservations.
  
  | Name          | Data Type | Description  |
  |---------------|-----------|--------------|
  | reservation_id| Primary Key| Unique identifier for the reservation. |
  | user_id       | Foreign Key| References the user in the User Hub. |
  | hotel_id      | Foreign Key| References the hotel in the Hotel Hub. |
  | room_id       | Foreign Key| References the room in the Room Hub. |
  | payment_id    | Foreign Key| References the payment in the Payment Hub. |
  | check_in_date | Date      | Date of check-in for the reservation. |
  | check_out_date| Date      | Date of check-out for the reservation. |
  | num_guests    | Integer   | Number of guests for the reservation. |
  
  #### 2.2.10 Review Link
  - Description: Connects users, hotels, and rooms for reviews.
  
  | Name          | Data Type | Description  |
  |---------------|-----------|--------------|
  | review_id     | Primary Key| Unique identifier for the review. |
  | user_id       | Foreign Key| References the user in the User Hub. |
  | hotel_id      | Foreign Key| References the hotel in the Hotel Hub. |
  | room_id       | Foreign Key| References the room in the Room Hub. |
  | rating        | Numeric   | Rating given by the user for the hotel/room. |
  | comment       | Text      | Comment provided by the user. |
  
  #### 2.2.11 Notification Satellite
  - Description: Contains details about notifications sent to users.
  
  | Name          | Data Type | Description  |
  |---------------|-----------|--------------|
  | notification_id| Primary Key| Unique identifier for the notification. |
  | user_id       | Foreign Key| References the user in the User Hub. |
  | message       | Text      | Content of the notification. |
  | timestamp     | DateTime  | Timestamp when the notification was sent. |
  
  #### 2.2.12 Customer Support Hub
  - Description: Represents customer support interactions.
  
  | Name          | Data Type | Description  |
  |---------------|-----------|--------------|
  | support_id    | Primary Key| Unique identifier for the support interaction. |
  | user_id       | Foreign Key| References the user in the User Hub. |
  | issue_type    | Text      | Type of support issue. |
  | status        | Text      | Status of the support interaction (e.g., open, closed). |
  | timestamp     | DateTime  | Timestamp of the support interaction. |
  
  #### 2.2.13 Customer Support Details Satellite
  - Description: Contains additional details about customer support interactions.
  
  | Name          | Data Type | Description  |
  |---------------|-----------|--------------|
  | support_id    | Foreign Key| References the support interaction in the Customer Support Hub. |
  | description   | Text      | Description of the support issue. |
  | resolution    | Text      | Resolution provided for the support issue. |
  | resolved_by   | Text      | User who resolved the support issue. |
  | timestamp     | DateTime  | Timestamp of the support interaction. |

# 3. Relationships

  ![Hotel Reservation](https://github.com/atadas10/Learn-Data-Modeling/assets/84840069/05870cbc-33ce-4cf6-b5f0-dac47739054d)

# 4. Implementation Details:

#### 4.1 User Registration & Login:
- **User Registration**: Information is inserted into the User Hub table upon registration.
- **User Login**: Credentials are verified against data in the User Hub table.

#### 4.2 Search & Filter:
- **Search**: Hotels are queried based on criteria from the Hotel Hub and Hotel Satellite tables.
- **Filter**: Additional filtering options are applied using SQL WHERE clauses.

#### 4.3 Booking Management:
- **View Reservations**: Query the Reservation Link table joined with the User Hub and Hotel Hub tables.
- **Modify Reservations**: Update reservation rows in the Reservation Satellite table.
- **Cancel Reservations**: Delete reservation rows from the Reservation Satellite table.

#### 4.4 Room Availability:
- **Real-time Availability**: Query the Room Hub and Reservation Satellite tables for room availability.

#### 4.5 Reviews & Ratings:
- **Submit Review**: Insert rows into the Review Satellite table, referencing relevant tables.
- **View Reviews**: Query the Review Satellite table joined with other relevant tables.

#### 4.6 Payment Integration:
- **Initiate Payment**: Insert payment details into the Payment Hub and Payment Satellite tables.
- **Transaction Processing**: Process payments and update status in the Payment Satellite table.

#### 4.7 Notifications:
- **Send Notifications**: Insert notification data into the Notification Satellite table.
- **Receive Notifications**: Query the Notification Satellite table for user notifications.

#### 4.8 Customer Support:
- **Submit Support Requests**: Insert support requests into the Customer Support Hub and Satellite tables.
- **Resolve Support Requests**: Update support request status and details in the Customer Support Satellite table.

# 5. Conclusion
  Functionalities in the hotel reservation app are achieved by interacting with the database tables using SQL queries and operations. This database-driven approach ensures efficient management of hotel reservations and related activities.

##
> © 2024 [atanuconsulting.in](www.atanuconsulting.in)  | [LinkedIn](www.linkedin.com/in/dasatanu10) | [email](mailto:atanu10.yt@gmail.com)
