# Stripe Mock API — curl examples

Replace your-project with your Thodr project slug.

## Create PaymentIntent

```bash
curl -X POST https://thodr.com/mock/your-project/v1/payment_intents \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "amount=2000&currency=usd"
```

## Retrieve PaymentIntent

```bash
curl https://thodr.com/mock/your-project/v1/payment_intents/pi_123
```

## List Charges

```bash
curl https://thodr.com/mock/your-project/v1/charges
```
