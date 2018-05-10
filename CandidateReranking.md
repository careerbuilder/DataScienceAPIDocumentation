Candidate Reranking Service V1
==============================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
    - [Top Level Request](#top-level)
    - [Query Object Parts](#query-object-parts)
        - [Query](#query)
        - [Keyword](#keyword)
        - [Parsed Input](#parsed-input)
        - [Filter](#filter)
    - [Profiles](#profiles)
- [Response structure](#response-structure)
- [Versioning](#versioning)

### Summary
The Candidate Reranking Service is an HTTP Rest service which performs machine learned ranking on a list of candidate profiles. The service makes use of a versioned reranking-library jar maintained by Texkterkernel which applies a machine learning model trained on Talent Discovery Platform query data.

### Request Structure
The service supports POST requests to:
https://api.careerbuilder.com/core/search/candidatereranker
OAUTH credentials are **required**.

A request is composed of 4 main parts:
`reranker_config`, `source`, `query`, and `profiles`.

#### Top Level
| Param    | Type | Description
|----------|------|--------|
| reranker_config | String | **Required**: The `reranker_config` determines which learned model to apply to your incoming rerank request. Currently there is only one reranker model to call so while the field is required it can be left empty or filled with a String value like "RerankerV1".
| source | String | **Required**: Excepted Values: **EDGE** or **MY_SUPPLY**. The source identifies where profile data was attained. A model can be learned based on the source providing more accurate results per each data format.
| query | [Query Object](#query-object-parts) | **Required**: The original fully enriched query sent to SOLR for the list of Candidate Profiles.
| profiles | List of [Profiles](#profiles) | **Required**: List of profiles to be reranked.

### Query Object Parts
------
Query Object parts are expected to be taken from the response of the SemanticSearchAPI. All the following objects mirror that response.

##### Query
| Param    | Type | Description
|----------|------|--------|
| search_keywords | String | **Required**: Raw query string applied in original search.
| location | String | **Optional**: Location requested in search.
| should_use_radius | boolean | **Optional**: Flag on the original search to determine use of radius.
| radius | Integer | **Optional**: Radius used in original search.
| extracted_keywords | List of [Keywords](#keyword) | **Optional**: List of Keyword enrichments to the query string or `search_keywords` returned from SemanticSearchAPI.
| parsed_input | ParsedInput | **Optional**: ParsedInput Object comprised of a Map<String,String> as `input_to_extracted_keywords` and a list of [ParsedEntities](#ParsedInput)
| filters | List of [Filters](#Filter) | **Optional**: List of filters used in the original query to search.

##### ParsedInput
| Param    | Type | Description
|----------|------|--------|
| input_to_extracted_keywords | Map<String, String> | **Optional**: Words extracted from `search_keywords` as the **Key** and their relavtive keywords identified by SemanticSearchAPI as the **Value**.
| parsed_entities | ParsedEntity | **Optional**: Entities parsed from `search_keywords`.

##### ParsedEntity
| Param    | Type | Description
|----------|------|--------|
| entity_name | String | **Optional**: Name of the entity.
| is_phrase | Boolean | **Optional**: Flags the entity as phrase or not.
| is_selected | Boolean | **Optinal**: Flags the entity as sleceted or not.

##### Keyword
Entites returned from the SemanticSearchAPI.

| Param    | Type | Description
|----------|------|--------|
| name | String | **Required**: Name of the keyword, e.g. "Java".
| type | Stirng | **Required**: Type of the keyword, e.g. "skill"
| weight | Integer | **Required**: Weight of the keyword as an int.
| relationships | Map<String, List of Entities> | **Optional**: Map of Strings(Semantic Enrichment Type) to a list of of entities. Valid list of Map key values are: `job_titles`, `occupations`, `skills`, `job_level`, `text_kernel_related_keywords`, `interesting_keywords`, `related_keywords`, `related_keywords_recruiter`, `related_keywords_jobseeker`, `raw_job_titles_jobseeker`, `raw_job_titles_recruiter`, `extracted_kewords`, `custom_keywords`, `related_search_terms`, and `job_titles_recruiter`.

##### Entity
| Param    | Type | Description
|----------|------|--------|
| name | String | **Required**: Name of enrichment **Entity**.
| weight | double | **Reqired**: Weight of enriched term according to the SemanticSearchAPI.
| selected | Boolean | **Optional**: Specific for the `related_search_terms` Semantic Enrichment. If selected the value is sent to the Reranker as a feature to be extracted for reranking else it is left out of reranking.

##### Filter
| Param    | Type | Description
|----------|------|--------|
| id | String | **Optional**:  






