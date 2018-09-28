# Summary

This repository houses documentation for the APIs published by CareerBuilder's Data Science Application Development (DSAD) team. Each API's application-specific documentation resides in a `.md` file bearing the service's name, e.g. `Skills.md` contains documentation on our Skills Extraction API. Application-specific documentation will generally include design principles and goals of the API, a description of the API's request and response structure(s), and (if applicable) a Frequently Asked Questions section for clarifying common questions with the API.

Below, you will find general information pertaining to all DSAD APIs. **Please review this documentation, along with the relevant service-specific documentation, in full before starting to work with any DSAD APIs. By integrating with any of the APIs documented here, you indicate that you have reviewed the relevant documentation and agree to any obligations defined for our API clients.**

### Contact Information

The Data Science Application Development team is the primary point of contact for API-related questions and concerns. The team can be reached at the following email:

![](/assets/dsad.png)

Email inquiries will typically be handled within 1-2 business days, though it may occasionally take longer. For internal users who are part of the CareerBuilder HipChat organization, the team can also be reached via the DataSciAppDevPublic chat room (use @here or @all to ping team members).

For after-hours emergencies concerning production systems, contact the following email. Note that this will trigger our team pager and possibly wake up an engineer -- as such, this email is only to be used for user-impacting production issues that cannot wait until business hours.

![](/assets/dsam.png)

