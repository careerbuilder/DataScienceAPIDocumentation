Semantic Autocomplete V2
========================

#### Table of Contents

- [Summary](#summary)
- [Request](#request)
- [Response](#response)
- [Example](#example)
- [Versioning](#versioning)


## Summary
Semantic Autocomplete is a service which provides textual autocomplete for search terms relating to DSAD's Semantic Search API. Version 2 of the Semantic Autocomplete service is a complete rewrite of the original service and makes use of the latest FSTs provided to DSAD from Textkernel. Textkernel refines FSTs throughout the year and updates happen every few months. With the exception of some removed feilds from the response, this version of the service functions exactly like V1.

For ease of use DSAD autogenerates and publishes Java and C# SDKs for all of our services using swagger. For documentation on usage please see our SDK repo [here](https://github.com/cbdr/dsad-sdks). All sdks are published to [cbdatascience jfrog artifactory](https://cbdatascience.jfrog.io/cbdatascience/webapp/#/home) and located in *ext-release-local/com/careerbuilder/datascience/sdk/*. Jfrog is managed by the CloudOps team, for permissions accessing artifacts please reach out to CloudOpsSupport@careerbuilder.com.

## Request
As with all DSAD services, Semantic Autocomplete accepts requests sent as either HTTP GET or HTTP POST. Access to semantic autocomplete is made available at:
 https://www.api.careerbuilder.com/core/semanticsearch/autocomplete

 This service requires Careerbuilder Oauth 2.0 client credentials. For more information please see our ReadMe [here](/Readme.md#access).

#### Parameters:
| Parameter | Require/Optional | Type | Description |
|-----------|------------------|------|-------------|
| query | Required | String | The `query` value to autocomplete.
| limit | Optional | Integer | The number of results expected to return.

**The limit field cannot exceed a size of 100 and defaults to 10 without the value present.**

The current version of the service is `2.0`. Version must be specified in the ```Accept``` header.

Example: ```Accept: application/json;version=2.0```

## Response
The response data is broken up into two parts: the original `query` and the list of suggestions returned from Solr. The first `suggestion` will always be identified with the `type` *free_text* and the associated `value` is that of  the highest `popularity` of the returned results.

### Data
| Field | Type | Description |
|-------|------|-------------|
| query | String | Original query sent to Solr.
| suggests | Suggestion[] | A json array of Suggestions broken up by type.

### Suggestion

| Field | Type | Description |
|-------|------|-------------|
| type | String | The type or cannonical form of the suggestion. Possible returned values are: *keyword, job_title, job_level, occupation, skill, school, company, location, freetext*
| values | value[] | A json array of values associated with the type.


### Value
A value object represents the surface forms associated with the type(cannonical) suggested for autocomplete. E.g., provided "e" as a query, under the `type` *job_title* you would see `names` like engineer, executive, sales executive, etc.

| Field | Type | Description |
|-------|------|-------------|
| name | String | A name associated with the type classification.
| popularity | Integer | An integer value indicating how often the term appears in resumes or job postings.

#### Notable Changes in V2
Version 1 of the service maintained a timing object as part of the data field. This is no longer returned in version 2.
```
{
    "data" : {
      ...
      "timing": {
        "time_received": "2018-10-29T17:35:23.724276-04:00",
        "time_elapsed_seconds": 0.031250599999999996
      }
    }

}
```

### Example
Provided the Request: https://www.api.careerbuilder.com/core/semanticsearch/autocomplete?query="e"&limit=5

```
{
  "data": {
    "query": "e",
    "suggests": [
      {
        "type": "freetext",
        "values": [
          {
            "name": "engineer",
            "popularity": 345628
          }
        ]
      },
      {
        "type": "skill",
        "values": [
          {
            "name": "E-Commerce",
            "popularity": 80804
          }
        ]
      },
      {
        "type": "job_title",
        "values": [
          {
            "name": "engineer",
            "popularity": 345628
          },
          {
            "name": "software engineer",
            "popularity": 192082
          },
          {
            "name": "account executive",
            "popularity": 162997
          },
          {
            "name": "sales executive",
            "popularity": 79346
          }
        ]
      }
    ]
  }
```

## Versioning
The current version for this service is 2.0. Following the adoption of this version Semantic Autocomplete 1.0 will be retired. For more information on our general versioning strategy please visit our guide to versioning [here](/Versioning.md).