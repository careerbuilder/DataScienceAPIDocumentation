Candidate Reranking Service V1
==============================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
    - [Query](#query)
        - [Query](#query)
        - [ParsedInput](#parsed-input)
        - [ParsedEntity](#parsedentity)
        - [Keyword](#keyword)
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
https://api.careerbuilder.com/core/search/candidatereranker
OAUTH credentials are **required**.

A request is composed of 4 main parts:
`reranker_config`, `source`, `query`, and `profiles`.

| Param    | Type | Description
|----------|------|--------|
| reranker_config | String | **Required**: Accepted Value: **RERANKER_V1** The `reranker_config` determines which learned model to apply to your incoming rerank request. Currently there is only one reranker model to call "RERANKER_V1".
| source | String | **Required**: Accepted Values: **EDGE** or **MY_SUPPLY**. The source identifies where profile data was attained. A model can be learned based on the source providing more accurate results per each data format.
| query | [Query](#query) | **Required**: The original fully enriched query sent to SOLR for the list of Candidate Profiles.
| profiles | [Profile[]](#profile) | **Required**: Array of profiles to be reranked.

### Query

----------
The Query and it's child objects are expected to be taken from the response of the [SemanticSearchAPI](https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/SemanticSearchV2.md). All the following objects mirror that response.

#### Query
| Param    | Type | Description
|----------|------|--------|
| search_keywords | String | **Optional**: Raw query string applied in original search.
| location | String | **Optional**: Location requested in search.
| should_use_radius | boolean | **Optional**: Flag on the original search to determine use of radius.
| radius | Integer | **Optional**: Radius used in original search.
| extracted_keywords | [Keyword[]](#keyword) | **Optional**: Array of **Keyword** enrichments to the query string or `search_keywords` returned from SemanticSearchAPI.
| parsed_input | ParsedInput | **Optional**: **ParsedInput** Object comprised of a Map<String,String> as `input_to_extracted_keywords` and a list of [ParsedEntities](#ParsedInput)
| filters | [Filters[]](#Filter) | **Optional**: Array of filters used in the original query to search.

#### ParsedInput
| Param    | Type | Description
|----------|------|--------|
| input_to_extracted_keywords | Map<String, String> | **Optional**: Words extracted from `search_keywords` as the **Key** and their relavtive keywords identified by SemanticSearchAPI as the **Value**.
| parsed_entities | ParsedEntity | **Optional**: Entities parsed from `search_keywords`.

#### ParsedEntity
| Param    | Type | Description
|----------|------|--------|
| entity_name | String | **Optional**: Name of the entity.
| is_phrase | Boolean | **Optional**: Flags the entity as a phrase or not.
| is_selected | Boolean | **Optional**: Flags the entity as sleceted or not.

#### Keyword
Entites returned from the SemanticSearchAPI.

| Param    | Type | Description
|----------|------|--------|
| name | String | **Optional**: Name of the keyword, e.g. "Java".
| type | String | **Optional**: Type of the keyword, e.g. "skill".
| weight | Integer | **Optional**: Weight of the keyword as an int.
| relationships | Map<String, Entities[]> | **Optional**: Map of Strings(Semantic Enrichment Type) to a list of of entities. Valid list of Map key values are: `job_titles`, `occupations`, `skills`, `job_level`, `text_kernel_related_keywords`, `interesting_keywords`, `related_keywords`, `related_keywords_recruiter`, `related_keywords_jobseeker`, `raw_job_titles_jobseeker`, `raw_job_titles_recruiter`, `extracted_kewords`, `custom_keywords`, `related_search_terms`, and `job_titles_recruiter`.

#### Entity
| Param    | Type | Description
|----------|------|--------|
| name | String | **Optional**: Name of enrichment **Entity**.
| weight | double | **Optional**: Weight of enriched term according to the SemanticSearchAPI.
| selected | boolean | **Optional**: Specific for the `related_search_terms` Semantic Enrichment. If selected the value is sent to the Reranker as a feature to be extracted for reranking else it is left out of reranking. All entites default to true.

#### Filter
| Param    | Type | Description
|----------|------|--------|
| id | String | **Optional**: Unique identfier for the **Filter**.
| name | String | **Optional**: Name of the **Filter**.
| type | String | **Optional**: Accepted values: facet or range. Type of **Filter** applied. Defaults to facet.
| values | String[] | **Optional**: Values chosen to filter on from the original search request **Filter**.

### Profile
----------
The profile is built with data from either **MY_SUPPLY** or **EDGE** (identified in the `source` param). Using data from each profile the service builds a RerankRequest Document which is sent along with features extracted from the query to the **Reranker**. The **Reranker** then returns the list of profiles in a new ordering which should be of more relevance to the query.

**IMPORTANT: Though many fields on the profile are optional its encouraged to fill out the profile as fully as possible in order to get the best results from the Reranker**.

#### Profile
| Param    | Type | Description
|----------|------|--------|
| document_id | String | **Optional**: Unique CB identifier string for the candidate.
| recent_jobs | [RecentJobs[]](#recentjob) | **Optional**: Array of **RecentJob** objects. Recent s jobs held by the candidate. Reranker requires at least one RecentJob object.
| skills | [Skill[]](#skill) | **Optional**: Array of **Skill** objects. Skills associated with the candidate.
| years_of_experience | Double | **Optional**: Total years of experience the candidate has as a double.
| normalized_education_level | String[] | **Optional**: Normalized Educations associated with the candidate i.e., Master's Degree, Bachelor's Degree etc..
| unnormalized_education_level | String[] | **Optional**: Unnormalized Educations associated with the candidate. i.e., masters degree, Master of Science, Masters etc..
| major | String | **Optional**: The major or field of study the candidate's degree pertains to.
| city | String | **Optional**: City that the candidate is located in.
| state | String | **Optional**: State that the candidate lives in.
| country | String | **Optional**: Country of the candidate.
| latitude | Double | **Optional**: Location of **Latitude** associated with the canddiates location.
| longitude | Double | **Optional**: Location of **Longitude** associated with the candidates location.

#### RecentJob
**IMPORTANT: The Reranker requires that at least one RecentJob be included on the request.**

| Param    | Type | Description
|----------|------|--------|
| company_name | String | **Optional**: Company name where job was held for the candidate.
| onet | Onet | **Optional**: ONet title associated with job.
| carotene | Carotene | **Optional**: Carotene title associated with the job.
| unclassified | Unclassified | **Optional**: Unclassified job title if the job does not have an ONet or Carotene title.

#### ONet, Carotene, Unclassified
Each Job Title object only has a single title field associated as follows:

| Param    | Type | Description
|----------|------|--------|
| title | String | **Optional**: Title associated with classification.

#### Skill
| Param    | Type | Description
|----------|------|--------|
| name | String | **Optional**: Name of the skill associated with the candidate.

#### Full Request Example:

```
{
  "reranker_config": "RERANKER_V1",
  "source": "EDGE",
  "query": {
    "search_keywords": "java",
    "location": "",
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
        "values": [0,-1]
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
              "Programmer"
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
      "years_of_exerperience": 4.0,
      "normalized_education_level": [
        "Master's Degree"
      ],
      "unnormalized_education_level": [
        "master's of science"
      ],
      "major": "Computer Science",
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

A **Response** is composed of a array of `ranked_profiles`. Each profile has the original `document_id` and a newly aquired `ranker_score` from the **Reranker**. `ranked_profiles` will be listed in descending order according to their new scores.

#### RerankedProfile

| Param    | Type | Description
|----------|------|--------|
| document_id | String | Document Id associated with a candidate profile.
| score | double | Score as double value as returned from the **Reranker**.

#### Full Response Example

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
