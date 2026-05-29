# Auth API — curl examples

Replace `your-project` with your actual Thodr project slug.

## Register

```bash
curl -X POST https://thodr.com/mock/your-project/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "newuser@example.com",
    "password": "securePassword123",
    "name": "Jane Doe"
  }'
```

**Response (201):**

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIs...",
  "token_type": "bearer",
  "user": {
    "id": "usr_new_001",
    "email": "newuser@example.com",
    "name": "Jane Doe",
    "is_verified": false
  }
}
```

## Login

```bash
curl -X POST https://thodr.com/mock/your-project/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "jane@example.com",
    "password": "securePassword123"
  }'
```

**Response (200):**

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIs...",
  "token_type": "bearer",
  "expires_in": 1200
}
```

## Get current user

```bash
curl https://thodr.com/mock/your-project/auth/me \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIs..."
```

**Response (200):**

```json
{
  "id": "usr_42",
  "email": "jane@example.com",
  "name": "Jane Doe",
  "plan": "pro",
  "is_verified": true
}
```

## Forgot password

```bash
curl -X POST https://thodr.com/mock/your-project/auth/forgot-password \
  -H "Content-Type: application/json" \
  -d '{"email": "jane@example.com"}'
```

**Response (200):**

```json
{
  "message": "If an account exists with that email, a reset link has been sent."
}
```

---

> **Tip:** Use Thodr scenarios to switch between success and error responses for the same endpoint. Configure a "401 Unauthorized" scenario to test your error handling.
