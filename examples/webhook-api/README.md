# Webhook Mock Example

This example shows how to use Thodr to mock webhook event payloads.

## Use case

You're building a webhook handler (e.g., for Stripe, GitHub, or any third-party service) and need to test it locally without triggering real events.

## How it works

1. Import the [`stripe-webhook-mock.yaml`](../../openapi/stripe-webhook-mock.yaml) spec into Thodr
2. Each endpoint returns a realistic webhook event payload
3. Point your local handler at the Thodr mock URL to test processing logic

## Endpoints

| Method | Path                           | Event type                    |
| ------ | ------------------------------ | ----------------------------- |
| GET    | /webhooks/checkout-completed   | checkout.session.completed    |
| GET    | /webhooks/subscription-updated | customer.subscription.updated |
| GET    | /webhooks/subscription-deleted | customer.subscription.deleted |
| GET    | /webhooks/payment-failed       | invoice.payment_failed        |
| GET    | /webhooks/payment-succeeded    | invoice.payment_succeeded     |

## Testing your webhook handler

```bash
# Fetch a mock event payload
EVENT=$(curl -s https://thodr.com/mock/your-project/webhooks/checkout-completed)

# Forward it to your local handler
curl -X POST http://localhost:3000/api/webhooks/stripe \
  -H "Content-Type: application/json" \
  -d "$EVENT"
```

## With Thodr webhook forwarding

Alternatively, use Thodr's built-in forwarding:

1. Create a `POST /webhooks/stripe` endpoint in Thodr
2. Set the **forward URL** to `https://your-ngrok-url.ngrok.io/api/webhooks/stripe`
3. Every request to your Thodr mock URL is forwarded to your local server

This lets you test webhook delivery without exposing localhost directly.
