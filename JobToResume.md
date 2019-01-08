Job To Resume
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

## Summary

Job To Resume Service recommends resumes based on job data. Job To Resume Service is available at 
`/core/jobtoresume`.


## Request Structure
Requests consist of:

|  Parameter        |     Type      |  Description        |
|-------------------|---------------|---------------------|
|`carotene_id`      | string        | Required. The caroteneID for which resumes should be recommended.
|`carotene_version` | string        | Required. 
|`skills`           | Array(Skill)  | Optional. The skills we are looking for in the resumes for a particular caroteneID.
|`location`         | Location      | Optional. Resume Location.

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
The response consist of a list of `resumes` where each item contains an `id` string which is a resumedid and `score` double which represents how well a given resume matches the job data in the request.

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
