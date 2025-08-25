# Airbnb Clone Backend – Features & Functionalities

This document lists the key features and functionalities of the Airbnb Clone backend.  
The purpose is to provide a clear blueprint for development, ensuring all critical capabilities are accounted for.

## 1. User Management

- **User Registration & Login**
  - Email/password authentication
  - Password hashing with bcrypt
  - JWT-based authentication for secure sessions
- **User Roles**
  - Guest, Host, Admin
  - Role-based access control (RBAC)
- **Profile Management**
  - View and update personal details
  - Password reset and account recovery

## 2. Property Management

- **Create, Read, Update, Delete (CRUD) Properties**
  - Hosts can add property listings
  - Include details: name, description, location, price per night, images, max guests
- **Property Search**
  - Filter by location, price, availability
  - Sort results by price, rating, or popularity

## 3. Booking System

- **Create and Manage Bookings**
  - Guests can book properties
  - Hosts can view bookings for their properties
- **Booking Status**
  - Pending, Confirmed, Canceled
- **Availability Checks**
  - Ensure no double-booking
  - Date validation and total price calculation

## 4. Payment Processing

- **Payment Integration**
  - Stripe, PayPal
- **Transactions**
  - Create, read, and track payments
  - Support multiple payment methods
- **Refunds**
  - Handle canceled bookings and partial/full refunds

## 5. Reviews and Ratings

- Guests can leave ratings (1-5) and comments for properties they booked
- Hosts can respond to reviews
- Aggregate property ratings for search and display

## 6. Messaging System

- Private messaging between users
- Send and receive messages
- Timestamped conversation history

## 7. Administrative Features

- Admin can manage users, properties, bookings, and payments
- Access audit logs
- Generate reports for system usage and revenue

## 8. Documentation & Maintenance

- API documentation for all endpoints
- Clear error handling and validation messages
- Logging for debugging and monitoring

## 📂 Diagram

A detailed diagram of these features and their interactions is provided below:

![Airbnb Clone Features](features.png)
