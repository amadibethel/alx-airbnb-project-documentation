# Backend Requirement Specifications

This document defines the technical and functional requirements for the backend features of the Airbnb Clone project.  
The focus is on four core modules: **User Authentication**, **Property Management**, **Booking System**, and **Payment Processing**.

---

## 1. User Authentication

### Functional Requirements

- Users should be able to **register** with their email, password, and name.
- Users should be able to **log in** using their email and password.
- Authentication should be managed using **JWT tokens**.
- Passwords must be securely hashed before storage.

### API Endpoints

- **POST /api/auth/register**
  - **Input**:  
    ```json
    {
      "name": "John Doe",
      "email": "john@example.com",
      "password": "securePassword123"
    }
    ```
  - **Validation**:
    - `name`: required, string, 3–50 chars
    - `email`: required, valid email format, unique
    - `password`: required, min length 8, must contain at least one number & letter
  - **Output (201)**:  
    ```json
    {
      "message": "User registered successfully",
      "userId": "12345"
    }
    ```
  - **Errors**:  
    - `400 Bad Request` if validation fails  
    - `409 Conflict` if email already exists  

- **POST /api/auth/login**
  - **Input**:  
    ```json
    {
      "email": "john@example.com",
      "password": "securePassword123"
    }
    ```
  - **Validation**:
    - `email`: required, must exist in DB
    - `password`: required, must match hashed password
  - **Output (200)**:  
    ```json
    {
      "token": "jwt.token.string",
      "user": {
        "id": "12345",
        "name": "John Doe",
        "email": "john@example.com"
      }
    }
    ```
  - **Errors**:  
    - `401 Unauthorized` if credentials invalid  

### Performance Criteria

- Login and registration requests should complete in < 300ms under normal load.
- Token expiration should be configurable (default: 1h).

---

## 2. Property Management

### Functional Requirements

- Authenticated users (hosts) can create, update, delete, and view property listings.
- Properties include attributes: title, description, price, location, amenities, images.

### API Endpoints

- **POST /api/properties**
  - **Input**:  
    ```json
    {
      "title": "Modern Apartment in Cape Town",
      "description": "A beautiful sea-facing apartment",
      "price": 150,
      "location": "Cape Town, South Africa",
      "amenities": ["WiFi", "Air Conditioning"],
      "images": ["img1.jpg", "img2.jpg"]
    }
    ```
  - **Validation**:
    - `title`: required, string, max 100 chars
    - `price`: required, positive number
    - `location`: required, string
  - **Output (201)**:  
    ```json
    {
      "message": "Property created successfully",
      "propertyId": "67890"
    }
    ```
  - **Errors**:  
    - `400 Bad Request` for invalid data  
    - `401 Unauthorized` if not logged in  

- **GET /api/properties/:id**
  - **Output (200)**:  
    ```json
    {
      "id": "67890",
      "title": "Modern Apartment in Cape Town",
      "price": 150,
      "location": "Cape Town, South Africa",
      "amenities": ["WiFi", "Air Conditioning"],
      "images": ["img1.jpg", "img2.jpg"],
      "hostId": "12345"
    }
    ```

- **PUT /api/properties/:id**
  - Updates property fields (only owner can update).  

- **DELETE /api/properties/:id**
  - Deletes property (only owner can delete).  

### Performance Criteria

- Retrieval of property details should complete in < 200ms.
- Properties should be paginated when listing (default: 10 per page).

---

## 3. Booking System

### Functional Requirements

- Users can book available properties for a given date range.
- System must prevent overlapping bookings.
- Booking confirmation should be sent to both guest and host.

### API Endpoints

- **POST /api/bookings**
  - **Input**:  
    ```json
    {
      "propertyId": "67890",
      "startDate": "2025-09-10",
      "endDate": "2025-09-15"
    }
    ```
  - **Validation**:
    - `propertyId`: must exist
    - `startDate` & `endDate`: valid ISO date format, endDate > startDate
    - Check property availability before confirming
  - **Output (201)**:  
    ```json
    {
      "message": "Booking confirmed",
      "bookingId": "98765",
      "totalPrice": 750
    }
    ```
  - **Errors**:  
    - `400 Bad Request` if validation fails  
    - `409 Conflict` if property already booked  

- **GET /api/bookings/:id**
  - Returns booking details.  

- **DELETE /api/bookings/:id**
  - Cancels booking if within cancellation window.

### Performance Criteria

- Booking conflict check must run in < 100ms.
- System must handle at least 100 concurrent booking requests reliably.

---

## 4. Payment Processing

### Functional Requirements

- Users can securely pay for bookings using third-party payment providers (Stripe preferred).
- System should handle transactions, refunds, and payment confirmation.
- Sensitive payment information should **never** be stored on the server (use Stripe tokens).

### API Endpoints

- **POST /api/payments/checkout**
  - **Input**:  
    ```json
    {
      "bookingId": "98765",
      "paymentMethodId": "pm_card_visa"
    }
    ```
  - **Validation**:
    - `bookingId`: must exist and be valid
    - `paymentMethodId`: required, valid Stripe payment method
  - **Output (201)**:  
    ```json
    {
      "message": "Payment successful",
      "paymentId": "pay_123abc",
      "status": "succeeded"
    }
    ```
  - **Errors**:  
    - `400 Bad Request` if validation fails  
    - `402 Payment Required` if transaction fails  

- **POST /api/payments/refund**
  - **Input**:  
    ```json
    {
      "paymentId": "pay_123abc",
      "amount": 150
    }
    ```
  - **Validation**:
    - `paymentId`: must exist
    - `amount`: must not exceed original payment
  - **Output (200)**:  
    ```json
    {
      "message": "Refund initiated",
      "refundId": "re_456def",
      "status": "pending"
    }
    ```

- **GET /api/payments/:id**
  - Returns payment details (status, amount, booking reference).

### Performance Criteria

- Checkout request must complete in < 500ms (excluding Stripe latency).
- Retry mechanism must handle transient payment failures.
- System must comply with **PCI DSS** by offloading sensitive data handling to Stripe.

---

## General Notes

- All endpoints should return **JSON responses**.
- Use standard HTTP status codes.
- API should follow REST conventions.
- Authentication required for protected routes via JWT in `Authorization` header.

---
