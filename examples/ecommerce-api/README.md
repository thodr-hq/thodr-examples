# E-Commerce API Example

This example shows a complete e-commerce mock API with:

- Product catalog (list, search, get by ID)
- Shopping cart (add, remove, get)
- Orders (create, list, get by ID)
- User profiles

## Import into Thodr

1. Go to [thodr.com](https://thodr.com) → your project → **Import**
2. Upload or paste the contents of [`openapi/ecommerce-api.yaml`](../../openapi/ecommerce-api.yaml)
3. All endpoints are generated instantly with realistic response bodies

## Endpoints

| Method | Path           | Description         |
| ------ | -------------- | ------------------- |
| GET    | /products      | List products       |
| GET    | /products/{id} | Get product details |
| GET    | /cart          | Get current cart    |
| POST   | /cart          | Add item to cart    |
| GET    | /orders        | List orders         |
| POST   | /orders        | Create order        |
| GET    | /orders/{id}   | Get order details   |
| GET    | /users/{id}    | Get user profile    |

## Suggested scenarios

| Endpoint           | Scenario          | Status            |
| ------------------ | ----------------- | ----------------- |
| GET /products/{id} | Product not found | 404               |
| POST /orders       | Out of stock      | 409               |
| POST /orders       | Payment failed    | 402               |
| GET /cart          | Empty cart        | 200 (empty array) |
