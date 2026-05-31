# Contract Testing

Thodr can automatically validate that upstream APIs still match the response shape defined by your mock routes. When breaking changes are detected (removed fields, type changes), you get alerted.

## Setup

### 1. Create a route with a forward URL

```bash
curl -X POST https://thodr.com/api/projects/{project_id}/routes \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "method": "GET",
    "path_pattern": "/users/1",
    "response_status": 200,
    "response_body": "{\"id\": 1, \"name\": \"Leanne Graham\", \"email\": \"test@example.com\"}",
    "forward_url": "https://jsonplaceholder.typicode.com/users/1"
  }'
```

### 2. Enable contract testing

```bash
curl -X PATCH https://thodr.com/api/projects/{project_id}/routes/{route_id} \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "contract_enabled": true,
    "contract_check_interval_minutes": 15
  }'
```

### 3. Trigger an on-demand validation

```bash
curl -X POST https://thodr.com/api/projects/{project_id}/routes/{route_id}/validate \
  -H "Authorization: Bearer <token>"
```

Response:

```json
{
  "status": "passing",
  "score": 100,
  "breaking": [],
  "non_breaking": [
    {
      "path": "$.phone",
      "change_type": "field_added",
      "expected": "not defined",
      "actual": "string",
      "severity": "non_breaking"
    }
  ],
  "upstream_status_code": 200,
  "upstream_latency_ms": 142,
  "checked_at": "2026-06-01T10:30:00Z"
}
```

### 4. View check history

```bash
curl https://thodr.com/api/projects/{project_id}/routes/{route_id}/contract-checks \
  -H "Authorization: Bearer <token>"
```

## Simulating a breaking change

Change the mock response body to expect fields the upstream doesn't have:

```bash
curl -X PATCH https://thodr.com/api/projects/{project_id}/routes/{route_id} \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "response_body": "{\"id\": 1, \"name\": \"Leanne Graham\", \"age\": 25, \"role\": \"admin\"}"
  }'
```

Now validate again — the schema is re-inferred from the new body:

```bash
curl -X POST https://thodr.com/api/projects/{project_id}/routes/{route_id}/validate \
  -H "Authorization: Bearer <token>"
```

Response shows breaking changes:

```json
{
  "status": "failing",
  "score": 55,
  "breaking": [
    {"path": "$.age", "change_type": "field_removed", "expected": "integer", "actual": "missing"},
    {"path": "$.role", "change_type": "field_removed", "expected": "string", "actual": "missing"}
  ],
  "non_breaking": [
    {"path": "$.email", "change_type": "field_added", "expected": "not defined", "actual": "string"},
    {"path": "$.phone", "change_type": "field_added", "expected": "not defined", "actual": "string"}
  ]
}
```

## Notification channels

### Add a Slack notification

```bash
curl -X POST https://thodr.com/api/projects/{project_id}/notifications \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "channel": "slack",
    "config_json": {
      "webhook_url": "https://hooks.slack.com/services/T00/B00/xxxx"
    }
  }'
```

### Add an email notification

```bash
curl -X POST https://thodr.com/api/projects/{project_id}/notifications \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "channel": "email",
    "config_json": {
      "recipients": ["alice@example.com", "bob@example.com"]
    }
  }'
```

### Add a webhook notification (with HMAC signing)

```bash
curl -X POST https://thodr.com/api/projects/{project_id}/notifications \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "channel": "webhook",
    "config_json": {
      "url": "https://your-service.com/thodr-webhook",
      "secret": "my-hmac-secret-key"
    }
  }'
```

## CI/CD integration

Fail your build when upstream APIs introduce breaking changes:

```bash
#!/bin/bash
RESULT=$(curl -s -X POST https://thodr.com/api/projects/{project_id}/routes/{route_id}/validate \
  -H "Authorization: Bearer <api-key>")

STATUS=$(echo $RESULT | jq -r '.status')

if [ "$STATUS" = "failing" ]; then
  echo "❌ Contract test failed!"
  echo $RESULT | jq '.breaking'
  exit 1
fi

echo "✅ Contract test passed."
exit 0
```

## Upstream auth headers

If the upstream API requires authentication:

```bash
curl -X PATCH https://thodr.com/api/projects/{project_id}/routes/{route_id} \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "upstream_auth_headers": {
      "Authorization": "Bearer upstream-api-key",
      "X-API-Key": "secret-key"
    }
  }'
```

Auth headers are encrypted at rest and never returned in API responses.
