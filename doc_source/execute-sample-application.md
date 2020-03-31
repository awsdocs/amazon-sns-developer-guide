# Step 2: To execute the sample application<a name="execute-sample-application"></a>

1. In the AWS Lambda console, on the navigation panel, choose **Applications**\.

1. On the **Applications** page, in the search field, search for `serverlessrepo-fork-example-ecommerce-my-app` and then choose the application\.

1. In the **Resources** section, do the following:

   1. To find the resource whose type is **ApiGateway RestApi**, sort the resources by **Type**, for example `ServerlessRestApi`, and then expand the resource\.

   1. Two nested resources are displayed, of types **ApiGateway Deployment** and **ApiGateway Stage**\.

   1. Copy the link **Prod API endpoint** and append `/checkout` to it, for example: 

      ```
      https://abcdefghij.execute-api.us-east-2.amazonaws.com/Prod/checkout
      ```

1. Copy the following JSON to a file named `test_event.json`\.

   ```
   {
      "id": 15311,
      "date": "2019-03-25T23:41:11-08:00",
      "status": "confirmed",
      "customer": {
         "id": 65144,
         "name": "John Doe",
         "email": "john.doe@example.com"
      },
      "payment": {
         "id": 2509,
         "amount": 450.00,
         "currency": "usd",
         "method": "credit",
         "card-network": "visa",
         "card-number": "1234 5678 9012 3456",
         "card-expiry": "10/2022",
         "card-owner": "John Doe",
         "card-cvv": "123"
      },
      "shipping": {
         "id": 7600,
         "time": 2,
         "unit": "days",
         "method": "courier"
      },
      "items": [{
         "id": 6512,
         "product": 8711,
         "name": "Hockey Jersey - Large",
         "quantity": 1,
         "price": 400.00,
         "subtotal": 400.00
      }, {
         "id": 9954,
         "product": 7600,
         "name": "Hockey Puck",
         "quantity": 2,
         "price": 25.00,
         "subtotal": 50.00
      }]
   }
   ```

1. To send an HTTPS request to your API endpoint, pass the sample event payload as input by executing a `curl` command, for example:

   ```
   curl -d "$(cat test_event.json)" https://abcdefghij.execute-api.us-east-2.amazonaws.com/Prod/checkout
   ```

   The API returns the following empty response, indicating a successful execution:

   ```
   { }
   ```