For questions concerning the CB API routing layer, including the OAuth flow described in the [Access](#Access) section, contact the Authorization team at the following email:

![](/assets/authz.png)

### Environments

Unless documented otherwise, all DSAD APIs are available in three environments:

* **Staging** - All new APIs, endpoints, and features -- and changes to existing ones -- are deployed to staging before being deployed to production. The amount of wait time between staging and production deployments varies; typically several days to a week for large or high-risk changes; less for low-risk changes.
 
  Clients of DSAD's APIs are **required** to maintain staging integrations which mirror their production integrations and are sufficiently monitored by the client. DSAD regularly uses data from its staging environment to inform production decisions; for example, a new feature may first be released into staging, and if no issues are caught, then it may proceed into production. Clients who opt out of staging integrations therefore put their applications at risk of unexpected breakages.
 
  Note that the staging environment is currently maintained on a best-effort basis. Service outages occurring in Staging are not considered "on-call" events, and will be addressed as fast as reasonably possible during business hours. Production outages will always take precedence over staging outages. The DSAD team monitors staging in the same ways as production, but with slightly more relaxed thresholds. Clients are discouraged from tying critical paths to DSAD's staging environments, e.g. a client's CI/CD pipeline should not be dependent on DSAD staging environment availability.

* **US Production** - The primary production environment for applications based in the Americas. Most CareerBuilder production systems will use this environment most of the time.

* **EU Production** - A secondary production environment that is hosted entirely within Europe. Data transmitted through the EU Production API environment is guaranteed to remain within the physical bounds of Europe. This production environment therefore satisfies certain EU data privacy laws restricting transmission of European citizens' personal data outside of Europe. This environment may also provide reduced latency to applications located in Europe, Africa, and Asia.

API environment access is determined in a standard way by modifying the "host" portion of the URL in a call to the API routing layer. The correct "host" for each API environment is as follows:

| API Environment | Host |
|----------|-------------|
| Staging | `wwwtest.api.careerbuilder.com` |
| US Production | `api.careerbuilder.com` |
| EU Production | `api.careerbuilder.eu` |

Our API routes and interfaces are identical across regions except for the "host" portion of the URL. (The exception to the rule is that staging and production will be temporarily out of sync whenever a change has been deployed to staging, but not yet to production.) Our API-specific documentation will refer to the US Production version of that API's route(s); simply adjust the "host" value accordingly to determine the proper Staging and EU Production route(s) for the API.

### Access

DSAD APIs are generally not available for free and public use. Access is governed via the CB API routing layer, maintained by CareerBuilder's Authorization team. More specifically, DSAD APIs must be accessed via CareerBuilder's implementation of the OAuth 2.0 Client Credentials flow. This is described in detail [here](/assets/ExternalClientCredentials.pdf).

DSAD clients are required to use a separate, dedicated pair of credentials in each application that consumes DSAD APIs. (An application consuming multiple DSAD APIs may reuse the same credential pair for all integrations.) These credentials must be kept secret and secure, and should not be shared with other clients.

OAuth client credentials in CareerBuilder's authorization systems are stored with a contact email for the owning team or individual. DSAD requires that API clients keep this contact information up-to-date. This information is occasionally necessary for contacting users of DSAD APIs about upcoming releases, breaking changes, planned maintenance periods, etc. **DSAD reserves the right to revoke, without warning, API access for any client found to be noncompliant with these obligations.**

### SLAs

DSAD does not currently publish formal SLAs around API availability or response time. The CareerBuilder standard is 99.5% availability in production; we typically do better than this. Clients with specific questions around average availability, response times, rate limits, etc. should reach out to the team for clarification.

### Versioning and Sunsetting

Please see our [documentation page on versioning](/Versioning.md).

### Common Request/Response Notes

Most DSAD APIs simply expose a "pure" function that transforms some input object into some output object. As such, most of our APIs do not adhere to strict RESTful standards for HTTP verbs. Except where otherwise noted, DSAD APIs respond to HTTP GET and HTTP POST requests identically; clients may use whichever HTTP verb they find most convenient. (For example, HTTP GET requests are easier to construct on the fly, but HTTP POST requests can support larger inputs, which may be a consideration for users of a service such as Resume Parse & Normalize, which receives input documents up to several MB in size.)

All DSAD APIs return JSON responses and, where applicable, accept JSON request bodies. No other serialization formats are currently supported.

Except where otherwise noted, DSAD APIs comply with CareerBuilder API standards with respect to request and response syntax. Specifically, all multi-word request and response field names are written in `lower_snake_case` and all successful API responses are returned as a JSON object with a single top-level "data" object field containing all other response data. Error responses are returned as a JSON object with a single top-level "errors" array field containing one or more objects, defined as follows: `{ type: String, message: String, code: int }`.

### Common Error Codes

Following is a non-exhaustive list of error codes that may be returned by DSAD APIs. Most of our APIs can return most of these codes in the cases described here. Additional codes, or deviations from the behavior-to-code mappings defined here, may be described in API-specific documentation pages.

Note that the CareerBuilder API routing layer or OAuth token endpoint may also occasionally return HTTP errors. In particular, any non-standard error code in the "high 500s" (such as HTTP 572) is generated by the routing layer, not by a DSAD API. Consult the Authorization team for information on these error codes.

* **400 (Bad Request)** : The API request was missing a required field or was otherwise invalid.
* **401 (Unauthorized)** : The API request was not authorized. The API may only accept requests from whitelisted client_ids, or the request may not have been transmitted through the CB API routing layer properly.
* **404 (Not Found)** : The API request targeted a route (or a version of a route) that does not exist.
* **405 (Method Not Allowed)** : The API request specified an HTTP method that is not supported by the targeted API route and version.
* **429 (Too Many Requests)** : The client_id associated with this request has sent too many requests to this API recently, and the API's performance is beginning to degrade as a result. Please wait a few minutes before resuming API calls.
* **500 (Internal Server Error)** : The API request was accepted, but failed to complete successfully due to an internal error within the API. This error should not occur under normal circumstances; if you encounter this, please contact the DSAD team.
* **502 (Bad Gateway)** : The API request was accepted, but failed to complete successfully due to an error which occurred with an upstream provider called during request processing. This error should not occur under normal circumstances; if you encounter this, please contact the DSAD team.
* **503 (Service Unavailable)** : The API was unable to accept your request. This typically happens when our internal infrastructure becomes overwhelmed or unhealthy, either due to excessive request volume or due to other issues which cause our servers to degrade. This error should not occur under normal circumstances; if you encounter this, please contact the DSAD team.
* **504 (Gateway Timeout)** : The API request was accepted, but failed to complete successfully due to an upstream provider called during request processing failing to respond within the required time threshold. This error should not occur under normal circumstances; if you encounter this, please contact the DSAD team.

### Client Libraries

To accelerate the development of new API integrations, DSAD publishes client libraries targeting Java and .NET runtime environments. These libraries are versioned and published into the CareerBuilder Artifactory repository.

CareerBuilder's Artifactory repository is managed by the CloudOps team. Please reach out to this team for assistance  with consuming libraries hosted in Artifactory.