# Monitor Analysis REST API

## API Requests

### Domain Base
NUVI's RESTful API can be accessed via `HTTPS` at `https://api.nuvi.com/v1` 

### Required Headers
You will need to supply two headers with every request:

1. Authorization - (see Request Signing docs or HTTP Basic docs for details)
2. Content-Type - `application/json` to specify the JSON interface

### Request Structure
All parameters are provided through a query string:

```json
https://api.nuvi.com/v1/social_monitors/<MONITOR_GUID>/stats
  ?starting_at_epoch_seconds_utc=1514790001
  &ending_at_epoch_seconds_utc=1515567600
  &agg_list[]=neutral_sentiment
  &agg_list[]=negative_sentiment
  &agg_list[]=positive_sentiment
```

## Schema

| Field Name | Required? | Description |
| --- | --- | --- |
| "starting_at_epoch_seconds_utc" | True | Start date in Unix Epoch |
| "ending_at_epoch_seconds_utc" | True | End date in Unix Epoch |
| "agg_list" | False | An array of analytics to include in response. If not specified all available analytics will be returned |

### Error Responses 

In the event of an error, we will return a json body that has the following format:

```json
{
  "message": "The start date parameter is missing."
}
```

### Common Error Codes
| HTTP Status Code | Description |
| --- | --- |
| 404 | Not Found or Unauthorized |
| 422 | Missing or invalid parameters |
| 500 | There was a problem on NUVI's side processing your request |


## Get Monitor Analysis

**Request URL**

```
GET https://api.nuvi.com/api/v1/social_monitors/d79ba726-f037-4954-93a3-80e243ae0b14/stats
  ?starting_at_epoch_seconds_utc=1514790001
  &ending_at_epoch_seconds_utc=1515567600
  &agg_list[]=neutral_sentiment
  &agg_list[]=negative_sentiment
  &agg_list[]=positive_sentiment
```

**Response Body**

```json
{
  "code": 0,
  "error": "",
  "analysis": {
    "company_guid": "<COMPANY_GUID>",
    "monitor_guid": "d79ba726-f037-4954-93a3-80e243ae0b14",
    "starting_at_epoch_seconds_utc": 1514790001,
    "ending_at_epoch_seconds_utc": 1515567600,
    "agg_list": [
      "neutral_sentiment",
      "negative_sentiment",
      "positive_sentiment"
    ],
    "interval": "1h",
    "mentions": <TOTAL_NUMBER_OF_MENTIONS>,
    "neutral_sentiment": [<DATA>],
    "negative_sentiment": [<DATA>],
    "positive_sentiment": [<DATA>],
  },
  "telemetry": {
    "timed_out": false,
    "partial": false,
    "status": 0,
    "duration": 6000000,
    "duration_millis": 4
  }
}
```
