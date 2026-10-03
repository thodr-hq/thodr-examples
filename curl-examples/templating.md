# Response Templating

Thodr resolves a fixed set of `{{...}}` placeholders dynamically on each request. These are Thodr's own template tokens (not a full templating language like Handlebars) — the supported tokens are listed below.

## Injecting request data

```json
{
  "user_id": "{{id}}",
  "search": "{{query.q}}",
  "auth_token": "{{header.authorization}}"
}
```

If you call `GET /users/42?q=test` with `Authorization: Bearer xyz`:

```bash
curl -H "Authorization: Bearer xyz" \
  "https://thodr.com/mock/your-project/users/42?q=test"
```

The response resolves to:

```json
{
  "user_id": "42",
  "search": "test",
  "auth_token": "Bearer xyz"
}
```

## Generating random data

```json
{
  "request_id": "{{random.uuid}}",
  "amount": "{{random.int}}",
  "created_at": "{{now}}"
}
```

Returns:

```json
{
  "request_id": "d8e8fca2-dfa8-4c8e-a9b0-9c2b4d6e9f1a",
  "amount": "4281",
  "created_at": "2026-05-29T10:00:00.000Z"
}
```

## Supported placeholders

| Placeholder                | Resolves to                      |
| -------------------------- | -------------------------------- |
| `{{id}}`                   | Path parameter named `id`        |
| `{{query.<name>}}`         | Query string parameter           |
| `{{header.<name>}}`        | Request header value             |
| `{{random.uuid}}`          | Random UUID v4                   |
| `{{random.int}}`           | Random integer                   |
| `{{now}}`                  | Current UTC timestamp (ISO 8601) |
