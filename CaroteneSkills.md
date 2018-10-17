Carotene Skills
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

## Summary

The Carotene Skills service provides skills for a given carotene id. CaroteneSkills is available at 
`/core/caroteneskills`.


## Request Structure
Requests consist of:

|                   |        |
|-------------------|--------|
|`carotene_id`      | string | 
|`carotene_version` | string |

Example cURL request:

```
curl -X POST \
  https://api.careerbuilder.com/core/caroteneskills \
  -H 'Accept: application/json;version=1.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
	"carotene_id": "11.0",
	"carotene_version": "CAROTENEV3"
      }'
```

Currently only Carotene v3 is supported. See the documentation for [Job Title service](https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/JobTitle.md)
for more information about carotene classification and carotene IDs.

## Response Structure
The response consist of a list of `skills` where each item contains a `term` string .

```json
{
    "data": {
        "skills": [
            {
                "term": "quality assurance"
            },
            {
                "term": "packing"
            },
            {
                "term": "labelling"
            },
            {
                "term": "sorting"
            },
            {
                "term": "operating instructions"
            }
        ]
    }
}
```


## Versioning
The current version of the service is 1.0. 

Version must be specified in the Accept header. E.g. `application/json;version=1.0`. 

Our general versioning strategy is available [here](/Versioning.md).
