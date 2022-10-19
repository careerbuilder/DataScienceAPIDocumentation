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
The Salary service returns salary info for a carotene id and, optionally, a postal or a CBSA code.
Salary info can be requested in either hourly or yearly amounts.

e.g.: How much does a `Customer service representative` make per `year` in `Chicago`?

The Salary service is available at `/core/salary`.

OG data is now supported, you can pass hostite and country to query only on OG data.

Request Structure
-----------

Requests consist of:

| Parameter | Require/Optional | Type | Description |
|:----------|:-------|:-------|:-------|
|`carotene_id`| Yes | string | A unique id representing a carotene job title classification https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/JobTitle.md#taxonomies|
|`cbsa_code` | Optional | string |  Unique Identifiers for US metropolitan statistical areas. |
|`postal_code` | Optional | string | A postal code.|
|`salary_period` | Yes | string | Possible values are "Year", "Hour". | 
|`country` | Yes | string | An ISO-3166 country code.|
|`hostsite` | Optional | string | A valid CareerBuilder hostsite.("US", "OG" are supported for now). Defaulted to US if not provided |

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

Example cURL request with hostsite request:

```
curl -X POST \
  https://api.careerbuilder.com/core/salary \
  -H 'Accept: application/json;version=1.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
	"carotene_id": "43.1",
	"salary_period": "year",
	"country": "UK"
	"hostsite": "OG"
      }'
```

Response Structure
-----------
The returned response is a salary information of a given carotene id and location
consist of:

| Parameter | Type | Description |
|:----------|:-------|:-------|
|`currency` | string | An ISO 4217 currency code.|
|`percentile_10` | string | 10th percentile wage of the requested `salary_period`. |
|`percentile_90` | string |  90th percentile wage of the requested `salary_period`. |
|`percentile_25` | string |  25th percentile wage of the requested `salary_period`. |
|`percentile_75` | string |  75th percentile wage of the requested `salary_period`. |
|`median` | string |  median wage of the requested `salary_period`. |
|`mean` | string |  mean wage of the requested `salary_period`. |
|`employment_per_thousand_jobs` | string | An Employment rate per 1000 jobs  |
|`wage_percent_relative_standard_error` | string | -  |
|`salary_period` | string | Possible values are `YEAR`, `HOUR` based on the requested`salary_period`.| 

Yearly Response:
```json
{
  "data": {
    "currency": "USD",
    "percentile_10": 21870.0,
    "percentile_90": 59100.0,
    "percentile_25": 30660.0,
    "percentile_75": 48340.0,
    "median": 38500.0,
    "mean": 39690.0,
    "employment_per_thousand_jobs": 12.311,
    "wage_percent_relative_standard_error": 1.2,
    "salary_period": "YEAR"
  }
}
```
Hourly Response:
```json
{
  "data": {
    "currency": "USD",
    "percentile_10": 10.52,
    "percentile_90": 29.049999999999997,
    "percentile_25": 14.74,
    "percentile_75": 23.24,
    "median": 18.51,
    "mean": 19.08,
    "employment_per_thousand_jobs": 12.311,
    "wage_percent_relative_standard_error": 1.2,
    "salary_period": "HOUR"
  }
}
```
Versioning
-----------
The response from the Salary Service is versioned with the current version being 1.0. 

Our general versioning strategy is available [here](/Versioning.md).
