Salary Service
====================

Table of Contents
_____________

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

Summary
-----------
The Salary service returns salary info for a carotene id and, optionally, a postal or a cbsa code in either hourly or yearly amounts.

e.g.: How much does a `Customer service representative` make per `year` in `Chicago`?

The Salary service is available at `/core/salary`.

Request Structure
-----------

Requests consist of:

| Parameter | Require/Optional | Type | Description |
|:----------|:-------|:-------|:-------|
|`carotene_id`| Yes | string | A unique id|
|`cbsa_code` | Optional | string |  Unique Identifiers for US Metropolitan Statistical areas. |
|`postal_code` | Optional | string |  Optional. The post code, postal code, or ZIP Code of an address.|
|`salary_period` | Yes | string | Possible values are "Year", "Hour". | 
|`country` | Yes | string |A country name or two-letter ISO-3166 country code. Supports only "US". |

Example cURL request with cbsa_code:

```
curl -X POST \
  https://api.careerbuilder.com/core/salary \
  -H 'Accept: application/json;version=1.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
	"carotene_id": "43.1",
	"cbsa_code": "26420",
	"postal_code": "",
	"salary_period": "year",
	"country": "US"
      }'
```

Example cURL request with postal_code:

```
curl -X POST \
  https://api.careerbuilder.com/core/salary \
  -H 'Accept: application/json;version=1.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
	"carotene_id": "43.1",
	"postal_code": "77494",
	"cbsa_code": "",
	"salary_period": "year",
	"country": "US"
      }'
```

Example cURL request nationwide:

```
curl -X POST \
  https://api.careerbuilder.com/core/salary \
  -H 'Accept: application/json;version=1.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
	"carotene_id": "43.1",
	"salary_period": "year",
	"country": "US"
      }'
```
Response Structure
-----------

The returned response is a salary information of a given carotene id and location,
which consists of `currency`, `percentile_10`, `percentile_90`, `period`.
```json
{
  "data": {
    "currency": "USD",
    "percentile_10": 21870.0,
    "percentile_90": 59100.0,
    "period": "YEAR"
  }
}

```

Versioning
-----------
The response from the Salary Service is versioned with the current version being 1.0. 

Our general versioning strategy is available [here](/Versioning.md).
