# Response Templating

Thodr supports Handlebars-style placeholders that resolve dynamically per request.

## Injecting Request Data

`json
{
  "user_id": "{{id}}",
  "search": "{{query.q}}",
  "auth_token": "{{header.authorization}}"
}
`

If you call GET /users/42?q=test with Authorization: Bearer xyz:

`ash
curl -H "Authorization: Bearer xyz" \
  "https://thodr.com/api/mock/your-project/users/42?q=test"
`

The response automatically resolves to:

`json
{
  "user_id": "42",
  "search": "test",
  "auth_token": "Bearer xyz"
}
`

## Generating Random Data

`json
{
  "request_id": "{{random.uuid}}",
  "amount": "{{random.int}}",
  "created_at": "{{now}}"
}
`

Returns:

`json
{
  "request_id": "d8e8fca2-dfa8-4c8e-a9b0-9c2b4d6e9f1a",
  "amount": "4281",
  "created_at": "2026-05-29T10:00:00.000Z"
}
`
