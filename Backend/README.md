# Backend API Documentation

Documentation for the backend endpoints of the Uber Clone application.

---

## User Endpoints

### 1. Register User

Registers a new user account, securely hashes their password, stores the record in the database, and returns a JSON Web Token (JWT) along with the user details.

- **URL:** `/users/register`
- **Method:** `POST`
- **Access / Authorization:** Public

---

### Request Headers

| Header | Type | Description |
| :--- | :--- | :--- |
| `Content-Type` | `string` | Must be `application/json` |

---

### Request Body

The request body must be a JSON object containing the user's personal information, email, and password.

#### Fields

| Field | Type | Required | Validation Rules / Constraints | Description |
| :--- | :--- | :--- | :--- | :--- |
| `fullname.firstname` | `string` | **Yes** | Minimum 3 characters long | User's first name |
| `fullname.lastname` | `string` | No | Minimum 3 characters long (if provided) | User's last name |
| `email` | `string` | **Yes** | Must be a valid email format, minimum 5 characters, unique | User's email address |
| `password` | `string` | **Yes** | Minimum 6 characters long | User's account password |

#### Example Request Body

```json
{
  "fullname": {
    "firstname": "John",
    "lastname": "Doe"
  },
  "email": "john.doe@example.com",
  "password": "secretPassword123"
}
```

---

### Responses

#### 200 OK (Success)

Returned when the user is successfully created and registered.

**Response Body:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "_id": "671234567890abcdef123456",
    "fullname": {
      "firstname": "John",
      "lastname": "Doe"
    },
    "email": "john.doe@example.com"
  }
}
```

---

#### 400 Bad Request (Validation Error)

Returned when required fields are missing, invalid, or do not satisfy validation constraints.

**Response Body:**
```json
{
  "errors": [
    {
      "type": "field",
      "value": "Jo",
      "msg": "First name must be at least 3 characters long",
      "path": "fullname.firstname",
      "location": "body"
    },
    {
      "type": "field",
      "value": "invalid-email",
      "msg": "Invalid Email!",
      "path": "email",
      "location": "body"
    },
    {
      "type": "field",
      "value": "123",
      "msg": "Password  must be at least 6 characters long",
      "path": "password",
      "location": "body"
    }
  ]
}
```

---

#### 500 Internal Server Error

Returned if there is an unhandled server error or database failure (e.g., duplicate email key violation or database connection issues).

**Response Body:**
```json
{
  "message": "Internal server error"
}
```
