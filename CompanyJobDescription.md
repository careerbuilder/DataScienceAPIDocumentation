Company Job Description
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

## Summary

The Company Job Description service provides a list of job descriptions and education details for jobs that a company has 
previously posted. 

Company Job Description is available at `/core/company/jobdescriptions`.


## Request Structure

Requests consist of:

|                    |        |
|--------------------|--------|
| `company_id`       | string |
| `carotene_id`      | string |
| `carotene_version` | string |
| `city`             | string |
| `state`            | string |
| `country`          | string |

Example cURL request: 

```
curl -X POST \
  https://api.careerbuilder.com/core/company/jobdescriptions \
  -H 'Accept: application/json;version=1.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
        "company_id": "NC5aea2e1d-383f-4659-82d5-bceaf48918c8",
        "carotene_id": "15.5",
        "carotene_version": "caroteneV3",
        "city": "Quantico",
        "state": "VA",
        "country": "US"
      }'
```

Note that currently only Carotene v3 is supported. See the documentation for [Job Title service](https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/JobTitle.md)
for more information about carotene classification and carotene IDs.


## Response Structure
The response consists of a list of `job_descriptions` where each item contains a `description`, an 
`id`, and an `education` object containing a `degree` and a list of `majors`, which have a `major` and
a `confidence`.

Example response body:

```json
{
  "data": {
    "job_descriptions": [
      {
        "id": "617aa090-6d42-3729-9df4-0b803567fd52",
        "description": "Maintain backup/disaster recovery solutions"
      },
      {
        "id": "a255879d-21f9-33e9-b2e4-d7cfc9a40787",
        "description": "Provide user support and guidance"
      }
    ],
    "education": {
      "degree": "Bachelor's Degree",
      "majors": [
        {
          "major": "Computer Science",
          "confidence": 0.41
        },
        {
          "major": "Business Administration",
          "confidence": 0.199
        }
      ]
    }
  }
}
```
 
## Versioning
The current version of the service is 1.0. 

Version must be specified in the Accept header. E.g. `application/json;version=1.0`. 

Our general versioning strategy is available [here](/Versioning.md).
