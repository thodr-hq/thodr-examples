# Payments API — curl examples

Replace `your-project` with your actual Thodr project slug.

## List charges

```bash
curl https://thodr.com/mock/your-project/payments/charges?limit=5
```

**Response (200):**

```json
{
  "data": [
    {
      "id": "ch_001",
      "amount": 4999,
      "currency": "usd",
      "status": "succeeded",
      "customer_id": "cus_42",
      "created_at": "2026-05-01T10:00:00Z"
    }
  ],
  "has_more": false
}
```

## Create a charge

```bash
curl -X POST https://thodr.com/mock/your-project/payments/charges \
  -H "Content-Type: application/json" \
  -d '{
    "amount": 9900,
    "currency": "usd",
    "customer_id": "cus_42",
    "payment_method_id": "pm_card_visa",
    "description": "Annual Pro plan"
  }'
```

**Response (201):**

```json
{
  "id": "ch_003",
  "amount": 9900,
  "currency": "usd",
  "status": "succeeded",
  "customer_id": "cus_42",
  "created_at": "2026-05-25T12:00:00Z"
}
```

## Create a refund

```bash
curl -X POST https://thodr.com/mock/your-project/payments/refunds \
  -H "Content-Type: application/json" \
  -d '{
    "charge_id": "ch_001",
    "amount": 4999,
    "reason": "requested_by_customer"
  }'
```

**Response (201):**

```json
{
  "id": "re_001",
  "charge_id": "ch_001",
  "amount": 4999,
  "currency": "usd",
  "status": "succeeded",
  "reason": "requested_by_customer"
}
```

## Get customer

```bash
curl https://thodr.com/mock/your-project/payments/customers/cus_42
```

**Response (200):**

```json
{
  "id": "cus_42",
  "email": "jane@example.com",
  "name": "Jane Doe",
  "default_payment_method": "pm_card_visa",
  "total_spent": 14998,
  "currency": "usd"
}
```

---

> **Tip:** Configure a "402 Card Declined" scenario on the create charge endpoint to test your payment failure handling.
