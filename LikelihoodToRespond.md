Likelihood To Respond Service
=============

Table of Contents
_________
- [Request Information](#request-information)
- [Sample Response](#sample-response)
- [Versioning](#versioning)



The Likelihood To Respond (LTR) service allows callers to understand the likelihood that a job seeker will respond positively when contacted regarding a new job opportunity. Data returned by the service is computed ahead of time by the Search Data Science team and is updated nightly.

The LTR service is hosted in the CareerBuilder API routing layer's Staging, US Production, and EU Production environments. The relative path for the service is `/core/classifier/likelihoodtorespond`.

The service responds to three types of requests, described in detail below. Requests can be provided as HTTP GETs or HTTP POSTs. POST requests may be sent as either `application/json` or `application/x-www-form-urlencoded` content types.

