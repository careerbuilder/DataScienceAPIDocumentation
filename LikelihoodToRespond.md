Likelihood To Respond Service
=============

Contents

- [Overview](#overview)
- [Request Information](#request-information)
- [Response Information](#response-information)

# Overview

The Likelihood To Respond (LTR) service provides data to describe the likelihood that a job seeker will respond positively when contacted regarding a new job opportunity. Data returned by the service is computed ahead of time by the Search Data Science team and is refreshed nightly.

The LTR service is hosted in the CareerBuilder API routing layer's Staging, US Production, and EU Production environments. The relative path for the service is `/core/classifier/likelihoodtorespond`.

# Request Information

The service responds to three types of requests, described in detail in this section. Requests can be provided as HTTP GETs or HTTP POSTs. POST requests may be sent as either `application/json` or `application/x-www-form-urlencoded` content types.

**Email lookups**<br />
*e.g. ?email_address=example@demo.com*

The service may be queried with an `email_address` parameter to retrieve the LTR record for a particular job seeker. No validation is performed on the provided email string. If no matching record exists, an empty list will be returned.

**Generic threshold queries**<br />
*e.g. ?ltr_threshold=0.05*

The service may be queried with an `ltr_threshold` decimal parameter to retrieve records for job seekers whose generic LTR score matches or exceeds the provided threshold. The `ltr_threshold` parameter must be between 0.0 and 1.0, inclusive.

The `skip_results` integer parameter may optionally be provided to enable "stepping through" a large result set. If no records meet or exceed the provided threshold, an empty list will be returned.

**Carotene-specific threshold queries**<br />
*e.g. ?ltr_threshold=0.05&carotene_id=15.1*

The service may be queried with an `ltr_threshold` parameter accompanied by a `carotene_id` parameter. The `carotene_id` parameter should be a valid and complete CaroteneV3 ID. (A full list of CaroteneV3 IDs can be obtained via the [Taxonomy Service](/TaxonomyService.md).) No validation is performed on the provided Carotene ID.

**Pagination**<br />

Generic and Carotene-specific threshold queries may occasionally yield large numbers of results. For these request types, a response from the service will contain a maximum of 1000 results. The service offers pagination controls to allow users to explore result sets beyond this limit, where one sequence of 1000 results is considered one "page."

The first page of results returned for a query is page 0; re-issuing the request with `page=1` will return results 1001 through 2000, and so on. The response will contain fields for `page` (indicating the current page), `total` (indicating the total size of the result set), and `page_size` (indicating the maximum number of results to be included in one page).

A request specifying an out of bounds `page` value (such as `page=6` for a result set of size 3000) will return zero results. A request specifying `page` as anything other than a nonnegative integer will result in an HTTP 400 Bad Request error.

The ordering of data returned by the service will be consistent within a given upload day. For example, consistency would not be guaranteed when comparing page 1 of results for a request made on Monday with page 2 of results for the same request made on Tuesday.

Empty requests or invalid requests (such as those specifying both `email_address` and `ltr_threshold`, or specifying an `ltr_threshold` value that is not a decimal in the specified range) will result in an HTTP 400 Bad Request error.

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
* `popularity`: A normalized score between 1 (least popular) and 3 (most popular) that describes the frequency with which recruiters have recently performed actions on this job seeker's profile. This field may be null.
* `likelihood_to_respond`: A score between 0.0 (lowest likelihood) and 1.0 (highest likelihood) indicating how likely the job seeker is to respond positively to contacts regarding new job opportunities. This is an overall score that does not consider specific job titles or classifications.
* `likelihood_to_respond_by_carotene`: A map from carotene IDs to LTR scores. Contains between one and three elements describing the carotene ID(s) for which the job seeker is most likely to respond.
* `activity_level`: A normalized score between 1 (least active) and 5 (most active) describing the job seeker's recent activity level on CareerBuilder sites.
* `employed_signal_date`: A date string (in YYYY-MM-DD format) representing the date at which a signal was received indicating that the job seeker was employed. Note that this only indicates when the job seeker confirmed that they were employed, and does not necessarily correspond to the seeker's hiring date. This field may be null.
* `page`: An integer indicating which page of results was returned. Will be identical to the `page` request parameter if provided; otherwise, this field will contain 0.
* `total`: An integer indicating the total size of the result set.
* `page_size`: An integer indicating the maximum number of results that will be returned in a single page. Currently this number is 1000.
* `upload_date`: The date at which the current data set was uploaded. This essentially functions as a version number for data returned by the service. The date will be returned in `YYYY-MM-DD` format.
