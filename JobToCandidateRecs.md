Job To Candidate Recommendation Service
=========================================

- [Summary](#summary)
- [Request Response Structure](#request-response-structure)
- [Develop Setup](#develop-setup)
- [Table Structure](#table-structure)

## Summary
DSAD's job-to-candidate-recs service is a FastAPI based web service, which takes a `job_id` and its related `latitude` 
and `longitude`, and will return a list of `Candidate` within 50 miles of `job_id` location. 

The related dynamodb tables are `JobCandidateGeo` and `CaroteneCandidateGeo`. 

If the `job_id` exists in table `JobCandidateGeo`, the service will grab all items with this `job_id` and collect their `Candidate` attribute as response. If the `job_id` is not found in table `JobCandidateGeo`, the service will try get job's carotene code by calling jobdetails service, and then do a radius search in table `CaroteneCandidateTable` and get candidates within 50 miles of requested job's location for the corresponding `carotene_code`. The service will also add those candidates for the `job_id` to the `JobCandidateGeo` table for future use.

The route: 
- Staging: https://wwwtest.api.careerbuilder.com/core/jobtocandidate/recs
- Produs: https://api.careerbuilder.com/core/jobtocandidate/recs

## Request Response Structure
Request
```
{
    "job_id": "j3322",
    "latitude": 40.508208,
    "longitude": -110.271953
}
```
Response
```
{
    "data": [
        {
            "document_id": "d111",
            "data_source": "data_source",
            "first_name": "kitty2",
            "last_name": "hello2",
            "email": "hehe2@email.com",
            "quality_score": 99.0
        },
        {
            "document_id": "d777",
            "data_source": "data_source",
            "first_name": "kitty3",
            "last_name": "hello3",
            "email": "hehe3@email.com",
            "quality_score": 77.0
        }
    ]
}
```
## Develop Setup

### Poetry config for downloading library from Jfrog

> `poetry config repositories.jfrog https://cbdatascience.jfrog.io/artifactory/api/pypi/pypi-local/simple`
> `poetry config http-basic.jfrog {JFROG_USER_ID} {JFROG_PASSWORD}`
> `poetry install`

### Start the built image locally
> `docker run -p 9000:8000 \
  --env ENVIRONMENT=LOCAL
  --env LOGGLY_URL=https://logs-01.loggly.com/inputs/{loggly_token}/tag/JobToCandiateService-Dev \
  dsad-jobtocandidate-recs`

## Table Structure

### JobCandidateGeo
| Field    | Description |
|----------|-------------|
| `hashKey` | The number of most significant digits (in base 10) of the 64-bit geo hash to use as the hash key. In our case, the dynamodbgeo use Sphere2 to calculate geohash and we chose `4` as the hashKey lenghth. | 
| `rangeKey` | The name of the attribute storing the range key. It is a `uuid` in our table.| 
| `job_id` | The unique job id to locate job. Also serve as the GSI in this table. |
| `candidate` | Candidate with fields: `document_id`, `data_source`, `first_name`, `last_name`, `email`, and `quality_score`. | 
| `geohash` |The name of the attribute storing the full 64-bit geohash. Its value is auto-generated based on item coordinates. | 
| `geoJson`| The name of the attribute which will contain the longitude/latitude pair in a GeoJSON-style point. |

### CaroteneCandidateGeo
| Field    | Description |
|----------|-------------|
| `hashKey` | The number of most significant digits (in base 10) of the 64-bit geo hash to use as the hash key. In our case, the dynamodbgeo use Sphere2 to calculate geohash and we chose `4` as the hashKey lenghth. | 
| `rangeKey` | The name of the attribute storing the range key. It is a `uuid` in our table.| 
| `carotene_code` | Classified job title code. Also serve as the GSI in this table. |
| `candidate` | Candidate with fields: `document_id`, `data_source`, `first_name`, `last_name`, `email`, and `quality_score`. | 
| `geohash` |The name of the attribute storing the full 64-bit geohash. Its value is auto-generated based on item coordinates. | 
| `geoJson`| The name of the attribute which will contain the longitude/latitude pair in a GeoJSON-style point. | 