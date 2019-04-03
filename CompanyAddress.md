Company Address
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

## Summary

The Company Address Service takes a request containing a company id, other company details as described in the below request structure and returns a response containing company Address. CompanyAddress is available at 
`/core/geography/companyaddress`.


## Request Structure
Requests consist of:

| Field Name        | Type   | Required |
|-------------------|--------|----------|
|`company_id`      | string | True      |
|`country` | string | False       |
|`state` | string | False       |
|`city` | string | False       |
|`branch` | string | False       |
|`postal_code` | string | False       |

Example cURL request:

```
curl -X POST \
  https://api.careerbuilder.com/core/geography/companyaddress \
  -H 'Accept: application/json;version=1.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
          "postal_code": "53211",
          "company_id": "NC496cd8bb-93bf-4fe1-b9fc-4c966f4f95e5",
          "city": "Milwaukee",
          "state": "WI",
          "branch": "10th Street Comprehensive Treatment Center",
          "country": "US"
      }'
```



## Response Structure
All responses with an HTTP status of 200 will consist of a JSON object with a top-level "data" node. The response consists of company address and other location parameters like country,state,city,postal code,latitude,longitude and branch codes as show in the below sample response.
### Sample Response
```json
{
  "data": {
    "id": "NC496cd8bb-93bf-4fe1-b9fc-4c966f4f95e5",
    "normalized_name": "Acadia Healthcare Company, Inc.",
    "branch_name": "10th Street Comprehensive Treatment Center",
    "branch_codes": [
      "10th Street Comprehensive Treatment Center"
    ],
    "address": "4800 S 10th St",
    "city": "Milwaukee",
    "state": "WI",
    "postal_code": "53211",
    "country": "US",
    "latitude" : 33.88,
    "longitude" : -88.3
  }
}
```


## Versioning
The current version of the service is 1.0. 

Version must be specified in the Accept header. E.g. `application/json;version=1.0`. 

Our general versioning strategy is available [here](/Versioning.md).
