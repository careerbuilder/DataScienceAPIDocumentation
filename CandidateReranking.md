Candidate Reranking Service V1
==============================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
    - [Top Level Request](#top-level-request)
    - [Query](#query)
    - [Profiles](#profiles)
- [Response structure](#response-structure)
- [Examples](#examples)
- [Versioning](#versioning)

### Summary
The Candidate Reranking Service is an HTTP Rest service which performs machine learned ranking on a list of candidate profiles. The service makes use of a versioned reranking-library jar maintained by Texkterkernel which applies a machine learning model trained on Talent Discovery Platform query data.

### Request Structure
The service supports POST requests to:
https://api.careerbuilder.com/core/search/candidatereranker
with provided OAUTH credentials

A request is composed of 4 main parts:
`reranker_config`, `source`, `query`, and `profiles`.

#### Top Level Request
| Param    | Type | Description
|----------|------|--------|
| reranker_config | String | **Required**: The `reranker_config` determines which learned model to apply to your incoming rerank request. Currently there is only one reranker model to call so while the field is required it can be left empty or filled with a String value like "RerankerV1".
| source | String | **Required**: Excepted Values: **EDGE** or **MY_SUPPLY**. The source identifies where profile data was attained. A model can be learned based on the source providing more accurate results per each data format.
| query | [Query](#query) | **Required**: The original fully enriched query sent to SOLR for the list of Candidate Profiles.
| profiles | List of [Profiles](#profiles) | **Required**: List of profiles to be reranked.


#### Query
| Param    | Type | Description
|----------|------|--------|
| search_keywords | String | **Required**: Raw query string applied in original search.
| location | String | **Optional**: Search location associated with the search.
| should_use_radius | boolean | **Optional**:


