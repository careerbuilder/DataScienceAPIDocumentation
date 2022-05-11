Programmatic Pricing
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

## Summary

The Programmatic Pricing service provides an estimated price for a job based on OnetCode and a location (not required). Programmatic Pricing  is available at
`/core/programmatic-pricing`.


## Request Structure

| Field name        | Type   |Required|
|:------------------|:-------|:-------|
|`onet_id`          | string |    Y   |
|`onet_version`     | string |    Y   |
|`locality`         | string |    N   | 
|`admin_area_1`     | string |    N   |

```json
{
  "onet_id": "29-1141.00",
  "onet_version": "ONET17",
  "locality": "los angeles",
  "admin_area_1": "ca"
}
```
Allowed method is :"POST"

The ONet version currently supported is listed in the [Versioning](#versioning)
section of this document and is subject to change over time. Any removal of support for a taxonomy
version will be implemented as a versioned change to the API itself.


## Response Structure
Response consist of a float of `price`.

```json
{
  "data": {
    "price": 21.88
  }
}
```

## Versioning
The current version of the service is 1.0.

Currently ONet17 classification is available.


Our general versioning strategy is available [here](/Versioning.md).
