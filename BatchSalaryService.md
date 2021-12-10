Batch Salary Service
====================

Table of Contents
_____________

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

Summary
-----------
The Batch Salary service returns salary info for a or multiple carotene id(s) and, optionally, a postal or a CBSA code.
Salary info can be requested in either hourly or yearly amounts.

The Batch Salary service is available at `/core/salaries`.

Request Structure
-----------

Requests consist of:

| Parameter | Require/Optional | Type | Description |
|:----------|:-------|:-------|:-------|
|`carotene_id`| Yes | string[] | An array of unique id(s) representing a carotene job title classification https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/JobTitle.md#taxonomies|
|`cbsa_code` | Optional | string |  Unique Identifiers for US metropolitan statistical areas. |
|`postal_code` | Optional | string | A postal code.|
|`salary_period` | Yes | string | Possible values are "Year", "Hour". | 
|`country` | Yes | string |An ISO-3166 country code.|

Example cURL request:

```
curl -X POST \
  https://api.careerbuilder.com/core/salaries \
  -H 'Accept: application/json;version=1.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
	"carotene_id": ["43.1", "11.0"]
	"cbsa_code": "26420",
	"salary_period": "year",
	"country": "US"
      }'
```

Response Structure
-----------
The returned response is an array of salary information of a given carotene id(s) and location
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
|`carotene_id` | string | An array unique id(s) representing a carotene job title classification https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/JobTitle.md#taxonomies|

Response:
```json
{
  "data": {
    "salary_info": [
      {
        "percentile_10": 76460.0,
        "percentile_25": 97680.0,
        "median": 132990.0,
        "mean": 147450.0,
        "percentile_75": 183670.0,
        "salary_period": "YEAR",
        "currency": "USD",
        "employment_per_thousand_jobs": 2.0,
        "wage_percent_relative_standard_error": 2.1,
        "carotene_id": "11.0"
      },
      {
        "percentile_10": 21870.0,
        "percentile_25": 30660.0,
        "median": 38500.0,
        "mean": 39690.0,
        "percentile_75": 48340.0,
        "percentile_90": 59100.0,
        "salary_period": "YEAR",
        "currency": "USD",
        "employment_per_thousand_jobs": 12.0,
        "wage_percent_relative_standard_error": 1.2,
        "carotene_id": "43.1"
      }
    ]
  }
}
```
Versioning
-----------
The response from the Batch Salary Service is versioned with the current version being 1.0.

Our general versioning strategy is available [here](/Versioning.md).
