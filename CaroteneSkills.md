Carotene Skills
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

## Summary

The CaroteneSkills provides skills for a particular carotene id. CaroteneSkills is available at 
`/core/caroteneskills`.


## Request Structure
Requests consist of  `carotene_id` string:
                     `carotene_version` string:
                     

```json
{
  "carotene_id": "53.3",
  "carotene_version": "carotenev3"
}
```



## Response Structure
Response consist of a list of `skills` where each item contains a `skill` string .

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
