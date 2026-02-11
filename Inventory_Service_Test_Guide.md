# Inventory Service API Testing Guide

This guide provides step-by-step instructions to test the **Inventory Service** endpoints.

**Base URL:** `http://localhost:8080/api` (via API Gateway)

> [!NOTE]
> The **Inventory Service is currently unsecured**. You do not need an Authorization token to access these endpoints.

---

## 1. Create Inventory (Add Stock)

**Endpoint:** `POST /inventory`
**Description:** Initializes inventory for a specific product.
**Query Params:** `productId`, `quantity`

**cURL Command:**
```bash
curl -X POST "http://localhost:8080/api/inventory?productId=1&quantity=50"
```

**Expected Response (200 OK):**
(Empty Body or 200 OK status)

---

## 2. Get Stock by Product ID

**Endpoint:** `GET /inventory/{productId}`
**Description:** Retrieves current stock level for a product.

**cURL Command:**
```bash
curl -X GET http://localhost:8080/api/inventory/1
```

**Expected Response (200 OK):**
```json
{
  "id": 1,
  "productId": 1,
  "quantity": 50
}
```

---

## 3. Update Stock (Set Exact Quantity)

**Endpoint:** `PUT /inventory/{productId}/stock`
**Description:** Overwrites the current stock quantity.
**Query Param:** `quantity`

**cURL Command:**
```bash
curl -X PUT "http://localhost:8080/api/inventory/1/stock?quantity=100"
```

**Expected Response (200 OK):**
```json
{
  "id": 1,
  "productId": 1,
  "quantity": 100,
  ...
}
```

---

## 4. Reduce Stock

**Endpoint:** `PUT /inventory/{productId}/reduce`
**Description:** Reduces the stock by a specific amount (e.g., after an order).
**Query Param:** `quantity`

**cURL Command:**
```bash
curl -X PUT "http://localhost:8080/api/inventory/1/reduce?quantity=5"
```

**Using GET to verify change:**
```bash
curl -X GET http://localhost:8080/api/inventory/1
```
*(Should show quantity: 95)*

---

## 5. Health Check

**Endpoint:** `GET /inventory/health`
**Description:** Checks if the service is running.

**cURL Command:**
```bash
curl -X GET http://localhost:8080/api/inventory/health
```

**Expected Response (200 OK):**
```text
Inventory Service is running
```
