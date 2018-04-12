# Social Monitor REST API

## API Requests

### Domain Base
NUVI's RESTful API can be accessed via `HTTPS` at `https://api.nuvi.com/v1` 

### Required Headers
You will need to supply two headers with every request:

1. Authorization - (see Request Signing docs or HTTP Basic docs for details)
2. Content-Type - `application/json` to specify the JSON interface

### Request Structure
The structure of a Social Monitor object will look like this

```json
{
  "guid": "d79ba726-f037-4954-93a3-80e243ae0b14",
  "rule": "word ANY Black Friday Sale AND word Marketing Campaign 2017",
  "name": "Black Friday Monitor",
  "status": "active"
}
```

## Schema

| Field Name | Required? | Description |
| --- | --- | --- |
| "name" | True | Name of the social monitor. This will need to be unique
| "rule" | True | NQL rule of the Monitor. See NQL documentation for instructions on how to use |
| "status" | True | Status dictating wether or not Nuvi should collection mentions for this monitor |

### Statuses

| Status | Description |
| --- | --- |
| "active" | We will actively collect mentions for this monitor. Mentions collected will be deducted from you available balance |
| "paused_by_user" | Paused by user. Mentions will not be collected for this monitor |
| "paused_by_overage" | You have exceeded your mention allowance and Nuvi has paused mention collection for this monitor |

### Error Responses 

In the event of an error, we will return a json body that has the following format:

```json
{
  "message": "You were missing a required field",
  "errors": [
    { 
      "message": "A social monitor name is required",
    },
    {
      "message": "A social monitor status is required"
    }
  ]
}
```

### Common Error Codes
| HTTP Status Code | Description |
| --- | --- |
| 404 | Not Found or Unauthorized |
| 422 | Bad Request - The rule you have defined is invalid or is missing parameter(s). |
| 422 | Missing Connector, message: "One or more CONNECTORS are missing or misplaced." |
| 422 | Invalid Value -  message: "Coordinate values are out of bounds|
| 500 | There was a problem on Nuvi's side processing your request |


## Create Monitor

**Request URL**

```
POST https://api.nuvi.com/v1/social_monitors
```

**Request Body**

```json
{
  "rule": "word ANY Black Friday Sale AND word Marketing Campaign 2017",
  "name": "Black Friday Monitor",
  "status": "active"
}
```

**Response Body**

```json
{
  "guid": "d79ba726-f037-4954-93a3-80e243ae0b14",
  "rule": "word ANY Black Friday Sale AND word Marketing Campaign 2017",
  "name": "Black Friday Monitor",
  "status": "active"
}
```

## Update Monitor

**Request URL**

```
PUT https://api.nuvi.com/v1/social_monitors/d79ba726-f037-4954-93a3-80e243ae0b14
```

**Request Body**

```json
{
  "guid": "d79ba726-f037-4954-93a3-80e243ae0b14",
  "rule": "word ANY Black Friday Sale AND word Marketing Campaign 2018",
  "name": "Black Friday Monitor",
  "status": "paused_by_user"
}
```

**Response Body**

```json
{
  "guid": "d79ba726-f037-4954-93a3-80e243ae0b14",
  "rule": "word ANY Black Friday Sale AND word Marketing Campaign 2018",
  "name": "Black Friday Monitor",
  "status": "paused_by_user"
}
```

## List Monitors

**Request URL**

```
GET https://api.nuvi.com/v1/social_monitors
```


**Response Body**

```json
[
  {
    "guid": "d79ba726-f037-4954-93a3-80e243ae0b14",
    "rule": "word ANY Black Friday Sale AND word Marketing Campaign 2018",
    "name": "Black Friday Monitor",
    "status": "paused_by_user"
  }
]
```

## Get Monitor

**Request URL**

```
GET https://api.nuvi.com/api/v1/social_monitors/d79ba726-f037-4954-93a3-80e243ae0b14
```

**Response Body**

```json
[
  {
    "guid": "d79ba726-f037-4954-93a3-80e243ae0b14",
    "rule": "word ANY Black Friday Sale AND word Marketing Campaign 2018",
    "name": "Black Friday Monitor",
    "status": "paused_by_user"
  }
]
```

## Delete Monitor

**Request**

```
DELETE https://api.nuvi.com/v1/social_monitors/d79ba726-f037-4954-93a3-80e243ae0b14
```

**Response**
```
```
