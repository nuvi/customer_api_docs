# Request Signing Authentication
This section explains request authentication with the NUVI Signature Version 2 algorithm.

Authentication with NUVI Signature Version 2 provides the following:

- Identity Verification: Authenticated requests require a signature that you create by using your private API secret key.
- In-transit Data Protection: Signatures are generated using elements of the request and then recalculated by NUVI using those same elements.
- Replay Attack Pevention - Signature are only valid within 15 minutes of the timestamp of the request.


## Signing Requests
Authentication information that you send in a request must include a signature. To calculate a signature perform the following steps:
- Create String To Sign
- Create Signing Key
- Create HMAC of String to Sign using Signing Key

### Create String To Sign
#### Request Body
To create a String To Sign, calculate the hexadecimal encoded (base 16, lower-case, two characters per byte) MD5 hash of the request body.

```
{
  "rule":"word ANY Black Friday Sale AND word Marketing Campaign 2017",
  "name":"Black Friday Monitor",
  "status":"active"
}
```

The preceeding request body would produce the string to sign: `d4ab0fd447b4b197dd676e81e51c0f78`

```
StringToSign = HEX( MD5( RequestBody ) )
```

#### Request Path
If your request doesn't have a message body (GET/DELETE methods), you
will use the path of the endpoint as your string to sign.

You will need to calculate the hexadecimal encoded (base 16, lower-case, two characters per byte) MD5 hash of the path of the request.

If you were requesting a list of all social monitors, you would use a
GET request with the following path. The path would be the string that
you would need to sign:

```
  /v1/social_monitors
```

The preceeding request body would produce the string to sign: `8cfaa58fdf9c796c9b6b5d3be4921941`

### Create Signing Key
To create a signing key:
- Generate a Timestamp string composed of the current time in Unix epoch seconds.
- Calculate the SHA-256 hash-based message authentication code (HMAC) of the Timestamp using your API Secret as the key. This is the signing key. Note - you should NOT convert your signing key to a hexadecimal key. 

The following pseudo-code demonstrates the flow:
```
 Timestamp = "1513707838"
 SigningKey = HMAC_SHA256( API Secret, Timestamp )
```

### Create HMAC of String to Sign using Signing Key
You then use the signing key to calculate the SHA-256 HMAC of the string to sign. The signature is the hexadecimal (base 16, lower-case, two characters per byte) encoded result of this operation.

```
 Signature = HEX( HMAC_SHA256( SigningKey, StringToSign ) )
```

## Authorization Header
You express authentication information using the HTTP Authorization header.
The header has the following form:

[scheme] [credential]

Note that there is a space between the scheme and the credential. The currently supported scheme is `nuvi-hmac-sha256-2`.

The credential has the following form:

AccessID=[API Access ID],Timestamp=[Timestamp],Signature=[Signature]

Where:

- [API Access ID] is the API Access ID generated within NUVI and associated with the API secret used to sign the request.
- [Timestamp] is the time of the request in Unix epoch seconds. It should be the same value used to compute the signing key.
- [Signature] is the Signature calculated as described above

The Access ID, Timestanp, and Signature components of the credential are separated from each other by a comma. The are also followed by an equals sign before their respective values.


### Example with string from request body

Assuming an API Access ID of `EXAMPLE-API-ID` and a API Secret Key of `test_key`

```
nuvi-hmac-sha256-2 AccessID=EXAMPLE-API-ID,Timestamp=1513723633,Signature=8b31a4ffefbf2fc22c3b1a145664e28f16b88587f6c75a285706dceca3afee56
```

### Example with string from request path

Assuming an API Access ID of `EXAMPLE-API-ID` and a API Secret Key of `test_key`

```
nuvi-hmac-sha256-2 AccessID=EXAMPLE-API-ID,Timestamp=1513723633,Signature=0b64a5cc61e3a851e558f79a9fa4e39f7c938be88c128307b98311d30658c078
```

When NUVI receives your request, it checks for the existence of the Authorization header, calculates the signature using the same steps described above, and if the signature matches the one provided in the request, NUVI processes your request.
