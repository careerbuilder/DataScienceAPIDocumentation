Job To Resume
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

## Summary

JobToResume Recommend resumes based on job data. JobToResume is available at 
`/core/jobtoresume`.


## Request Structure
Requests consist of:

|                   |               |
|-------------------|---------------|
|`carotene_id`      | string        | 
|`carotene_version` | string        |
|`skills`           | List(Skill)   |
|`location`         | Location      |

Example cURL request:

```
curl -X POST \
  https://api.careerbuilder.com/core/jobtoresume \
  -H 'Accept: application/json;version=1.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
	"carotene_id": "11.0",
	"skills": [{"skill_did": "KS120L16S3RLJ82VQ08H"}, {"skill_did": "KS120L96KMYTDJ48NRSH"}],
    "location": {"latitude": 40.7831, "longitude": -73.9712},
    "carotene_version": "carotenev3"
      }'
```

Currently only Carotene v3 is supported. See the documentation for [Job Title service](https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/JobTitle.md)
for more information about carotene classification and carotene IDs.

## Response Structure
The response consist of a list of `resumes` where each item contains a `id` string which is resumedid and `score` double which is cosine similarity score of resumedids which is a measure of relevancy of resumdids to the job data in request.

```json
{
  "data": {
    "resumes": [
      {
        "id": "RD77VY60Q4B63RQQDGF",
        "score": 0.3700087736569639
      },
      {
        "id": "RHD3Y879J07Q8V1WPKW",
        "score": 0.3030534085555415
      }
    ]
  }
}
```


## Versioning
The current API version is 1.1. The data returned from the service is unversioned.
