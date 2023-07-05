Job To Candidate Recommendation Service
=========================================

- [Summary](#summary)
- [Request](#request)
- [Response](#response)

## Summary
DSAD's job-to-candidate-recs service accepts POST requests which specify a `job_id`, and will return a list of `Candidate` including their `document_id`, `data_source`, `first_name`, `last_name`, `email` and `quality_score`. 

The route: 
- Staging: https://wwwtest.api.careerbuilder.com/core/jobtocandidate/recs
- Produs: https://api.careerbuilder.com/core/jobtocandidate/recs

## Request 

HTTP method: POST (form or JSON)

Parameters:

* `job_id` (required) : the unique id of a job

Example: 
```
{
    "job_id": "j3322"
}
```
## Response
example:
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

