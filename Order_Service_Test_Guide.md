# Order Service API Testing Guide

This guide provides step-by-step instructions to test the **Order Service** endpoints.

**Base URL:** `http://localhost:8080/api/orders` (via API Gateway)

> [!NOTE]
> The **Order Service is currently unsecured**. You do not need an Authorization token to access these endpoints.
> However, placing an order typically requires a valid `userId` and `productIds` that exist in the system.

---

## 1. Create Order

**Endpoint:** `POST /orders`
**Description:** Places a new order.
**Note:** This usually triggers inventory reduction and cart clearing if implemented.

**Request Body (JSON):**
```json
{
  "userId": 1,
  "totalAmount": 1999.98,
  "shippingAddress": "123 Main St, New York, NY",
  "items": [
    {
      "productId": 1,
      "quantity": 2,
      "price": 999.99
    }
  ]
}
```

**cURL Command:**
```bash
curl -X POST http://localhost:8080/api/orders \
  -H "Content-Type: application/json" \
  -d '{
    "userId": 1,
    "totalAmount": 1999.98,
    "shippingAddress": "123 Main St, New York, NY",
    "items": [
      {
        "productId": 1,
        "quantity": 2,
        "price": 999.99
      }
    ]
  }'
```

**Expected Response (200 OK):**
```json
{
  "id": 1,
  "userId": 1,
  "status": "PENDING",
  "totalAmount": 1999.98,
  "items": [ ... ],
  ...
}
```

---

## 2. Get User Orders

**Endpoint:** `GET /orders/user/{userId}`
**Description:** Retrieves all orders placed by a specific user.

**cURL Command:**
```bash
curl -X GET http://localhost:8080/api/orders/user/1
```

---

## 3. Get All Orders (Admin)

**Endpoint:** `GET /orders`
**Description:** Retrieves all orders in the system.

**cURL Command:**
```bash
curl -X GET http://localhost:8080/api/orders
```

---

## 4. Get Order by ID

**Endpoint:** `GET /orders/{id}`
**Description:** Retrieves details of a specific order.

**cURL Command:**
```bash
curl -X GET http://localhost:8080/api/orders/1
```

---

## 5. Update Order Status

**Endpoint:** `PUT /orders/{id}/status`
**Description:** Updates the status of an order (e.g., PENDING -> SHIPPED).
**Query Param:** `status`

**cURL Command:**
```bash
curl -X PUT "http://localhost:8080/api/orders/1/status?status=SHIPPED"
```

**Expected Response (200 OK):**
```json
{
  "id": 1,
  "status": "SHIPPED",
  ...
}
```

---

## 6. Health Check

**Endpoint:** `GET /orders/health`
**Description:** Checks if the service is running.

**cURL Command:**
```bash
curl -X GET http://localhost:8080/api/orders/health
```

**Expected Response (200 OK):**
```text
Order Service is running
```
