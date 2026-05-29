# Scenarios — curl examples

Thodr scenarios let you configure multiple responses for the same endpoint.
Switch between them to test different states without changing your code.

## How scenarios work

1. Create an endpoint (e.g., `GET /users/{id}`)
2. Add scenarios: "Success", "Not Found", "Server Error"
3. Activate a scenario — the endpoint returns that response
4. Switch scenarios anytime from the dashboard

## Example: User endpoint with scenarios

### Default response (200 OK)

```bash
curl https://thodr.com/api/mock/your-project/users/42
```

```json
{
  "id": "42",
  "name": "Jane Doe",
  "email": "jane@example.com",
  "status": "active"
}
```

### After activating "Not Found" scenario (404)

```bash
curl -i https://thodr.com/api/mock/your-project/users/42
```

```
HTTP/1.1 404 Not Found
Content-Type: application/json

{
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "No user found with the given ID."
  }
}
```

### After activating "Server Error" scenario (500)

```bash
curl -i https://thodr.com/api/mock/your-project/users/42
```

```
HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "error": {
    "code": "INTERNAL_ERROR",
    "message": "An unexpected error occurred. Please try again later."
  }
}
```

## Example: Simulating slow responses

Add a delay to any endpoint or scenario:

```bash
# This endpoint has a 2000ms delay configured in Thodr
curl -w "\nTotal time: %{time_total}s\n" \
  https://thodr.com/api/mock/your-project/users/42
```

```
{"id": "42", "name": "Jane Doe"}
Total time: 2.045s
```

Use this to test:

- Loading states in your UI
- Timeout handling in your HTTP client
- Retry logic in your application

## Example: Testing with API key protection

If your project has API key protection enabled:

```bash
# Without API key — rejected
curl -i https://thodr.com/api/mock/your-project/users/42
# → 401 Unauthorized

# With API key — works
curl https://thodr.com/api/mock/your-project/users/42 \
  -H "X-Api-Key: thodr_your_api_key_here"
# → 200 OK
```

---

> **Tip:** Use scenarios to test your entire error handling flow without writing mock code. One URL, multiple behaviors — switch from the dashboard.
