# Pay order Mercado Pago endpoint

This API is meant to work sending the user to Mercado Pago's check out and bring them back so user can see the result of the transaction for further information on how this 2 staged process works please visit the Mercado Pago's API documentation at the botton of this page ,  in both stages you will need to get the information of the current order from query params. The backend will update the status of the order in the database with the paymentId provided on this step. 


## API Reference

#### Pay order already created
For this endpoint PUT is the only action available

```http
  PUT /api/orders/:id/payMP
```

| Parameter | Type     | Description                |
| :-------- | :------- | :------------------------- |
| `Authorization` | `string` | **Required**. Bearer access_token |
| `Content-Type` | `string` | application/json |



#### Request Body

No request body is required for this endpoint.


#### Response
Success Response

- Status Code: 200 OK
- Response Body: JSON object representing the updated order.

```javascript
{
  "_id": "609c0cdd0d4f180030f20ac2",
  "user": {
    "_id": "609c0bfe0d4f180030f20abf",
    "name": "John Doe",
    "email": "johndoe@example.com"
  },
  "orderItems": [
    {
      "title": "Product 1",
      "quantity": 2,
      "imagesMain": ["image1.jpg"],
      "price": 25.0,
      "discount": 0.1,
      "product": "609c0cdd0d4f180030f20ac1"
    }
  ],
  "shippingAddress": {
    "address": "123 Main St",
    "city": "Los Angeles",
    "postalCode": "90001",
    "country": "USA",
    "email": "johndoe@example.com",
    "phone": "123-456-7890",
    "state": "CA"
  },
  "paymentMethod": "MercadoPago",
  "paymentResult": {
    "id": "123456789",
    "status": "approved",
    "update_time": "2023-09-11T12:34:56Z",
    "email_address": "johndoe@example.com"
  },
  "taxPrice": 5.0,
  "shippingPrice": 10.0,
  "totalPrice": 50.0,
  "isPaid": true,
  "paidAt": "2023-09-11T12:34:56Z",
  "isDelivered": false,
  "deliveredAt": null,
  "createdAt": "2023-05-14T08:42:37.473Z",
  "updatedAt": "2023-09-11T12:34:56.789Z"
}

```

#### Error Responses
- Status Code: '301 Not Authorized'



```
{
  "error": "User not authorized"
}

```
- Status Code: '404 Not Found'



```
{
  "error": "Order not found"
}

```
- Status Code: 500 Internal Server Error 

```
{
  "error": "Internal Server Error",
  "message": "An internal server error occurred."
}
```

## Implementation

### React Example
  - In a React component, you can use the fetch API or a library like axios to make the API request. Here's an example using fetch:

```
import { useEffect } from 'react';

const PaymentComponent = () => {
  const orderId = 'Get_this_from_params';
  const paymentId = 'Get_this_from_params';
  const ACCESS_TOKEN = 'Get_this_from_localstorage_JWT_format'; 

  useEffect(() => {
    const payOrderMP = async () => {
      try {
        const response = await fetch(`http://server-url/api/orders/${orderId}/payMP`, {
          method: 'PUT',
          headers: {
            'Authorization': `Bearer ${ACCESS_TOKEN}`,
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ payment_id: paymentId })
        });

        if (response.status === 200) {
          const updatedOrder = await response.json();
          console.log('Order successfully paid:', updatedOrder);
        } else {
          const errorResponse = await response.json();
          console.error('Error:', errorResponse.error);
        }
      } catch (error) {
        console.error('Error:', error.message);
      }
    };

    payOrderMP();
  }, []);

  return (
    <>
      {/* JSX component here  */}
    </>
  );
};

export default PaymentComponent;

```


## External Documentation



[Mercado Pago Order API](https://www.mercadopago.com.co/developers/es/reference/payments/_payments_id/put)

