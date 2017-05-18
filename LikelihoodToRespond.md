Likelihood To Respond Service
=============

Table of Contents
_________
- [Overview](#overview)
- [Request Information](#request-information)
- [Response Information](#response-information)
- [Sample Request](#sample-request)

# Overview

The Likelihood To Respond (LTR) service provides data to describe the likelihood that a job seeker will respond positively when contacted regarding a new job opportunity. Data returned by the service is computed ahead of time by the Search Data Science team and is refreshed nightly.

The LTR service is hosted in the CareerBuilder API routing layer's Staging, US Production, and EU Production environments. The relative path for the service is `/core/classifier/likelihoodtorespond`.

# Request Information

The service responds to three types of requests, described in detail in this section. Requests can be provided as HTTP GETs or HTTP POSTs. POST requests may be sent as either `application/json` or `application/x-www-form-urlencoded` content types.

**Email lookups**<br />The service may be queried with an `email_address` parameter to retrieve LTR records for a particular job seeker. No validation is performed on the provided email string. If no matching record exists, an empty list will be returned.

**Generic threshold queries**<br />The service may be queried with an `ltr_threshold` decimal parameter to retrieve records for job seekers whose generic LTR score matches or exceeds the provided threshold. The `ltr_threshold` parameter must be between 0.0 and 1.0, inclusive. The `skip_results` integer parameter may optionally be provided to enable "stepping through" a large result set. If no records meet or exceed the provided threshold, an empty list will be returned.

**Carotene-specific threshold queries**<br />The service may be queried with an `ltr_threshold` parameter accompanied by a `carotene_id` parameter. The `carotene_id` parameter should be a valid and complete CaroteneV3 ID. (A full list of CaroteneV3 IDs can be obtained via the [Taxonomy Service](/TaxonomyService.md).) No validation is performed on the provided Carotene ID. The `skip_results` integer parameter may optionally be provided to enable "stepping through" a large result set. If the Carotene ID does not match any records, or none of the matched records meet or exceed the provided threshold, an empty list will be returned.

Invalid requests (such as requests specifying both `email_address` and `ltr_threshold`, or with an `ltr_threshold` value that is not a decimal in the specified range) will incur an HTTP 400 Bad Request error.

# Response Information

The service returns a consistent JSON response structure for all requests, regardless of type. All successful responses will return data in the following format:
```
{
  "data": {
    "results": [
      {
        "email_address": string,
        "popularity": int,
        "propensity_to_leave": double,
        "likelihood_to_respond": double,
        "likelihood_to_respond_by_carotene": {
          string: double,
          string: double,
          string: double
        },
        "activity_level": int
      },
      [etc...]
    ],
    "total_results": int,
    "skipped_results": int,
    "upload_date": string
  }
}
```
Each response field is defined as follows:
* `data`: The top-level node for successful responses, as specified by CB API standards.
* `results`: An array of LTR records. May be empty. For email lookup requests, contains a maximum of one element. For both types of threshold query requests, contains a maximum of 1000 elements. (The `skip_results` request parameter may be used to obtain results beyond the first 1000 for a query.)
* `email_address`: The job seeker's email address.
* `popularity`: A normalized score between 1 (least popular) and 3 (most popular) that describes the frequency with which recruiters have recently performed actions on this job seeker's profile.
* `likelihood_to_respond`: A score between 0.0 (lowest likelihood) and 1.0 (highest likelihood) indicating how likely the job seeker is to respond positively to contacts regarding new job opportunities. This is an overall score that does not consider specific job titles or classifications.
* `likelihood_to_respond_by_carotene`: A map from carotene IDs to LTR scores. Contains between one and three elements describing the carotene ID(s) for which the job seeker is most likely to respond.
* `activity_level`: A normalized score between 1 (least active) and 5 (most active) describing the job seeker's recent activity level on CareerBuilder sites.
* `total_results`: An integer representing the total number of results returned for a query.
* `skipped_results`: An integer representing the number of results that were "skipped" from the result set. This value will always be equal to the `skip_results` request parameter if it is provided; otherwise, this value will be zero.
* `upload_date`: The date at which the current data set was uploaded. This essentially functions as a version number for data returned by the service. The date will be returned in `YYYY-MM-DD` format.
