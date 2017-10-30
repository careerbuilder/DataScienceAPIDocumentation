Job Parse and Normalize API
====================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

## Summary

The Job Parse and Normalize (JPAN) service parses a Base64-encoded job posting and further enriches the parsed data with the following normalizations and classifications.  

#### Available Enrichments:
 - __Job Title Classifications__
    - __[Carotene](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/JobTitle.md)__ <sup>(optional)</sup>
    - __[ONET](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/JobTitle.md)__ <sup>(optional)</sup>
    - __Textkernel__
 - __[Normalized Company](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/CompanyNormalization.md)__  <sup>(optional)</sup> - Normalized company data including name, website, etc.
 - __Skills Extraction__
    - __Textkernel Skills Extraction__ - Normalized skills extracted using Textkernel's extraction engine and skills taxonomy. These skills are returned by default as part of the response (regardless of desired_enrichments) and are unversioned.
    - __[CareerBuilder Skills Extraction](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/Skills.md)__ <sup>(optional)</sup> - When `skills` is requested as a `desired_enrichment`, the service will enrich the parsed job data using the CareerBuilder Skills API.
 - __Language Skills__ 
 - __[Geocoding](https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/Geocoding.md)__ <sup>(optional)</sup> - Location normalization for both company and job locations
 - __[Job Level](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/JobLevel.md)__ <sup>(optional)</sup> - Experience, or seniority level of the job posting
 - __Education Level__ - Minimum education level required by the job posting
 - __Employment Level__ - Whether the job is full-time, part-time, etc.
 - __Contract Type__ - Whether the contract is permanent, temporary, internship, etc.
 

Enrichments marked as <sup>__(optional)__</sup> must be requested using the `desired_enrichments` parameter, as they require additional calls to other services. For example, a client who only needs Job Title and Skills classifications could structure a Job Parse and Normalize request that would only retrieve these classifications. This parameter is documented in further detail below.

The service is located at https://api.careerbuilder.com/core/parsing/normalizedjob. As usual, you will need OAuth core credentials to use this service. (*If you do not have these, please go [here](http://apitester.cbplatform.link/credentials) or email PlatformSoftware@careerbuilder.com to request core credentials.*)

## Request Structure

This service supports the HTTP GET and POST methods. Because Base64-encoded documents can be quite large, POST is encouraged for production use.

The following parameters may be supplied in the query string (for HTTP GET) or form body (for HTTP POST):

* **document** (Required) -- A .doc, .docx, .pdf, .rtf, .txt, .odt, or .wps document given in a BASE64 encoded string.

