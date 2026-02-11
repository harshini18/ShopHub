# Cart Service API Testing Guide

This guide provides step-by-step instructions to test the **Cart Service** endpoints.

**Base URL:** `http://localhost:8080/api` (via API Gateway)

> [!NOTE]
> The **Cart Service is currently unsecured**. You do not need an Authorization token to access these endpoints.
> However, requests require a valid `userId`.

---

## 1. Add Item to Cart

**Endpoint:** `POST /cart`
**Description:** Adds a product to the user's cart.

**Request Body (JSON):**
```json
{
  "userId": 1,
  "productId": 1,
  "quantity": 2,
  "price": 999.99,
  "name": "iPhone 15",
  "imageUrl": "http://example.com/iphone.jpg"
}
```

**cURL Command:**
```bash
curl -X POST http://localhost:8080/api/cart \
  -H "Content-Type: application/json" \
  -d '{
    "userId": 1,
    "productId": 1,
    "quantity": 2,
    "price": 999.99,
    "name": "iPhone 15",
    "imageUrl": "http://example.com/iphone.jpg"
  }'
```

**Expected Response (200 OK):**
```json
{
  "id": 1,
  "userId": 1,
  "productId": 1,
  "quantity": 2,
  "price": 999.99,
  ...
}
```

---

## 2. Get User Cart

**Endpoint:** `GET /cart/{userId}`
**Description:** Retrieves all items in a user's cart.

**cURL Command:**
```bash
curl -X GET http://localhost:8080/api/cart/1
```

---

## 3. Update Item Quantity

**Endpoint:** `PUT /cart/{userId}/items/{productId}`
**Description:** Updates the quantity of a specific product in the cart.
**Query Param:** `quantity` (new quantity)

**cURL Command:**
```bash
curl -X PUT "http://localhost:8080/api/cart/1/items/1?quantity=5"
```

---

## 4. Remove Item from Cart

**Endpoint:** `DELETE /cart/{userId}/items/{productId}`
**Description:** Removes a specific product from the cart.

**cURL Command:**
```bash
curl -X DELETE http://localhost:8080/api/cart/1/items/1
```

---

## 5. Clear Cart

**Endpoint:** `DELETE /cart/{userId}`
**Description:** Removes all items from the user's cart.

**cURL Command:**
```bash
curl -X DELETE http://localhost:8080/api/cart/1
```

---

## 6. Health Check

**Endpoint:** `GET /cart/health`
**Description:** Checks if the service is running.

**cURL Command:**
```bash
curl -X GET http://localhost:8080/api/cart/health
```

**Expected Response (200 OK):**
```text
Cart Service is running
```
