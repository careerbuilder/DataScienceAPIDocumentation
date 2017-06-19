Semantic Search API V2 (Under Revision)
=====================

The semantic search API supports two endpoints: /query and /document.
The /query endpoint interprets and provides related entities for queries.
The /document endpoint parses a document, either a job or resume, and returns the most relevant semantic entities.

# Table of Contents
_____________

## Query Endpoint

- [Query Description](#query-description)

- [Query Request Information](#query-request-information)

- [Query Response](#query-response)

## Document Endpoint

- [Document Description](#document-description)

- [Document Request Information](#document-request-information)

- [Document Response](#document-response)

## General Information

- [Error Handling](#error-handling)

# Query Description

Interprets the meaning of a user's query. Used to parse a query into the intended phrases, to identify the type of entities being searched for (job title, skill, company, location, etc.), and to identify the most related other phrases/entities that help better represent the intended query. This service also applies stored user overrides to queries when user parameters are supplied in conjunction with the Semantic Overrides API. 


# Query Request Information


HTTP method: GET or POST
Parameters (query/form):
-	query (required) : query to parse and from which to infer meaning.
-	language (optional) : two letter language code followed by underscore, followed by two letter country code. Determines the language in which enrichments are returned. Required to use personalized overrides. Defaults to en_us. Currently, allowed values are en_us, en_gb, fr_fr, de_de, and nl_nl.
-	user_id (optional) : user id for which to look up personalized overrides. Defaults to null. 
-	relationships_threshold : the mimimum weight of a relationship entity to be added as enrichments. Can be used to prune the result size. Defaults to 0.5. Allowed values are any number between 0 and 1.
 
Example 2.0 request: 
```
HTTP GET
Accept: application/json;version=2.0
https://api.careerbuilder.com/core/semanticsearch/query?query=registered%20nurse&language=en_us&user_id=U1234
```

# Query Response

The query response is divided into two parts. First is the parsed_input node, which gives information about parsing of extracted keywords in the query (this node is missing in the document response).
Second is the extracted_keywords node, which gives the type and related entities of each extracted keyword. Enrichments can be overridden to be returned with a selected=true flag for a given user_id through the Overrides API.

2.0 response:
```
 {
	"data": {
		"parsed_input": {
			"input": "keyword",
			"parsed": "{extracted keyword}",
			"raw_parsed_input": "{keyword}",
			"input_to_extracted_keywords": {
				"keyword": "extracted keyword"
			},
			"parsed_fragments": ["extracted keyword"]
		},
		"extracted_keywords": [{
			"name": "extracted keyword",
			"weight": 100.0,
			"type": "entity type",
			"relationships": {
				"job_titles": [{
					"name": "extracted keyword job title",
					"id": "1",
					"selected": true,
					"weight": 1.0
				}],
				"occupations": [{
					"name": "extracted keyword occupation",
					"id": "99-9999.99",
					"weight": 1.0
				}],
				"related_keywords": [{
					"name": "extracted keyword related keyword",
					"weight": 1.0
				}],
				"related_keywords_recruiter": [{
					"name": "extracted keyword related keyword",
					"weight": 1.0
				}],
				"related_keywords_jobseeker": [{
					"name": "extracted keyword related keyword",
					"weight": 1.0
				}],
				"skills": [{
					"name": "extracted keyword skill",
					"selected": true,
					"weight": 1.0
				}],
				"raw_job_titles_recruiter": [{
					"name": "extracted keyword job title",
					"weight": 1.0
				}],
				"raw_job_titles_jobseeker": [{
					"name": "extracted keyword job title",
					"weight": 1.0
				}],
				"job_level": [{
					"name": "extracted keyword job level",
					"weight": 1.0
				}],
				"text_kernel_related_keywords": [{
					"name": "extracted keyword Text Kernel related keyword",
					"weight": 1.0
				}]
			}
		}],
		"versions": {
			"interesting_keywords": "InterestingTermsV2",
			"job_level": "JobLevel",
			"job_titles": "CaroteneV3",
			"occupations": "ONet17",
			"raw_job_titles_jobseeker": "RawJobTitlesJobSeekerV1",
			"raw_job_titles_recruiter": "RawJobTitlesRecruiterV1",
			"related_keywords": "RelatedSearchTermsV1",
			"related_keywords_jobseeker": "RelatedSearchTermsJobseekerV1",
			"related_keywords_recruiter": "RelatedSearchTermsRecruiterV1",
			"skills": "SkillsV4",
			"text_kernel_related_keywords": "TextKernelRelatedSearchTerms",
			"custom_keywords": "CustomKeywords"
		}
	}
}
```

# Document Description

Used to parse a job or resume into its most important features and to represent these as a prioritized list of features that can be used to form a matching query.

# Document Request Information

HTTP method: GET or POST
Parameters (query/form):
-	language (optional) : two letter language code followed by underscore, followed by two letter country code. Determines the language in which enrichments are returned. Required to use personalized overrides. Defaults to en_us. Currently, allowed values are en_us, en_gb, fr_fr, de_de, and nl_nl.
-	user_id (optional) : user id for which to look up personalized overrides. Defaults to null. 
-	relationships_threshold : the mimimum weight for a relationship entry to be added as enrichment. Can be used to prune the result size. Defaults to 0.5. Allowed values are any number between 0 and 1.
-	document : the binary document to parse as base64 encoded string (according to RFC 3548/4648).
-	type : the type of document either "JOB" or "RESUME".

Example 2.0 request:
```
HTTP GET
Accept: application/json;version=2.0
https://api.careerbuilder.com/core/semanticsearch/document
{
	"type": "JOB",
	"document": "SmF2YSBkZXZlbG9wZXIKCkNvbXBhbnk6IENhcmVlckJ1aWxkZXIKTG9jYXRpb246IEF0bGFudGEsIEdlb3JnaWEKCgpBcHBseSBub3chIFRoYW5rIHlvdSBmb3IgYXBwbHlpbmcuCg==",
	"relationships_threshold": "0.9"
}
```

# Document Response

The document response is divided into two parts. First is the parsed_input node which contains information on all extracted keywords and phrases. Next is the extracted_keywords node, which gives the name, type and relationships of all extracted phrases:

- data
	- parsed_input
		- input (comma-separated list of extracted keywords and phrases)
		- parsed_fragments (input as list of strings)
		- input_to_extracted_keywords (map of extracted strings to canonical strings)
	- extracted_keywords (list)
		- name (canonical string)
		- weight (float)
		- type (string: skill, job_title, keyword, company, school, location, company_geography)
		- entity_node (object of type-specific attributes for type: location and company_geography)
		- relationships (map of canonical string to list of relationship objects)
			- occupations (for type: job_title, skill, company, keyword)
			- related_keywords (for type: job_title, skill, company, keyword)
			- textkernel_related_keywords (for type: job_title, skill, company, keyword)
			- skills (for type: job_title, skill, company, keyword)
			- job_titles (for type: job_title, skill, company, keyword)
			- job_level (for type: job_title, skill, company, keyword)
			- education_level (for type:school)
	- versions (map of relationship string to version string)


2.0 response:
```
{
	"data" : {
		"parsed_input" : {
			"input" : "Java developer,Careerbuilder",
			"parsed_fragments" : [
				"java developer",
				"careerbuilder"
			],
			"input_to_extracted_keywords" : {
				"Careerbuilder" : "careerbuilder",
				"Java developer" : "java developer"
			}
		},
		"extracted_keywords" : [
			{
				"name" : "java developer",
				"relationships" : {
					"skills" : [
						{
							"weight" : 1,
							"name" : "Java (Programming Language)",
							"id" : "KS120076FGP5WGWYMP0F"
						},
						{
							"name" : "Hibernate (Java)",
							"id" : "KS124PR62MV42B5C9S9F",
							"weight" : 0.93154
						},
						{
							"id" : "KS123KG6DL8N3D5ZW036",
							"name" : "Java Platform Enterprise Edition (J2EE)",
							"weight" : 0.92962
						}
					],
					"job_titles" : [
						{
							"id" : "15.2",
							"name" : "Java Developer",
							"weight" : 1
						}
					],
					"job_level" : [
						{
							"name" : "Experienced (non-Manager)",
							"id" : "3",
							"weight" : 1
						}
					],
					"text_kernel_related_keywords" : [
						{
							"name" : "Java Architect",
							"weight" : 1
						},
						{
							"name" : "J2EE",
							"weight" : 1
						},
						{
							"weight" : 1,
							"name" : "Java Programmer"
						}
					],
					"occupations" : [],
					"related_keywords" : []
				},
				"type" : "job_title",
				"weight" : 100
			},
			{
				"name" : "careerbuilder",
				"type" : "company",
				"weight" : 100
				"relationships" : {},
			},
			{
				"type" : "job_level",
				"name" : "EXPERIENCED_NON_MANAGER"
			},
			{
				"type" : "location",
				"entity_node" : {
					"address" : "Atlanta, Georgia",
					"region" : "Georgia"
				}
			},
			{
				"name" : "Java developer",
				"type" : "job_title"
			}
		],
		"versions": {
			"extracted_keywords": "ExtractedKeywords",
			"job_titles": "CaroteneV3",
			"skills": "SkillsV4",
			"related_keywords": "RelatedSearchTermsV1",
			"text_kernel_related_keywords": "TextKernelRelatedSearchTerms",
			"custom_keywords": "CustomKeywords"
		}
	}
}
```
# Error Handling

On invalid input, the service returns an error message with HTTP status code 400.

Error Type | Error Message | Error Code | Cause
---------- | ------------- | ---------- | -----
Missing parameters | the document parameter cannot be null or empty | 0 | (only for /document endpoint) document parameter not specified by user or is empty.
Missing parameters | The type parameter cannot be null or empty, valid types are JOB, RESUME | 0 | Invalid or no type parameters specified by user.
EmptyRequest | Query parameter must be non-empty | 0 | (only for /query endpoint) query parameter not specified by the user or is empty.
Bad request | "Language '<lang>' is not supported. Supported languages are: EN_GB, EN_US, FR_FR, DE_DE, NL_NL. | 0 | Language specified by user is not supported.
Bad request | Invalid language '<lang>'. Valid languages are: EN_GB, EN_US, FR_FR, DE_DE, NL_NL. | 0 | Value for parameter language is invalid.
Bad request | The parameter 'relationships_threshold' must be between 0.0 and 1.0 (default: 0.5). | 0 | Value for parameter relationships_threshold is invalid.

Example error response:
```
{
  "errors": [
    {
      "type": "Missing parameters",
      "message": "The type parameter cannot be null or empty, valid types are [JOB, RESUME]",
      "code": 0
    }
  ],
  "status": "BAD_REQUEST"
}
```

