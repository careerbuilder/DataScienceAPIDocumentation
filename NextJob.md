Next Job
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

## Summary

The Next Job provides skills related to a particular job and what are next level jobs available and the common skills between the present Job and the next level Jobs.It also tells us what are the missing skills a person should acquire to pursue the next level job. The Next Job is available at 
`/core/careerpath/nextjob`.


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
Response consist of a `carotene_id`,`carotene_title`,list of `skills` of present job where each item contains a `skill` string and a list of `next_jobs` where each item consists of
`carotene_id` string,`carotene_title` string,list of `common_skills` and list of `missing_skills`.

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
