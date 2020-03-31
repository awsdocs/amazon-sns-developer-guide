# Step 4: To simulate an issue and replay events for recovery<a name="simulate-issue-replay-events-for-recovery"></a>

## Step 1: To enable the simulated issue and send a second API request<a name="enable-simulated-issue-send-second-api-request"></a>

1. Sign in to the [AWS Lambda console](https://console.aws.amazon.com/lambda/)\.

1. On the navigation panel, choose **Functions**\.

1. Search for `serverlessrepo-fork-example` and choose `CheckoutFunction`\.

1. On the **fork\-example\-ecommerce\-*my\-app*\-CheckoutFunction\-*ABCDEF*\.\.\.** page, in the **Environment variables** section, set the **BUG\_ENABLED** variable to **true** and then choose **Save**\.

1. Copy the following JSON to a file named `test_event_2.json`\.

   ```
   {
   	   "id": 9917,
   	   "date": "2019-03-26T21:11:10-08:00",
   	   "status": "confirmed",
   	   "customer": {
   	      "id": 56999,
   	      "name": "Marcia Oliveira",
   	      "email": "marcia.oliveira@example.com"
   	   },
   	   "payment": {
   	      "id": 3311,
   	      "amount": 75.00,
   	      "currency": "usd",
   	      "method": "credit",
   	      "card-network": "mastercard",
   	      "card-number": "1234 5678 9012 3456",
   	      "card-expiry": "12/2025",
   	      "card-owner": "Marcia Oliveira",
   	      "card-cvv": "321"
   	   },
   	   "shipping": {
   	      "id": 9900,
   	      "time": 20,
   	      "unit": "days",
   	      "method": "plane"
   	   },
   	   "items": [{
   	      "id": 9993,
   	      "product": 3120,
   	      "name": "Hockey Stick",
   	      "quantity": 1,
   	      "price": 75.00,
   	      "subtotal": 75.00
   	   }]
   	}
   ```

1. To send an HTTPS request to your API endpoint, pass the sample event payload as input by executing a `curl` command, for example:

   ```
   curl -d "$(cat test_event_2.json)" https://abcdefghij.execute-api.us-east-2.amazonaws.com/Prod/checkout
   ```

   The API returns the following empty response, indicating a successful execution:

   ```
   { }
   ```

## Step 2: To verify simulated data corruption<a name="verify-simulated-data-corruption"></a>

1. Sign in to the [Amazon DynamoDB console](https://console.aws.amazon.com/dynamodb/)\.

1. On the navigation panel, choose **Tables**\.

1. Search for `serverlessrepo-fork-example` and choose `CheckoutTable`\.

1. On the table details page, choose **Items** and then choose the created item\.

   The stored attributes are displayed, some marked as **CORRUPTED\!**

## Step 3: To disable the simulated issue<a name="disable-simulated-issue"></a>

1. Sign in to the [AWS Lambda console](https://console.aws.amazon.com/lambda/)\.

1. On the navigation panel, choose **Functions**\.

1. Search for `serverlessrepo-fork-example` and choose `CheckoutFunction`\.

1. On the **fork\-example\-ecommerce\-*my\-app*\-CheckoutFunction\-*ABCDEF*\.\.\.** page, in the **Environment variables** section, set the **BUG\_ENABLED** variable to **false** and then choose **Save**\.

## Step 4: To enable replay to recover from the issue<a name="enable-replay-recover-from-simulated-issue"></a>

1. In the AWS Lambda console, on the navigation panel, choose **Functions**\.

1. Search for `serverlessrepo-fork-example` and choose `ReplayFunction`\.

1. Expand the **Designer** section, choose the **SQS** tile and then, in the **SQS** section, choose **Enabled**\.
**Note**  
It takes approximately 1 minute for the Amazon SQS event source trigger to become enabled\.

1. Choose **Save**\.

1. To view the recovered attributes, return to the Amazon DynamoDB console\.

1. To disable replay, return to the AWS Lambda console and disable the Amazon SQS event source trigger for `ReplayFunction`\.