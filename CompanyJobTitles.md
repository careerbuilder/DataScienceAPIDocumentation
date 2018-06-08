Company Job Titles
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

## Summary

The company Job Titles service provides a list of job titles (along with their ONet and Carotene 
classifications) that are personalized for a given company. Company Job Titles is available at 
`/core/company/jobtitles`.


## Request Structure
Requests consist of a `company_id` string:

```json
{
	"company_id": "NC6623058c-2e0b-4998-ac4a-72e478e91337",
	"carotene_version": "carotenev3",
	"onet_version": "onet17"
}
```

Available taxonomies are `onet17` for `onet_version` and `carotenev3` for `carotene_version`.


## Response Structure
Response consist of a list of `job_titles` where each item contains a `job_title` string and 
its associated ONet and Carotene classification code.

```json
{
    "data": {
        "job_titles": [
            {
                "job_title": "Clerical Assistant",
                "onet_code": "43-4051.00",
                "carotene_code": "43.106"
            },
            {
                "job_title": "Commercial Account Manager / Department Manager",
                "onet_code": "41-3021.00",
                "carotene_code": "41.12"
            }
        ]
    }
}
```


## Versioning
The current version of the service is 1.0. 

Currently ONet17 and Carotene v3 classifications are available.

Version must be specified in the Accept header. E.g. `application/json;version=1.0`. 

Our general versioning strategy is available [here](/Versioning.md).