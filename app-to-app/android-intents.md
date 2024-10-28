
## Transactions/Payments Acceptance using Android Intents

Integration with Halo.Go application for transactions using Android Intent Mechanism.

We provide a sample code to help you with the intent request function call. The code is available on [GitHub](https://github.com/).

>   Note: Experience with Android development is advised

### Initiate an Intent Transaction

Retrieve a Transaction ID and payment JWT by hitting the endpoing below. You will need the API Key and Merchant
ID from the previous step for this API call.

#### Post

https://kernelserver.prod.haloplus.io/consumer/intentTransaction

The Call to initiate an Intent Transactioin.

#### Headers

| Name |Type | Description |
| ----------- | ----------- |-------------|
| Content-Type* | String | Content Type of The Request: application/json|
| x-api-key| String | The API Key retrieved from the Merchant Portal|

#### Request Body

| Name |Type | Description |
| ----------- | ----------- |-------------|
| merchantId* | Integer | Merchant ID from Merchant Portal|
| paymentReference*| String | Reference of the transaction|
| amount* |Integer | Amount of the transaction|
| timestamp* | String | ISO Standard Timestamp |
| currencyCode* | String | ISO Standard Currency Codes |
| packageName* | String | Identifier of the app |

#### Response

200: OK Intent Transaction JWT

The response will contain a Transaction ID and JWT Token that will be used in the Intent call

Please see the example of the response body below:

```json
{
    
    "id": "ffe12ca8-61e6-48f9-b89c-537818652988",
    "token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY
    3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2"
}
```

#### Send an Intent Request to the Halo Dot Go App

Initialise the transaction through the **startActivityForResult(Intent, int)** function as shown in the sample app.