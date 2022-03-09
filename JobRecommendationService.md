# Job Recommendation service
# Contents

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response Structure](#response-structure)
- [Versioning](#versioning)

----------------------------
# Summary

Given a job ID and, optionally, a algorithm name and a host site, the Job Recommendation Service provides recommendations of relevant and similar jobs.

----------------------------
# Request structure
The service supports `GET` and `POST` requests to:
https://api.careerbuilder.com/consumer/recommendations/platform.

OAUTH credentials are **required** and Documentation for CareerBuilder authentication is provided [here](https://apimanagement.cbplatform.link/#/oauth/faq) by the Platform Software team.

The request parameters:

| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| `jobID` | String | **True** | This needs to be a valid jobdid.
| `hostSite` | String | **False** | Hostsite for source `jobID`. Currently supports US, UK, CA. Defaults to US if omitted 
| `personalizationAlgorithm` | String | **False** | Current supported algorithms are `gbr_only`(**default**), `gbr_programmatic`, `gbr_wfh`, `gbr_conversion_rate`


&nbsp;
### Full Request Example

```json
{
  "jobID": "J3T2QQ6CJCRBZ6JS46VS",
  "hostSite": "US",
  "personalizationAlgorithm": "gbr_only"
}
```
----------------------
# Response Structure

All responses with an HTTP status of 200 will consist of a JSON object with a top-level "data" node containing a list of `records`, which is composed of recommended job metadata and its `score`.

Example response body:
```JSON
{
    "data": {
        "records": [
            {
                "city": "Chicago",
                "state": "IL",
                "date_posted": "2022-02-22T01:06:10.0000000Z",
                "company_name": "Button Chrysler Jeep Dodge Ram",
                "expire_date": "2022-03-21T23:59:59.0000000Z",
                "id": "J4R3CJ5X89FNYFPC25K",
                "has_quick_apply": false,
                "title": "Chrysler Certified Automotive Technician",
                "score": 0.7234,
                "location": "US-IL-Chicago",
                "job_details_url": "https://api.careerbuilder.com/v1/joblink?DID=J4R3CJ5X89FNYFPC25K&recid=TR659D6723C6384E9480F9A42071B1AB69",
                "description": "  Chrysler Certified Automotive Technician ...",
                "pay": "$60,000 - $95,000/Year",
                "company_image_url": ""
            }
        ],
        "rec_id": "TR659D6723C6384E9480F9A42071B1AB69"
    }
}
```

&nbsp;

-----------
# Versioning
The current API version is 1.0. The data returned from the service is unversioned.

Our general versioning strategy is available [here](/Versioning.md).
