Next Job
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

## Summary

The Next Job service returns common next jobs for a given carotene ID. For example, Administrative 
Assistant and Office Manager are common next jobs for Receptionists. Additionally it returns the 
set of skills associated with the requested carotene ID, as well as the skills the carotene ID has
in common with its next jobs and the missing skills a person should acquire to pursue the next job. 

The Next Job service is available at `/core/careerpath/nextjob`.

## Request Structure

Requests consist of:

|                   |        |
|-------------------|--------|
|`carotene_id`      | string |
|`carotene_version` | string |
                     
Example request body:

```json
{
  "carotene_id": "53.3",
  "carotene_version": "carotenev3"
}
```

Note that currently only Carotene v3 is supported.

## Response Structure

Response consist of a the `carotene_id`, `carotene_title`, and list of `skills` for the requested 
`carotene_id`. Each item in the `skills` list contains a `skill` string. The `next_jobs` sections is 
a list consisting of objects containing a `carotene_id` and `carotene_title` string for the next job
as well as a list of `common_skills` and list of `missing_skills`.

```json
{
  "data": {
    "carotene_id": "53.3",
    "carotene_title": "material handler (transportation and material moving)",
    "skills": [
      {
        "term": "truckload quantities"
      },
      {
        "term": "packing"
      },
      {
        "term": "lifting"
      },
      {
        "term": "loading trucks"
      },
      {
        "term": "palletizing"
      }
    ],
    "next_jobs": [
      {
        "carotene_id": "53.0",
        "carotene_title": "truck driver",
        "common_skills": [
          {
            "term": "truckload quantities"
          },
          {
            "term": "lifting"
          },
          {
            "term": "loading trucks"
          }
        ],
        "missing_skills": [
          {
            "term": "operate wheeled vehicle"
          },
          {
            "term": "driving"
          },
          {
            "term": "operate truck"
          },
          {
            "term": "operate trailer"
          }
        ]
      },
      {
        "carotene_id": "53.11",
        "carotene_title": "forklift driver",
        "common_skills": [
          {
            "term": "packing"
          },
          {
            "term": "lifting"
          },
          {
            "term": "palletizing"
          }
        ],
        "missing_skills": [
          {
            "term": "driving"
          },
          {
            "term": "cargo handling"
          },
          {
            "term": "cargos"
          }
        ]
      }
    ]
  }
}
```

## Versioning
The current version of the service is 1.0. 

Version must be specified in the Accept header. E.g. `application/json;version=1.0`. 

Our general versioning strategy is available [here](/Versioning.md).