# Proxy Forwarding

You can configure any endpoint to forward the request to a real upstream server in a fire-and-forget manner.

1. Add an endpoint in Thodr.
2. Set the orward_url (e.g. https://api.yourcompany.com/webhooks).
3. Send a request to the Thodr mock URL.

`ash
curl -X POST https://thodr.com/api/mock/your-project/webhooks/stripe \
  -H "Content-Type: application/json" \
  -d '{"type": "charge.succeeded"}'
`

Thodr will immediately return the configured mock response (e.g. 200 OK), and asynchronously forward the exact request payload and headers to the upstream server.