* **url** (Optional) -- When parsing html, the Job Parser has custom parsing rules for [certain domains](#domains-with-custom-parsing-rules) that can improve the quality of the parse. This field allows users who are sending html to specify the url of the document and thereby improve parsing quality for domains which have custom parsing rules. Users including this field should use the full url of the document.

* **job_title** (Optional) -- The job title for the document. Overrides the job_title field in the response if supplied. The job_title_clean field in the response will be cleaned based on this supplied value as well, instead of using the job title parsed from the document.

* **location** (Optional) -- A location supplied by the user, which will be used to override job_location field in the response.

* **desired_enrichments** (Required) -- A comma-separated list, without spaces, of the desired normalization calls to perform on the results of the job parsing operation. 


    This list of possible values for **desired_enrichments** and the enrichments they correspond to are as follows:
    
    |desired_enrichments | Enrichment(s) |
    | --- | --- |
    | `company_norm` | Normalized Company Name |
    | `geocoding` | Company Geography, Job Geography |
    | `job_level` | Job Level |
    | `job_title_carotene` | Carotene Job Title Classifications |
    | `job_title_onet` | ONET Job Title Classifications |
    | `skills` | Careerbuilder Skills Extraction |

    For example, a request with a desired_enrichments value equal to `job_level,skills,job_title_onet,company_norm` would receive job level classifications, skills extractions, ONet job title classifications, and company normalizations. The API does not currently allow callers to request only certain versions of a classification service. The value `none` must be supplied to return none of the optional enrichments. 

## Response Structure
```
{
  "data": {
    "parsed": {
      "job_title": string,
      "job_title_clean": string,
      "job_location": {
        "address": string,
        "region": string,
        "country": string,
      },
      "salary": {
        "from": string,
        "to": string,
        "time_scale": string
      },
      "experience_level": {
        "min_years": integer,
        "max_years": integer,
        "level": string
      },
      "working_hours": string,
      "job_posting_date": string,
      "application_deadline": string,
      "contact_person": string,
      "company": {
        "location": {
          "address": string,
          "region": string,
          "country": string,
          "city": string
        },
        "name": string,
        "phone_numbers": [
          string,
          ...
        ],
        "fax_numbers": [
          string,
          ...
        ],
        "email_addresses": [
          string,
          ...
        ],
        "websites": [
          string,
          ...
        ]
      },
      "extracted_skills": [
        {
          "name": string,
          "desirability": string,
          "required": boolean
        }
      ],
      "extracted_text": {
        "job": {
          "confidence": double,
          "text": string
        },
        "employer": {
          "confidence": double,
          "text": string
        },
        "candidate": {
          "confidence": double,
          "text": string
        },
        "conditions": {
          "confidence": double,
          "text": string
        },
        "application": {
          "confidence": double,
          "text": string
        },
        "summary": {
          "text": string
        }
      },
      "full_text": string
    },
    "normalized": {
      "job_classifications": {
        "carotene_v3": [
          {
            "title": string,
            "id": string,
            "confidence": double
          },
          ...
        ],
        "carotene_v3.1": [
          {
            "title": string,
            "id": string,
            "confidence": double,
            "minor_title": string,
            "minor_id": string
          },
          ...
        ],
        "onet17": [
          {
            "title": string,
            "id": string,
            "confidence": integer
          },
          ...
        ],
        "textkernel": [
          {
            "classification": string,
            "code": integer,
            "group": integer,
            "profession_class": integer,
            "confidence": double
          },
          ...
        ]
      },
      "normalized_company": {
        "normalized_companies": [
          {
            "confidence": double,
            "normalized_name": string,
            "id": string,
            "naics_code": string,
            "naics_description": string,
            "duns_number": string,
            "website": string,
            "country": string,
            "state": string,
            "postal_code": string,
            "city": string,
            "address": string,
            "company_size": int
          }
        ],
        "master_company": {
          "confidence": double,
          "normalized_name": string,
          "id": string,
          "naics_code": string,
          "naics_description": string,
          "duns_number": string,
          "website": string,
          "country": string,
          "state": string,
          "postal_code": string,
          "city": string,
          "address": string,
          "company_size": int
        },
        "data_version": string
      },
      "skills": {
        "textkernel": [
          {
            "name": string,
            "skill_code": integer,
            "required": boolean,
            "skill_group": string
          },
          ...
        ],
        "4.0": [
          {
            "skilldid": string,
            "normalized_term": string,
            "confidence": string,
            "type": string
          },
          ...
        ]
      },
      "language_skills": [
        {
          "iso_code": string,
          "language": string,
          "skill_code": integer
        },
        ...
      ],
      "company_geographies": {
        "2.0": [
          {
            "admin_areas": [
              {
                "long_name": string,
                "short_name": string,
                "level": integer
              }
            ],
            "country": string,
            "country_code": string,
            "formatted_address": string,
            "latitude": double,
            "locality": string,
            "location_type": string,
            "longitude": double,
            "partial_match": true,
            "place_id": string,
            "metropolitan_statistical_area": {
              "title": string,
              "code": integer
            },
            "designated_market_areas": [
              string
            ],
            "viewport": {
              "northeast": {
                "lat": double,
                "lng": double
              },
              "southwest": {
                "lat": double,
                "lng": double
              },
              "suggested_radius_miles": double
            }
          }
        ]
      },
      "job_geographies": {
        "2.0": [
          {
            "admin_areas": [
              {
                "long_name": string,
                "short_name": string,
                "level": integer
              }
            ],
            "country": string,
            "country_code": string,
            "formatted_address": string,
            "landmark": string,
            "latitude": double,
            "location_type": string,
            "longitude": double,
            "partial_match": false,
            "place_id": string,
            "designated_market_areas": [
              string
            ],
            "viewport": {
              "northeast": {
                "lat": double,
                "lng": double
              },
              "southwest": {
                "lat": double,
                "lng": double
              },
              "suggested_radius_miles": double
            }
          }
        ]
      },
      "job_level": {
        "level": integer,
        "description": string
      },
      "education_level": {
        "level": integer,
        "description": string
      },
      "employment_level": {
        "level": integer,
        "description": string
      },
      "nonstandard_working_hours": boolean
      "contract_type": {
        "type": integer,
        "description": string
      },
      "posted_by_intermediary": boolean,
      "document_language": string
    }
  }
}
```

Note that the field ```data.normalized.nonstandard_working_hours``` will only be present if true.

## Domains With Custom Parsing Rules

Domains for which there are custom parsing rules include, but are not limited to:

- indeed.com
- truckdrivingjobs.com
- glassdoor.com
- care.com
- snagajob.com
- careerarc.com
- monster.com
- careerboard.com
- jobserve.com
- careersingear.com
- dice.com
- allnurses.com
- resume-library.com
- pure-jobs.com
- thejobnetwork.com
- mitalent.org
- findatruckerjob.com
- k12jobspot.com
- disabledperson.com
- jobing.com


## Versioning

The data returned is unversioned. The current version is 1.0. We expect that each of our vendors return the same data for repeated calls, however we have not verified this systematically.  We will occasionally update our vendors which may change the output.  If we believe this change is significant we will communicate about it. However, customers will not be able to specify vendor versions.

Our general versioning strategy is available [here](/Versioning.md).
