# Analyzing messages for Amazon ES destinations<a name="firehose-message-analysis-elasticsearch"></a>

This page describes how to analyze Amazon SNS messages sent through Amazon Kinesis Data Firehose delivery streams to Amazon Elasticsearch Service \(Amazon ES\) destinations\.

**To analyze SNS messages sent through Kinesis Data Firehose delivery streams to Amazon ES destinations**

1. Configure your Amazon ES resources\. For instructions, see [Getting Started with Amazon Elasticsearch Service](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-gsg.html) in the *Amazon Elasticsearch Service Developer Guide*\.

1. Configure your delivery stream\. For instructions, see [Choose Amazon ES for Your Destination](https://docs.aws.amazon.com/firehose/latest/dev/create-destination.html#create-destination-elasticsearch) in the *Amazon Kinesis Data Firehose Developer Guide*\.

1. Run a query using Amazon ES queries and Kibana\. For more information, see [Step 3: Search Documents in an Amazon ES Domain](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-gsg-search.html) and [Kibana](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-kibana.html) in the *Amazon Elasticsearch Service Developer Guide*\.

## Example query<a name="example-es-query"></a>

The following example queries the `my-index` index for all SNS messages received in the specified date range:

```
POST https://search-my-domain.us-east-1.es.amazonaws.com/my-index/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "range": {
            "Timestamp": {
              "gte": "2020-12-08T00:00:00.000Z",
              "lte": "2020-12-09T00:00:00.000Z",
              "format": "strict_date_optional_time"
            }
          }
        }
      ]
    }
  }
}
```