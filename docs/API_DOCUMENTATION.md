# Chirpy API Documentation

Chirpy is a microblogging platform with a RESTful API for user management, posting short messages ("chirps"), authentication, and admin operations. Endpoints generally accept and return JSON.

## Table of Contents
- [Authentication](#authentication)
- [Chirps](#chirps)
- [Users](#users)
- [Token Management](#token-management)
- [Admin & Metrics](#admin--metrics)
- [Webhooks](#webhooks)
- [Health Check](#health-check)

---

## Authentication

Chirpy uses JWT tokens for user authentication. For endpoints requiring authentication, include the header:

```
Authorization: Bearer <JWT_TOKEN>
```

---

## Chirps

### Create Chirp
- **POST** `/api/chirps`
- **Auth:** Required (Bearer JWT)
- **Body:**
  ```json
  { "body": "string" }
  ```
- **Response:** `201 Created`
  ```json
  {
    "id": "uuid",
    "created_at": "timestamp",
    "updated_at": "timestamp",
    "body": "string",
    "user_id": "uuid"
  }
  ```

### Get All Chirps
- **GET** `/api/chirps`
- **Query Params:**
  - `author_id` (optional): Filter by user ID.
  - `sort` (optional): "asc" (default) or "desc" by created time.
- **Response:** `200 OK`
  ```json
  [
    {
      "id": "uuid",
      "created_at": "timestamp",
      "updated_at": "timestamp",
      "body": "string",
      "user_id": "uuid"
    },
    ...
  ]
  ```

### Get Chirp by ID
- **GET** `/api/chirps/{chirpID}`
- **Response:** `200 OK`
  ```json
  {
    "id": "uuid",
    "created_at": "timestamp",
    "updated_at": "timestamp",
    "body": "string",
    "user_id": "uuid"
  }
  ```

### Delete Chirp
- **DELETE** `/api/chirps/{chirpID}`
- **Auth:** Required (Bearer JWT)
- Only the chirp's author may delete.
- **Response:** `204 No Content`

---

## Users

### Register
- **POST** `/api/users`
- **Body:**
  ```json
  {
    "email": "string",
    "password": "string"
  }
  ```
- **Response:** `201 Created`
  ```json
  {
    "id": "uuid",
    "created_at": "timestamp",
    "updated_at": "timestamp",
    "email": "string",
    "is_chirpy_red": false
  }
  ```

### Login
- **POST** `/api/login`
- **Body:**
  ```json
  {
    "email": "string",
    "password": "string"
  }
  ```
- **Response:** `200 OK`
  ```json
  {
    "token": "jwt_token",
    "refresh_token": "string"
  }
  ```

### Update User
- **PUT** `/api/users`
- **Auth:** Required (Bearer JWT)
- **Body:** (fields to update)
  ```json
  {
    "email": "string",
    "password": "string"
  }
  ```
- **Response:** `200 OK`
  ```json
  {
    "id": "uuid",
    "created_at": "timestamp",
    "updated_at": "timestamp",
    "email": "string",
    "is_chirpy_red": true|false
  }
  ```

---

## Token Management

### Refresh Token
- **POST** `/api/refresh`
- **Body:**
  ```json
  { "refresh_token": "string" }
  ```
- **Response:** `200 OK`
  ```json
  { "token": "new_jwt_token" }
  ```

### Revoke Token
- **POST** `/api/revoke`
- **Body:**
  ```json
  { "refresh_token": "string" }
  ```
- **Response:** `204 No Content`

---

## Admin & Metrics

### Health Check
- **GET** `/api/healthz`
- **Response:** `200 OK`
  - Returns a simple health check message.

### Metrics
- **GET** `/admin/metrics`
- **Response:** `200 OK`
  - Returns HTML with file server hit count.

### Reset (Dev only)
- **POST** `/admin/reset`
- **Response:** `200 OK`
  - Resets the database and file server hit count (only in development environment).

---

## Webhooks

### Polka Webhook
- **POST** `/api/polka/webhooks`
- **Auth:** Required (ApiKey)
  - Header: `Authorization: ApiKey <POLKA_KEY>`
- **Body:**
  ```json
  {
    "event": "user.upgraded",
    "data": { "user_id": "uuid" }
  }
  ```
- **Response:** `204 No Content`

---

## Chirp Validation

### Validate Chirp Body
- **POST** `/api/validate_chirp`
- **Body:**
  ```json
  { "body": "string" }
  ```
- **Response:** `200 OK`
  ```json
  { "cleaned_body": "string" }
  ```
  - Cleanses the chirp body (removes banned words, checks length).

---

## Error Handling

- Errors are returned as JSON:
  ```json
  { "error": "description" }
  ```

---

## Static Files

- **GET** `/app/*` — Serves frontend files.
- **GET** `/assets` — Serves static assets.

---

## Notes

- All timestamps are in RFC3339 format.
- All UUIDs are standard v4 UUIDs.
- For endpoints that require authentication, invalid or missing tokens will result in `401 Unauthorized`.

---

For more details, see the implementation in the [Chirpy GitHub repository](https://github.com/jacobdanielrose/chirpy).