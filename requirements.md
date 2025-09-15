# Backend Requirements Specification

This document outlines the technical and functional requirements for core backend features of the Airbnb Clone project.

---

## 1. User Authentication

### Functional Requirements
- Users must be able to register, log in, and log out securely.
- Authentication should be implemented using JWT (JSON Web Tokens).
- Passwords must be hashed and never stored in plain text.
- Only authenticated users can access protected endpoints.

### API Endpoints
- `POST /api/auth/register` → Register a new user.
- `POST /api/auth/login` → Log in and receive a JWT token.
- `POST /api/auth/logout` → Invalidate user session.
- `GET /api/auth/profile` → Retrieve current user profile (protected).

### Input/Output
- **Register Input:** `{ "username": "string", "email": "string", "password": "string" }`
- **Register Output:** `{ "id": "uuid", "username": "string", "email": "string", "created_at": "timestamp" }`

- **Login Input:** `{ "email": "string", "password": "string" }`
- **Login Output:** `{ "token": "jwt-token", "user": { "id": "uuid", "username": "string" } }`

### Validation Rules
- Email must be unique and valid.
- Password must be at least 8 characters long.
- Username must not contain special characters.

### Performance Criteria
- Authentication requests must complete in < 200ms under normal load.
- Token validation should not exceed 50ms.

---

## 2. Property Management

### Functional Requirements
- Hosts can create, update, and delete property listings.
- Users can view property details and search with filters.
- Properties must be linked to the host (owner).

### API Endpoints
- `POST /api/properties` → Create new property (host only).
- `GET /api/properties` → Retrieve all properties (with optional filters).
- `GET /api/properties/{id}` → Retrieve a specific property by ID.
- `PUT /api/properties/{id}` → Update property details (host only).
- `DELETE /api/properties/{id}` → Remove property (host only).

### Input/Output
- **Create Input:**  
  ```json
  {
    "title": "string",
    "description": "string",
    "price_per_night": "number",
    "location": "string",
    "amenities": ["string"]
  }

## 3. Booking System
### Functional Requirements

- Users can book available properties for a date range.
- Bookings must check availability before confirmation.
- Users can view and cancel their bookings.
- Bookings must integrate with the Payments module.

### API Endpoints

- `POST /api/bookings` → Create a new booking.
- `GET /api/bookings` → Retrieve all bookings for the current user.
- `GET /api/bookings/{id}` → Retrieve booking details.
- `DELETE /api/bookings/{id}` → Cancel a booking.

### Input/Output
- **Booking Input**:
```json
{
  "property_id": "uuid",
  "start_date": "YYYY-MM-DD",
  "end_date": "YYYY-MM-DD"
}

- **Booking Output**
```json
{
  "id": "uuid",
  "property_id": "uuid",
  "user_id": "uuid",
  "start_date": "YYYY-MM-DD",
  "end_date": "YYYY-MM-DD",
  "status": "confirmed|canceled"
}

### Validation Rules

- Booking dates must not overlap with existing confirmed bookings.
- End date must be after start date.
- Users cannot book their own property.
- Performance Criteria
- Availability checks must complete in < 300ms.
- The system must handle at least 100 concurrent booking requests without data conflicts.