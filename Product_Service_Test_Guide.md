# Product Service API Testing Guide

This guide provides step-by-step instructions to test the **Product Service** endpoints using Postman or cURL.

**Base URL:** `http://localhost:8080/api` (via API Gateway)

> [!IMPORTANT]
> **Authentication Required:**
> The **Product Service is now secured**.
> - **Public:** `GET` endpoints (Viewing products/categories) are open to everyone.
> - **Restricted:** `POST`, `PUT`, `DELETE` endpoints require an **ADMIN Token**.
>
> You must include the `Authorization: Bearer <token>` header in your requests for creating/updating data.

> [!NOTE]
> Since Products belong to Categories, you should **create a Category first**.

---

## 1. Create a Category

**Endpoint:** `POST /categories`
**Description:** Creates a new product category.

**Request Body (JSON):**
```json
{
  "name": "Electronics",
  "description": "Gadgets and devices"
}
```

**cURL Command:**
```bash
curl -X POST http://localhost:8080/api/categories \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Electronics",
    "description": "Gadgets and devices"
  }'
```

**Expected Response (200 OK):**
```json
{
  "id": 1,
  "name": "Electronics",
  "description": "Gadgets and devices"
}
```

---

## 2. Get All Categories

**Endpoint:** `GET /categories`
**Description:** Retrieves all available categories.

**cURL Command:**
```bash
curl -X GET http://localhost:8080/api/categories
```

---

## 3. Create a Product

**Endpoint:** `POST /products`
**Description:** Creates a new product.
**Important:** You must link it to an existing Category by ID.

**Request Body (JSON):**
```json
{
  "name": "iPhone 15",
  "description": "Latest Apple Smartphone",
  "price": 999.99,
  "quantity": 50,
  "imageUrl": "http://example.com/iphone.jpg",
  "category": {
    "id": 1
  }
}
```

**cURL Command:**
```bash
curl -X POST http://localhost:8080/api/products \
  -H "Content-Type: application/json" \
  -d '{
    "name": "iPhone 15",
    "description": "Latest Apple Smartphone",
    "price": 999.99,
    "quantity": 50,
    "imageUrl": "http://example.com/iphone.jpg",
    "category": {
      "id": 1
    }
  }'
```

**Expected Response (200 OK):**
```json
{
  "id": 1,
  "name": "iPhone 15",
  "price": 999.99,
  "quantity": 50,
  "category": {
    "id": 1,
    "name": "Electronics",
    ...
  },
  ...
}
```

---

## 4. Get All Products

**Endpoint:** `GET /products`
**Description:** Retrieves all products.

**cURL Command:**
```bash
curl -X GET http://localhost:8080/api/products
```

---

## 5. Get Product by ID

**Endpoint:** `GET /products/{id}`
**Description:** Retrieves a specific product.

**cURL Command:**
```bash
curl -X GET http://localhost:8080/api/products/1
```

---

## 6. Search Products

**Endpoint:** `GET /products/search`
**Description:** Searches products by name.
**Query Param:** `q` (request query)

**cURL Command:**
```bash
curl -X GET "http://localhost:8080/api/products/search?q=iPhone"
```

---

## 7. Update Stock

**Endpoint:** `PUT /products/{id}/updateStock`
**Description:** Updates the quantity of a product.
**Query Param:** `quantity` (new quantity)

**cURL Command:**
```bash
curl -X PUT "http://localhost:8080/api/products/1/updateStock?quantity=100"
```

**Expected Response (200 OK):**
```text
Stock updated successfully
```

---

## 8. Health Check

**Endpoint:** `GET /products/health`
**Description:** Checks if the service is running.

**cURL Command:**
```bash
curl -X GET http://localhost:8080/api/products/health
```

**Expected Response (200 OK):**
```text
Product Service is running
```
