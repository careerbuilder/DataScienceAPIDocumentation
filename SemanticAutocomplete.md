Semantic Autocomplete V2
========================

#### Table of Contents

- [Summary](#summary)
- [Request](#request)
- [Response](#response)
- [Example](#example)
- [Versioning](#versioning)


## Summary
The autocomplete service wraps the [Solr Suggester](#https://lucene.apache.org/solr/guide/6_6/suggester.html) which is configured in DSAD's Semantic Search Solr boxes. Version 2 of the Semantic Autocomplete service is a complete rewrite of the original service and makes use of the latest FSTs provided to DSAD from Textkernel which are updated sparodically throughout the year. With the exception of some removed feilds from the response, this version of the service functions exactly like V1.


## Request
As per all DSAD services, this service accepts requests sent as either HTTP GET or HTTP POST and responds to both with the same response. Access to semantic autocomplete is made available at https://www.api.careerbuilder.com/core/semanticsearch/autocomplete and requires Oauth 2.0 client credentials.

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
| query | String | Original query sent to Solr
| suggests | Suggestion[] | A json array of Suggestions broken up by type.

### Suggestion

| Field | Type | Description |
|-------|------|-------------|
| type | String | The type or cannonical form of the suggestion. Accepted values include: *keyword, job_title, job_level, occupation, skill, school, company, location, freetext*
| values | value[] | A json array of values associated with the type.


### Value
A value object represents the surface forms associated with the type(cannonical) suggested for autocomplete. E.g., provided "e" as a query, under the job_title `type` you would see results like engineer, executive, sales executive, etc.

| Field | Type | Description |
|-------|------|-------------|
| name | String | A name associated with the type classification.
| popularity | Integer | An integer value representing the score of popularity indicating how often the value is used.

#### Notable missing fields
The older service maintained a timing object as part of the data field. This is no longer returned.
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
The current version for this service is 2.0. Following the adoption of this version Semantic Autocomplete 1.0 will be shutdown and no longer in use. For more information on our general versioning strategy please visit our guide to versioning [here](/Versioning.md).