# Auth API Example

This example shows a complete authentication mock API with:

- User registration
- Login with JWT tokens
- Token refresh
- Email verification
- Password reset flow

## Import into Thodr

1. Go to [thodr.com](https://thodr.com) → your project → **Import**
2. Upload or paste the contents of [`openapi/auth-api.yaml`](../../openapi/auth-api.yaml)
3. All endpoints are generated instantly

## Endpoints

| Method | Path                  | Description      |
| ------ | --------------------- | ---------------- |
| POST   | /auth/register        | Create account   |
| POST   | /auth/login           | Login            |
| POST   | /auth/refresh         | Refresh token    |
| GET    | /auth/me              | Get current user |
| POST   | /auth/verify          | Verify email     |
| POST   | /auth/forgot-password | Request reset    |
| POST   | /auth/reset-password  | Reset password   |
| POST   | /auth/logout          | Logout           |

## Suggested scenarios

| Endpoint            | Scenario            | Status |
| ------------------- | ------------------- | ------ |
| POST /auth/login    | Invalid credentials | 401    |
| POST /auth/login    | Account locked      | 423    |
| POST /auth/register | Email taken         | 400    |
| GET /auth/me        | Token expired       | 401    |
| POST /auth/verify   | Invalid code        | 400    |
