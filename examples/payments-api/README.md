# Payments API Example

This example shows a payments/billing mock API with:

- Charge creation and listing
- Refund processing
- Customer management
- Payment method listing
- Subscription management

## Import into Thodr

1. Go to [thodr.com](https://thodr.com) → your project → **Import**
2. Upload or paste the contents of [`openapi/payments-api.yaml`](../../openapi/payments-api.yaml)
3. All endpoints are generated instantly

## Endpoints

| Method | Path                      | Description          |
| ------ | ------------------------- | -------------------- |
| GET    | /payments/charges         | List charges         |
| POST   | /payments/charges         | Create charge        |
| GET    | /payments/charges/{id}    | Get charge           |
| POST   | /payments/refunds         | Create refund        |
| GET    | /payments/customers       | List customers       |
| POST   | /payments/customers       | Create customer      |
| GET    | /payments/customers/{id}  | Get customer         |
| GET    | /payments/payment-methods | List payment methods |
| GET    | /payments/subscriptions   | List subscriptions   |

## Suggested scenarios

| Endpoint                     | Scenario           | Status |
| ---------------------------- | ------------------ | ------ |
| POST /payments/charges       | Card declined      | 402    |
| POST /payments/charges       | Insufficient funds | 402    |
| POST /payments/refunds       | Already refunded   | 409    |
| GET /payments/customers/{id} | Customer not found | 404    |
