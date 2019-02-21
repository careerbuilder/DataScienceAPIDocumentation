Candidate Reranking Service V1
==============================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
    - [Query](#query)
        - [Query](#query)
        - [Location](#location)
        - [ParsedInput](#parsed-input)
        - [ParsedEntity](#parsedentity)
        - [Keyword](#keyword)
        - [Relationship](#relationship)
        - [Entity](#entity)
        - [Filter](#filter)
    - [Profile](#profile)
        - [Profile](#profile)
        - [RecentJob](#recentjob)
        - [Skill](#skill)
- [Response Structure](#response-structure)
- [Versioning](#versioning)

### Summary
The Candidate Reranking Service is an HTTP Rest service which performs machine learned ranking on a list of candidate profiles. The service makes use of a versioned reranking-library jar maintained by Textkernel which applies a model trained on Talent Discovery Platform search data.

### Request Structure
The service supports POST requests to:
https://api.careerbuilder.com/core/search/candidatereranker.

OAUTH credentials are **required**.

A request is composed of 4 main parts:
`model_version`, `source`, `query`, and `profiles`.

| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| model_version | String | **TRUE** | Accepted Value: **X.Y** The `model_version` determines which learned model to apply to your incoming rerank request. Currently there is only two reranker models to call, **1.0** and **1.1**. The default value is 1.0.
| source | String | **TRUE** | Accepted Values: **EDGE** or **MY_SUPPLY**. The source identifies where profile data was obtained. A model can be learned based on the source, thereby providing more accurate results per each data format.
| query | [Query](#query) | **TRUE** | The original, fully enriched query sent to SOLR for the list of candidate profiles.
| profiles | [Profile[]](#profile) | **TRUE** | Array of profiles to be reranked.

### Query

----------
The query and its child objects are expected to be taken from the response of the [SemanticSearchAPI](https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/SemanticSearchV2.md). All the following objects mirror that response.

#### Query
| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| search_keywords | String | **FALSE** | Raw query string applied in original search.
| location | [Location](#Location) | **FALSE** | Location Object comprised of location related information, including `city`, `state`, `country`, `latitude` and `longitude`.
| should_use_radius | boolean | **FALSE** | Flag on the original search to determine use of radius.
| radius | Integer | **FALSE** | Radius used in original search.
| extracted_keywords | [Keyword[]](#keyword) | **FALSE** | Array of **Keyword** enrichments to the query string or `search_keywords` returned from SemanticSearchAPI.
| parsed_input | ParsedInput | **FALSE** | **ParsedInput** Object comprised of a Map<String,String> as `input_to_extracted_keywords` and a list of [ParsedEntities](#ParsedInput)
| filters | [Filters[]](#Filter) | **FALSE** | Array of filters used in the original query to search.

#### Location
| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| city | String | **FALSE** | City name for the query.
| state | String | **FALSE** | State name for the query.
| country | String | **FALSE** | Country name for the query.
| latitude | String | **FALSE**<sup>1<sup> | Latitude for the location.
| longitude | String | **FALSE**<sup>1<sup> | Longitude for the location.
1: Latitude and longitude can be omitted all together, but if one of them is provided, the other is needed too.

#### ParsedInput
| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| input_to_extracted_keywords | Map<String, String> | **FALSE** | Words extracted from `search_keywords` as the **Key** and their relavtive keywords identified by SemanticSearchAPI as the **Value**.
| parsed_entities | ParsedEntity | **FALSE** | Entities parsed from `search_keywords`.

#### ParsedEntity
| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| entity_name | String | **FALSE** | Name of the entity.
| is_phrase | Boolean | **FALSE** | Flags the entity as a phrase or not.
| is_selected | Boolean | **FALSE** | Flags the entity as sleceted or not.

#### Keyword
Entities returned from the SemanticSearchAPI.

| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| name | String | **FALSE** | Name of the keyword, e.g. "Java".
| type | String | **FALSE** | Type of the keyword, e.g. "skill".
| weight | Integer | **FALSE** | Weight of the keyword as an int. \[0-100\]
| relationships | [Relationship](#relationship) | **FALSE** | Array of relationship objects which enrich the keyword with additional entities.

#### Relationship
A `relationship` is composed of a **\<semantic enrichment type\>** and the associated set of entities which can be classified under that **\<semantic enrichment type\>**. Accepted values for the **\<semantic enrichment type\>** are: `job_titles`, `occupations`, `skills`, `job_level`, `text_kernel_related_keywords`, `interesting_keywords`, `related_keywords`, `related_keywords_recruiter`, `related_keywords_jobseeker`, `raw_job_titles_jobseeker`, `raw_job_titles_recruiter`, `extracted_kewords`, `custom_keywords`, `related_search_terms`, and `job_titles_recruiter`.

| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| **\<semantic enrichment type\>** | [Entity[]](#entity) | **FALSE** | Relationship to the keyword `name` value.

#### Entity
An `entity` is composed of a `name`, `weight`, and `selected` boolean. An `entity` enriches the original keyword with terms or phrases which share a close association. For example, if you were to have a `keyword` java, an enrichment entity might be development or programming.

| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| name | String | **FALSE** | Name of enrichment `entity`. e.g., javascript, java, programming or software engineer.
| weight | double | **FALSE** | Weight of enriched term according to the Semantic Search API. \[0.0 - 1.0\]
| selected | boolean | **FALSE** | Specific for the `related_search_terms` Semantic Enrichment. If selected the value is sent to the Reranker as a feature to be extracted for reranking else it is left out of reranking. All entites default to true.

#### Filter
| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| id | String | **FALSE** | Unique identfier for the **Filter**.
| name | String | **FALSE** | Name of the **Filter**.
| type | String | **FALSE** | Type of **Filter** applied. Accepted values are `facet` or `range`. Service defaults to `facet`.
| values | String[] | **FALSE** | Values chosen to filter on from the original search request **Filter**.

### Profile
----------
A profile represents a candidate and is composed of data which, in relation to the query, can help the **Reranker** score the candidate higher or lower in the re-ordered results.

**IMPORTANT: Though many fields on the profile are optional it is encouraged to fill out the profile as fully as possible in order to get the best results from the Reranker**.

#### Profile
| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| document_id | String | **FALSE** | Unique CB identifier string for the candidate.
| recent_jobs | [RecentJobs[]](#recentjob) | **FALSE** | Recent jobs held by the candidate.
| skills | [Skill[]](#skill) | **FALSE** | Array of **Skill** objects. Skills associated with the candidate.
| years_of_experience | Double | **FALSE** | Total years of experience the candidate has.
| normalized_education_level | String[] | **FALSE** | Normalized Educations associated with the candidate i.e., Master's Degree, Bachelor's Degree etc.
| unnormalized_education_level | String[] | **FALSE** | Unnormalized Educations associated with the candidate. i.e., masters degree, Master of Science, Masters etc.
| majors | String[] | **FALSE** | The array of majors or fields of study the candidate's degree pertains to.
| city | String | **FALSE** | City that the candidate is located in.
| state | String | **FALSE** | State that the candidate is located in.
| country | String | **FALSE** | Country that the candidate is located in.
| latitude | Double | **FALSE** | **Latitude** of the canddiates location.
| longitude | Double | **FALSE** | **Longitude** of the candidates location.

#### RecentJob

| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| company_name | String | **FALSE** | Company name where job was held for the candidate.
| onet | Onet | **FALSE** | ONet title associated with job.
| carotene | Carotene | **FALSE** | Carotene title associated with the job.
| unclassified | Unclassified | **FALSE** | If the job has no associated onet or carotene the service accepts an unclassfied job title sent under the unclassifed object.

#### ONet, Carotene, Unclassified
Each Job Title object only has a single title field associated as follows:

| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| title | String | **FALSE** | Title associated with classification.

#### Skill
| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| name | String | **FALSE** | Name of the skill associated with the candidate.

#### Full Request Example:
-----------

```
{
  "model_version": "1.0",
  "source": "EDGE",
  "query": {
    "search_keywords": "java",
    "location": {
      "city": "Atlanta",
      "state": "GA"
      },
    "should_use_radius": false,
    "Radius": 30,
    "extracted_keywords": [
      {
        "name": "java",
        "type": "skill",
        "weight": 100,
        "relationships": {
          "raw_job_titles_jobseeker": [
            {
              "name": "software engineer",
              "weight": 1
            }
          ],
          "raw_job_titles_recruiter": [
            {
              "name": "java developer",
              "weight": 1
            }
          ],
          "custom_keywords": [
            {
              "name": "javascript",
              "weight": 1
            }
          ],
          "related_keywords_recruiter": [
            {
              "name": "javascript",
              "weight": 1
            }
          ],
          "text_kernel_related_keywords": [
            {
              "name": "java",
              "weight": 1
            }
          ],
          "occupations": [
            {
              "name": "Computer Programmers",
              "weight": 0.87536
            }
          ],
          "job_titles": [
            {
              "name": "Computer Programmers",
              "weight": 0.87536
            }
          ],
          "related_keywords_jobseeker": [
            {
              "name": "j2ee",
              "weight": 1
            }
          ],
          "skills": [
            {
              "name": "extracted keyword skill",
              "weight": 1.0
            }
          ],
          "job_level": [
            {
              "name": "Experienced (non-Manager)",
              "weight": 1
            }
          ],
          "related_search_terms": [
            {
              "name": "magic",
              "weight": 1,
              "selected": true
            }
          ],
          "job_titles_recruiter": [],
          "job_titles_jobseeker": [],
          "interesting_keywords": []
        }
      }
    ],
    "parsed_input": {
      "input_to_extracted_keywords": {
        "nurse": "nurse"
      },
      "parsed_entities": [
        {
          "entity_name": "nurse",
          "is_phrase": true,
          "is_selected": true
        }
      ]
    },
    "filters": [
      {
        "id": "",
        "name": "Freshness",
        "type": "Range",
        "unit_of_measurement": "days",
        "values": [-1, 0]
      }
    ]
  },
  "profiles": [
    {
      "document_id": "CR3C63B5WX54M7DGSGSX",
      "recent_jobs": [
        {
          "company_name": "SOLIEL / DISA",
          "unclassified" :
            {
              "title": "Programmer"
            },
          "onet":
          	{
            	"title": "Sr. Java Developer"
          	},
          "carotene":
          	{
            	"title": "SR. Programmer"
          	}
        }
      ],
      "skills": [
        {
          "name": "JavaServer Faces"
        },
        {
          "name": "Hibernate (Java)"
        },
        {
          "name": "Java Servlet"
        },
        {
          "name": "Data/Record Logging"
        },
        {
          "name": "JavaServer Pages Standard Tag Library"
        },
        {
          "name": "Java Persistence API"
        },
        {
          "name": "Enterprise JavaBeans"
        },
        {
          "name": "Spring Framework"
        },
        {
          "name": "Java Database Connectivity"
        }
      ],
      "years_of_experience": 4.0,
      "normalized_education_level": [
        "Master's Degree"
      ],
      "unnormalized_education_level": [
        "master's of science"
      ],
      "majors": ["Computer Science"],
      "city": "Atlanta",
      "state": "GA",
      "country": "US",
      "latitude": 33.7489954,
      "longitude": -84.3879824
    }
  ]
}
```

### Response Structure
----------

A **Response** is composed of an array of `ranked_profiles`. Each profile has the original `document_id` and a newly acquired `ranker_score` from the **Reranker**. `ranked_profiles` will be listed in descending order according to their new scores.

#### RerankedProfile

| Param    | Type | Description
|----------|------|--------|
| document_id | String | Document ID associated with a candidate profile.
| score | double | Score as double value as returned from the **Reranker**.

### Full Response Example
------------

```
{
	"data" : {
		"ranked_profiles" : [
			{
				"document_id" : "CR3C63B5WX54M7DGSGSX",
				"ranker_score" : 0.79
			},
			{
				"document_id" : "CR371056LNDNDZMPWHR2",
				"ranker_score" : 0.75
			},
			{
				"document_id" : "CRJS7RB6Y4GDQ0400XMY",
				"ranker_score" : 0.66
			},
			{
				"document_id" : "CRZN2N8679L058Q0B5X6",
				"ranker_score" : 0.62
			}
		]
	}
}
```

### Versioning
---------
The current API version is 1.0. API version must be specified in the Accept header, e.g. application/json;version=1.0.
