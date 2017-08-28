Semantic Search API V2 
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

HTTP method: POST.

Parameters:
-	language (optional) : two letter language code followed by underscore, followed by two letter country code. Determines the language in which enrichments are returned. Required to use personalized overrides. Defaults to en_us. Currently, allowed values are en_us, en_gb, fr_fr, de_de, and nl_nl.
-	user_id (optional) : user id for which to look up personalized overrides. Defaults to null. 
-	relationships_threshold : the mimimum weight for a relationship entry to be added as enrichment. Can be used to prune the result size. Defaults to 0.5. Allowed values are any number between 0 and 1.
-	document (required) : base64 encoded string of the document to be parsed.
-	type (required) : the type of the document. Acceptable values are "JOB" and "RESUME".
-	job_title (optional, only valid when type is JOB) : a pre-defined job title which overrides the job_title field extracted from a job document. Note that the job_title field will also be cleaned.
-	location (optional, only valid when type is JOB): a pre-defined location which overrides the location extracted from a job document.

Example 2.0 request:
```
HTTP POST
Accept: application/json;version=2.0
https://api.careerbuilder.com/core/semanticsearch/document
{
	"type": "JOB",
	"document": "SmF2YSBkZXZlbG9wZXIKCkNvbXBhbnk6IENhcmVlckJ1aWxkZXIKTG9jYXRpb246IEF0bGFudGEsIEdlb3JnaWEKCgpBcHBseSBub3chIFRoYW5rIHlvdSBmb3IgYXBwbHlpbmcuCg==",
	"relationships_threshold": "0.9"
}
```

# Document Response

The document response includes the following parts. First is the `parsed_input` node which contains information on all extracted keywords and phrases. Next is the `semantic_keywords` node, which gives the name, type and relationships of all extracted phrases. Also, `job_titles`, `location`, `geography` and etc json objects may also be present. These objects are derived from parsing the job or resume document, and may not appear when the supplied document lacks corresponding information. 

- data
	- parsed_input
		- input (list): comma-separated list of extracted keywords and phrases
		- parsed_fragments (list): input as list of strings
		- input_to_extracted_keywords (map): mapping extracted strings to canonical strings
	- semantic_keywords (list)
		- name (string): canonical name
		- weight (float): estimated weight (0-100)
		- type (string): skill, job_title, keyword, company, school, location, company_geography		   
		- relationships (map of strings):
			- occupations (for type: job_title, skill, company, keyword)
			- related_keywords (for type: job_title, skill, company, keyword)
			- textkernel_related_keywords (for type: job_title, skill, company, keyword)
			- skills (for type: job_title, skill, company, keyword)
			- job_titles (for type: job_title, skill, company, keyword)
			- job_level (for type: job_title, skill, company, keyword)
			- education_level (for type:school)
	- location: possible fields are address, city, country, region, state, zip.
	
	 *(The following fields are only available when requesting a job document.)*
	- company_geography (list): contains geography objects.
	- contract_type (string): whether the contract is permanent, temporary, internship, etc.
	- education_level (string): minimum education level required by the job posting
	- employment_type (string): whether the job is full-time, part-time, etc.
	- experience_level: possible fields are name, level, min_years, max_years.
	- job_titles (list): each object contains name, source, confidence, id. 
	- job_level (string): experience, or seniority level of the job posting.
	- language_skills (list): contains a list of language skills, each language is a string type.
	
	*(The following fields are only available when requesting a resume document.)*
	- candidate_experience: possible fields are name, experience_months, experience_months_by_job_category (list) 
	- geography (list): contains geography objects.
	- highest_education_level (string): the highest education achieved.
	- job_type (string): whether the latest job is fulltime, parttime, etc.
	
2.0 response:
```
{
	"data" : {
		"parsed_input" : {
			"input" : "Java developer,Careerbuilder, ...",
			"parsed_fragments" : [
				"java developer",
				"careerbuilder",
				...
			],
			"input_to_extracted_keywords" : {
				"Careerbuilder" : "careerbuilder",
				"Java developer" : "java developer",
				...
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
			...
		],
		"location":{  
		   "address" : "Peachtree pkwy, Norcross, Georgia, United States",
		   "city" : "Norcross",
		   "region" : "Georgia",
		   "country" : "United States"
		},
		"job_titles" : [  
		   {  
		      "name" : "Client Services Manager",
		      "source" : "carotenev3.1",
		      "confidence" : 1,
		      "id" : "11.114"
		   },
		   ...
		],
		"experience_level" : {  
		   "min_years" : 10,
		   "max_years" : 0
		},
		"company_geographies" : [
		   {
			"admin_areas" : [
			  {
			    "long_name" : "Georgia",
			    "short_name" : "GA",
			    "level" : 1
			  },
			  {
			    "long_name" : "Gwinnett County",
			    "short_name" : "Gwinnett County",
			    "level": 2
			  }
			],
			"country" : "United States",
			"country_code" : "US",
			"formatted_address" : "Norcross, GA, USA",
			"latitude" : 33.9412127,
			"longitude" : -84.2135309,
			"locality" : "Norcross",
			"location_type" : "LOCALITY",
			"place_id" : "ChIJqTKlbTih9YgRVjJ2xmfcfJQ",
			"viewport" : {
			  "northeast" : {
			    "lat" : 33.9685209,
			    "lng" : -84.173478
			  },
			  "southwest" : {
			    "lat" : 33.920772,
			    "lng" : -84.23084109999999
			  },
			  "suggested_radius_miles" : 2.3295469101154485
			},
			"metropolitan_statistical_area" : {
			  "title" : "Atlanta-Sandy Springs-Roswell, GA",
			  "code" : 12060
			},
			"designated_market_areas" : [
			  "Atlanta, GA"
			],
			"partial_match" : false
		   }
		],
		"education_level" : "BACHELORS_DEGREE",
        	"employment_level" : "FULL_TIME",
        	"contract_type" : "PERMANENT",
		"job_level" : "Internship",
        	"language_skills" : ["EN", ...],
		"candidate_experience" : {
            	    "experience_months" : 8,
            	    "experience_months_by_job_category" : [
			{
			    "job_category_code" : "27",
			    "job_category_description" : "Arts, Design, Entertainment, Sports, and Media Occupations",
			    "months" : 8
			},
			...
	    	    ]
		},
		"geography" : [ *(same as company_geographies)*],
		"highest_education_level" : "Master's Degree",
		"most_recent_employment_type" : "fulltime"
	}
}
```
