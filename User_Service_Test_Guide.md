# User Service API Testing Guide

This guide provides step-by-step instructions to test the **User Service** endpoints using Postman or cURL.

**Base URL:** `http://localhost:8080/api/users` (via API Gateway)

---

## 1. Register a New User

**Endpoint:** `POST /register`
**Description:** Creates a new user account.

**Request Body (JSON):**
```json
{
  "email": "testuser@example.com",
  "password": "password123",
  "firstName": "Test",
  "lastName": "User",
  "phone": "1234567890",
  "role": "CUSTOMER"
}
```

**cURL Command:**
```bash
curl -X POST http://localhost:8080/api/users/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "testuser@example.com",
    "password": "password123",
    "firstName": "Test",
    "lastName": "User",
    "phone": "1234567890",
    "role": "CUSTOMER"
  }'
```

**Expected Response (201 Created):**
```json
{
  "token": "eyJhbGciOiJIUzI1NiJ9...",
  "email": "testuser@example.com",
  "role": "CUSTOMER",
  "userId": 1
}
```

---

## 2. Login

**Endpoint:** `POST /login`
**Description:** Authenticates a user and returns a JWT token.

**Request Body (JSON):**
```json
{
  "email": "testuser@example.com",
  "password": "password123"
}
```

**cURL Command:**
```bash
curl -X POST http://localhost:8080/api/users/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "testuser@example.com",
    "password": "password123"
  }'
```

**Expected Response (200 OK):**
```json
{
  "token": "eyJhbGciOiJIUzI1NiJ9...",
  "email": "testuser@example.com",
  "role": "CUSTOMER",
  "userId": 1
}
```

> [!IMPORTANT]
> **Save the `token` from the response!** You will need it for the next requests.

---

## 3. Get User Profile

**Endpoint:** `GET /profile`
**Description:** Retrieves the profile of the currently authenticated user.
**Auth:** Requires Bearer Token.

**How to set Token in Postman:**
1. Go to the **Authorization** tab.
2. Select **Bearer Token** from the Type dropdown.
3. Paste the token (from the Login response) into the **Token** field.

**cURL Command:**
```bash
# Replace YOUR_TOKEN_HERE with the token from login/register
curl -X GET http://localhost:8080/api/users/profile \
  -H "Authorization: Bearer YOUR_TOKEN_HERE"
```

**Expected Response (200 OK):**
```json
{
  "id": 1,
  "email": "testuser@example.com",
  "firstName": "Test",
  "lastName": "User",
  "phone": "1234567890",
  "role": "CUSTOMER"
}
```

---

## 4. Update User Profile

**Endpoint:** `PUT /profile`
**Description:** Updates the profile of the currently authenticated user.
**Auth:** Requires Bearer Token.

**How to set Token in Postman:**
1. Go to the **Authorization** tab.
2. Select **Bearer Token** from the Type dropdown.
3. Paste the token (from the Login response) into the **Token** field.

**Request Body (JSON):**
```json
{
  "firstName": "UpdatedName",
  "lastName": "UpdatedLast",
  "phone": "0987654321"
}
```

**cURL Command:**
```bash
# Replace YOUR_TOKEN_HERE with the token from login/register
curl -X PUT http://localhost:8080/api/users/profile \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -H "Content-Type: application/json" \
  -d '{
    "firstName": "UpdatedName",
    "lastName": "UpdatedLast",
    "phone": "0987654321"
  }'
```

**Expected Response (200 OK):**
```json
{
  "id": 1,
  "email": "testuser@example.com",
  "firstName": "UpdatedName",
  "lastName": "UpdatedLast",
  "phone": "0987654321",
  "role": "CUSTOMER"
}
```

---

## 5. Health Check

**Endpoint:** `GET /health`
**Description:** Checks if the service is running.

**cURL Command:**
```bash
curl -X GET http://localhost:8080/api/users/health
```

**Expected Response (200 OK):**
```text
User Service is running
```
