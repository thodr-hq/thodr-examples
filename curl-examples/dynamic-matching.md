# Dynamic Scenario Matching

Scenarios can activate automatically based on request conditions (headers, query params, body content).

## Example: Error Testing Header

Configure a scenario in Thodr with the condition header.x-test-error = true.

When the header is omitted, you get the default success response:

`ash
curl https://thodr.com/api/mock/your-project/users/42
# -> 200 OK
`

When the header is provided, the error scenario activates dynamically:

`ash
curl -H "x-test-error: true" https://thodr.com/api/mock/your-project/users/42
# -> 500 Internal Server Error
`

## Supported Match Keys

| Key | Example Condition |
|---|---|
| query.<param> | query.env = test |
| header.<name> | header.authorization = secret |
| ody_contains | ody_contains = admin |
