Likelihood To Respond Service
=============

Table of Contents
_________
- [Overview](#overview)
- [Request Information](#request-information)
- [Sample Response](#sample-response)
- [Versioning](#versioning)

# Overview

The Likelihood To Respond (LTR) service provides data to describe the likelihood that a job seeker will respond positively when contacted regarding a new job opportunity. Data returned by the service is computed ahead of time by the Search Data Science team and is refreshed nightly.

The LTR service is hosted in the CareerBuilder API routing layer's Staging, US Production, and EU Production environments. The relative path for the service is `/core/classifier/likelihoodtorespond`.

# Request Information

The service responds to three types of requests, described in detail in this section. Requests can be provided as HTTP GETs or HTTP POSTs. POST requests may be sent as either `application/json` or `application/x-www-form-urlencoded` content types.

**Email Lookups**<br />The service may be queried with an `email_address` parameter to retrieve LTR records for a particular job seeker. No validation is performed on the provided email string. If no matching record exists, an empty list will be returned.

**Generic LTR threshold queries**<br />The service may be queried with an `ltr_threshold` decimal parameter to retrieve records for job seekers whose generic LTR score matches or exceeds the provided threshold. The `ltr_threshold` parameter must be between 0.0 and 1.0, inclusive. If no records meet or exceed the provided threshold, an empty list will be returned.

**Carotene-specific LTR threshold queries**<br />The service may be queried with an `ltr_threshold` parameter accompanied by a `carotene_id` parameter. The `carotene_id` parameter should be a valid and complete CaroteneV3 ID. (A full list of CaroteneV3 IDs can be obtained via the [Taxonomy Service](/TaxonomyService.md).) No validation is performed on the provided Carotene ID. If the Carotene ID does not match any records, or none of the matched records meet or exceed the provided threshold, an empty list will be returned.
