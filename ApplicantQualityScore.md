Applicant Quality Score Service
====================

Table of Contents
_____________

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

Summary
-----------
Applicant Quality Score Service is a service helps JobSeekers check how well their profiles match job requirements before applying.

For each job you add, we provide a relevance score:

* 1 - Not Relevant
* 2 - Low Relevant
* 3 - Relevant
* 4 - High Relevant

The Applicant Quality Score Service is available at `/core/optimizer`.

Request Structure
-----------

Requests consist of:

| Parameter             | Require/Optional | Type            | Description                               |
|:----------------------|:-----------------|:----------------|:------------------------------------------|
| `email `              | Yes              | String          | JobSeeker Email                           |
| `job_did `            | yes              | Array of String | An array of job DIDs for the application. |


Example cURL request : 

```
curl -X POST \
  https://api.careerbuilder.com/core/application-quality-score \
  -H 'Accept: application/json;version=1.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
        "email":"email@example.com",
        "job_did": ["J8B2606V22T551FGNWF","J8B36Z60T1523M7FNNN","J7X1RQ68Q8Z3QX327WK","J8D8FN7111Q70X662Z1","JHN0YB6BKFL70JB7LB0","JD67L36K6XGYLD9LBGD","JD825Z6WS1GH0QP7KDC"]
      }'
```

Response Structure
-----------
Upon a successful execution, the response will have a status code of 200 and a JSON content with the following structure:

job_did (string): The job ID for which the quality score is calculated.
relevant (number): Indicates the relevance of the applicant to the job.



```json
{
  "data": [
        {
            "job_did": "JD825Z6WS1GH0QP7KDC",
            "relevant": 3,
            "score": 0.69072
        },
        {
            "job_did": "J8D8FN7111Q70X662Z1",
            "relevant": 3,
            "score": 0.62685
        },
        {
            "job_did": "J7X1RQ68Q8Z3QX327WK",
            "relevant": 3,
            "score": 0.66062
        },
        {
            "job_did": "JHN0YB6BKFL70JB7LB0",
            "relevant": 3,
            "score": 0.62271
        },
        {
            "job_did": "J8B2606V22T551FGNWF",
            "relevant": 3,
            "score": 0.6737
        },
        {
            "job_did": "J8B36Z60T1523M7FNNN",
            "relevant": 3,
            "score": 0.66685
        },
        {
            "job_did": "JD67L36K6XGYLD9LBGD",
            "relevant": 3,
            "score": 0.72922
        }
    ]
}
```
Versioning
-----------
The response from the Optimizer Service is versioned with the current version being 1.0. 

Our general versioning strategy is available [here](/Versioning.md).
