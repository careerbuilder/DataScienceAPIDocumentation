Semantic Search API
===================

The semantic search API supports two endpoints: /query and /document. The /query endpoint interprets and provides related entities for queries. The /document endpoint uses a document's title and content to extract relevant keywords and find related entities.

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

## General information

- [Versioning](#versioning)



#Query Description

Interprets the meaning of a user's query. Used to parse a query into the intended phrases, to identify the type of entities being searched for (job title, skill, company, location, etc.), and to identify the most related other phrases/entities that help better represent the intended query. 


#Query Request Information


HTTP method: GET or POST
Parameters (query/form):
-        query (required) : query to parse and from which to infer meaning
-        version (required) : version to use. currently supports only 0.8 
 
Example: 
```
https://api.careerbuilder.com/search/semanticsearch/query/?query=registered nurse&version=0.8
```

#Query Response

The query response is divided into two parts. First is the ParsedInput node, which gives information about parsing of extracted keywords in the query (this node is missing in the document response). Second is the extracted keywords node, which gives the type and related entities of each extracted keyword.

```
 {
  "parsed_input": {
    "input": "keyword",
    "parsed": "{extracted keyword}",
    "raw_parsed_input": "{keyword}",
    "input_to_extracted_keywords": {
      "keyword": "extracted keyword"
    }
  },
  "extracted_keywords": [
    {
      "name": "extracted keyword",
      "weight": 100.0,
      "type": "entity type",
      "relationships": {
        "job_titles": [
          {
            "name": "extracted keyword job title",
            "id": "1",
            "weight": 1.0 
          }
        ],
        "occupations": [
          {
            "name": "extracted keyword occupation",
            "id": "99-9999.99",
            "weight": 1.0
          }
        ],
        "related_keywords": [
          {
            "name": "extracted keyword related keyword",
            "weight": 1.0
          }
        ],
        "related_keywords_recruiter": [
          {
            "name": "extracted keyword related keyword",
            "weight": 1.0
          }
        ],
        "related_keywords_jobseeker": [
          {
            "name": "extracted keyword related keyword",
            "weight": 1.0
          }
        ],
        "skills": [
          {
            "name": "extracted keyword skill",
            "weight": 1.0
          }
        ],
        "raw_job_titles_recruiter": [
          {
            "name": "extracted keyword job title",
            "weight":  1.0
          }
        ],
        "raw_job_titles_jobseeker": [
          {
            "name": "extracted keyword job title",
            "weight":  1.0
          }
        ],
        "job_level": [
         {
            "name": "extracted keyword job level",
            "weight": 1.0
         }
        ],
        "text_kernel_related_keywords": [
         {
            "name": "extracted keyword Text Kernel related keyword"
            "weight": 1.0
         }
       ]
      }
    }
  ],
  "versions": {
    "job_titles": "CaroteneV3",
    "occupations": "ONet17",
    "skills": "SkillsV4",
    "interesting_keywords": "InterestingTermsV3",
    "related_keywords": "RelatedSearchTermsV1",
    "related_keywords_recruiter": "RelatedSearchTermsRecruiterV1",
    "related_keywords_jobseeker": "RelatedSearchTermsJobSeekerV1",
    "raw_job_titles_jobseeker": "RawJobTitlesJobSeekerV1",
    "raw_job_titles_recruiter": "RawJobTitlesRecruiterV1"
    "job_level": "JobLevel"
    "text_kernel_related_keywords": "TextKernelRelatedSearchTerms"
  }
}
```

#Document Description

Used to parse a job or CV/resume into its most important features and to represent these as a prioritized list of features that can be used to form a matching query.

#Document Request Information


HTTP method: GET or POST
Parameters (query/form):
-        title (required if content not provided) : the title of the document from which to infer meaning
-        content (required if title not provided) : the content of the document (job or CV/resume) from which to infer meaning 
-        version (required) : version to use. currently supports only 0.8 
 
Example: 
```
https://api.careerbuilder.com/search/semanticsearch/document/?version=0.8&title=java developer&content=
Job Description
Bachelor's degree or equivalent work experience preferred.
A minimum of 5 years of Information Technology experience.
Excellent communication and problem-solving skills
A minimum of 5 years of Java/J2EE development experience as a developer with extensive experience 
in Object Oriented Design and Development
A minimum of 5 years working with and writing SQL to query a database
A minimum of 1 year of experience with Spring
A minimum of 1 year of experience with Hibernate (annotations-based configuration)
Experience with parsing XML data into Java objects (e.g. JAXB, JAXP, SAX, DOM)

Preferred Qualifications
Java certifications a plus
Experience with the following: JSP, Servlets, JDBC, JavaScript
Experience with Ant and JUnit
Experience with Adobe Flex and ActionScript
Strong hands-on knowledge of WSDL development, including defining messages, operations, services, and 
fault handling.
Experience with XML and XSLT
Experience with RESTful web services
Experience with Struts (1 or 2)
Customer service and results-oriented while maintaining a team focus
Ability to work in a dynamic environment with cross-functional teams
Basic working experience with Unix environment and scripts&version=0.8
```

#Document Response


The document response is divided into two parts. First is the extracted keywords node, which gives the type and weight of each extracted keyword in the document, and last is the summary node (e.g. "job_title," "occupations" ....) which give the related entities of the entire document. 


```
{
	"extracted_keywords": [{
		"name": "java",
		"weight": 0.95,
		"type": "keyword",
		"relationships": {}
	}],
	"summary": {
		"job_level": [{
			"name": "Experienced (non-Manager)",
			"id": "3",
			"weight": 1
		}],
		"job_titles": [{
			"name": "Java Developer",
			"id": "15.2",
			"weight": 1
		}],
		"skills": [{
			"name": "Java (Programming Language)",
			"id": "KS120076FGP5WGWYMP0F",
			"weight": 1
		}],
		"occupations": [{
			"name": "Computer Programmers",
			"id": "15-1131.00",
			"weight": 0.98
		}]
	},
	"versions": {
		"job_titles": "CaroteneV3",
		"occupations": "ONet17",
		"skills": "SkillsV4",
		"job_level": "JobLevel",
		"extracted_keywords": "InterestingTermsV3"
	}
}
```

# Versioning

Each semantic search (query and document) response has a versions section outlining the data version for each taxonomy returned. This versioning information is stable over the life of the contract version. For example, 0.8 will always return job_titles with data version CaroteneV3. New contract versions will sometimes include contract upgrades to taxonomy versions.

Our general versioning strategy is available [here](/Versioning.md).
