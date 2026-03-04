# User CRUD REST API

A production-ready RESTful API built with **Express.js**, **MongoDB**, and **Mongoose**.

---

## 📁 Project Structure

```
user-crud-api/
├── src/
│   ├── config/
│   │   └── database.js          # MongoDB connection
│   ├── controllers/
│   │   └── userController.js    # Business logic for each route
│   ├── middleware/
│   │   ├── errorHandler.js      # Centralised error handling
│   │   └── responseHelper.js    # Consistent JSON response helpers
│   ├── models/
│   │   └── User.js              # Mongoose schema + pre-save hook
│   ├── routes/
│   │   └── userRoutes.js        # Express router
│   ├── app.js                   # Express app setup
│   └── server.js                # Entry point
├── .env.example                 # Required environment variables
├── package.json
└── README.md
```

---

## ⚡ Quick Start

### 1. Install dependencies
```bash
npm install
```

### 2. Configure environment
```bash
cp .env.example .env
# Edit .env with your MongoDB URI and preferred settings
```

### 3. Start the server
```bash
# Production
npm start

# Development (with auto-reload via nodemon)
npm run dev
```

---

## 🔑 Environment Variables

| Variable             | Description                          | Default                                    |
|----------------------|--------------------------------------|--------------------------------------------|
| `PORT`               | Port the server listens on           | `3000`                                     |
| `NODE_ENV`           | Environment mode                     | `development`                              |
| `MONGODB_URI`        | MongoDB connection string            | `mongodb://localhost:27017/user_crud_api`  |
| `BCRYPT_SALT_ROUNDS` | bcrypt hashing cost factor (10–12)   | `10`                                       |

---

## 🛣️ API Endpoints

| Method   | Endpoint          | Description             |
|----------|-------------------|-------------------------|
| `POST`   | `/api/users`      | Create a new user       |
| `GET`    | `/api/users`      | Get all users           |
| `GET`    | `/api/users/:id`  | Get a user by ID        |
| `PUT`    | `/api/users/:id`  | Update a user by ID     |
| `DELETE` | `/api/users/:id`  | Delete a user by ID     |
| `GET`    | `/health`         | Health check            |

---

## 📦 Request & Response Examples

### POST /api/users — Create User
**Request body:**
```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "password": "secret123",
  "age": 28
}
```
**Response (201):**
```json
{
  "success": true,
  "message": "User created successfully",
  "data": {
    "_id": "665f1a2b3c4d5e6f7a8b9c0d",
    "name": "Jane Doe",
    "email": "jane@example.com",
    "age": 28,
    "createdAt": "2024-06-04T10:00:00.000Z",
    "updatedAt": "2024-06-04T10:00:00.000Z"
  }
}
```

### GET /api/users/:id — Get User
**Response (200):**
```json
{
  "success": true,
  "message": "User retrieved successfully",
  "data": { ... }
}
```

### PUT /api/users/:id — Update User
**Request body (only include fields to update):**
```json
{ "age": 29 }
```

### DELETE /api/users/:id — Delete User
**Response (200):**
```json
{
  "success": true,
  "message": "User deleted successfully",
  "data": { "id": "665f1a2b3c4d5e6f7a8b9c0d" }
}
```

---

## ⚠️ Error Responses

All errors follow the same envelope:
```json
{
  "success": false,
  "error": "Human-readable message",
  "details": ["optional array of validation errors"]
}
```

| Scenario                    | Status Code |
|-----------------------------|-------------|
| Missing required fields     | `400`       |
| Invalid ObjectId format     | `400`       |
| Validation failure          | `400`       |
| User not found              | `404`       |
| Duplicate email             | `409`       |
| Database / server error     | `500`       |

---

## 🔒 Security Notes

- Passwords are **hashed with bcrypt** before storage and **never returned** in responses.
- Sensitive config lives in `.env` — never commit `.env` to source control.
- Add authentication middleware (e.g. JWT) before deploying publicly.
