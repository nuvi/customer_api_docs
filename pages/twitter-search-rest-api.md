# Twitter Search Historical REST API

## API Requests

## Domain Base

NUVI's RESTful API can be accessed via `HTTPS` at `https://api.nuvi.com/v1`

## Required Headers

You will need to supply two headers with every request:

1. Authorization - (see Request Signing docs or HTTP Basic docs for details)
2. Content-Type - `application/json` to specify the JSON interface

## Get Count

Returns the number of mentions a twitter search queury will pull into monitors.w

### GET https://api.nuvi.com/v1/twitter_search?fromDate=YYYYMMDD0000&toDate=YYYYMMDD0000&query=YourQuery

Response:

```json
{
  "results": [
    {
      "timePeriod": "201812010000",
      "count": 8719
    },
    {
      "timePeriod": "201812110000",
      "count": 10110
    },
    {
      "timePeriod": "201812300000",
      "count": 8332
    }
  ],
   "totalCount": 27161
}
```

## Create Historical Job

Starts a historical job. The number of mentions that will be pulled in can first be checked using the "Get Request Count" endpoint.

### POST https://api.nuvi.com/v1/twitter_search

Reqeust:

```javascript
{
    "fromDate": "201901010000",
    "toDate": "201902010000",
    "query": ["superbowl 2019"],
    "tag": "superbowl monitor", //optional
    "limit": 100 // optional
}
```

Response:

```json
{
    "search_job_id": 12
}
```
