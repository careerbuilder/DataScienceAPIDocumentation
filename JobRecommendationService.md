# Job Recommendation service
# Contents

- [Summary](#summary)
- [Request structure](#request-structure)
  - [JobData](#jobdata)
    - [Job](#job)
  - [UserData](#userdata)
  - [UserPreferences](#userpreferences)
    - [PreferredLocation](#preferredlocation)
- [Response Structure](#response-structure)
- [Versioning](#versioning)

----------------------------
# Summary

Given a job ID and, optionally, an email, and a set of user preferences, the Job Recommendation Service provides recommendations of relevant and similar jobs.

The endpoint for Job Recommendation Service is at https://api.careerbuilder.com/core/recommendations/job.

----------------------------
# Request structure
The service supports GET and POST requests to:
https://api.careerbuilder.com/core/recommendations/job.

OAUTH credentials are **required** and the reference is provided [here](https://apimanagement.cbplatform.link/#/oauth/faq).

A request is composed of 3 main parts:
`job_data`, `user_data`, `user_preferences`.

| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| `job_data` | [**JobData**](#jobdata) | **TRUE** | **JobData** contains a list of jobs which are needed for recommendations.
| `user_data` | [**UserData**](#userdata) | **FALSE** | **UserData** contains the  email address for a given user. This data is used for filtering jobs that the user has already applied to from the final recommendations set.
| `user_preferences` | [**UserPreferences**](#userpreferences) | **FALSE** | **UserPreferences** indicates preferences of the user including location, compensation, job titles.

### JobData
| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| `source_jobs` | List<[**Job**](#job)> | **TRUE** | A list of **Job**.

Note: currently only the first job in the list is used for recommendations.

#### Job
| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| `job_did` | String | **TRUE** | This needs to be a valid jobdid.

### UserData
**UserData** contains only one String parameter: `email_address`.

| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| `email_address` | String | **TRUE** | The email of the registered user.


### UserPreferences
**UserPreferences** provides additional constraints for recommendations. The service will rank recommendations based on the location.

| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| `location_filter` | [**PreferredLocation**](#preferredlocation) | **FALSE** | Preferered working location.

#### PreferredLocation
**PreferredLocation** uses coordinates along with the radius to indicate the preferred work location. All the parameters have to be present in valid values.

| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| `latitude` | double | **TRUE** | latitude of the location, range in `(-90, 90)`.
| `longitude` | double | **TRUE** | longitude of the location, range in `(-180, 180)`.
| `radius_in_miles` | float | **TRUE** | The radius surrounding the preferred location within which all recommended jobs must be located.

&nbsp;
### Full Request Example

```json
{
  "job_data": {
      "source_jobs": [
          {
              "job_did": "J2W4YS61P1FX58XCNBT"
          }
      ]
  },
  "user_data": {
      "email_address": "test@example.com"
  },
  "user_preferences": {
    	"location_filter":
      {
          "latitude": 33.9679,
          "longitude": -84.2224,
          "radius_in_miles": 15
      }
  }
}
```
----------------------
# Response Structure

All responses with an HTTP status of 200 will consist of a JSON object with a top-level "data" node containing a list of `recommendations`, which is composed of `job_did` and `score`.

Example response body:
```JSON
{
    "data": {
        "recommendations": [
            {
                "job_did": "JD63LL6GZBV3R6H4K72",
                "score": 0.15
            },
            {
                "job_did": "J4R4MK6BGXMZPMSBTK6",
                "score": 0.14
            },
            {
                "job_did": "J8H74V6HS1X04J38VT9",
                "score": 0.09
            }
        ]
    }
}
```

&nbsp;

-----------
# Versioning
The current API version is 1.0. The data returned from the service is unversioned.

Our general versioning strategy is available [here](/Versioning.md).
