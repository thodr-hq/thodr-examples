# Stripe Mock API — curl examples

Replace your-project with your Thodr project slug.

## Create PaymentIntent

`ash
curl -X POST https://thodr.com/api/mock/your-project/v1/payment_intents \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "amount=2000&currency=usd"
`

## Retrieve PaymentIntent

`ash
curl https://thodr.com/api/mock/your-project/v1/payment_intents/pi_123
`

## List Charges

`ash
curl https://thodr.com/api/mock/your-project/v1/charges
`
