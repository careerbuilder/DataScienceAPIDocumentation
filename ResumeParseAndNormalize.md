Resume Parse and Normalize API
====================

Table of Contents
_____________

- [Summary](#summary)
- [Languages](#languages)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [PII Scrubbing](#pii-scrubbing)
- [Versioning](#versioning)

Summary
===========

This service parses a Base64-encoded resume, then (optionally) further enriches the parsed data by issuing calls to several other classification APIs. Specifically, the service currently makes one call to our [Geocoding API](https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/Geocoding.md) to normalize the candidate's location information; one call to the [Skills API](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/Skills.md) to parse skills from the complete text of the resume; and one call per work history item to the [Job Level](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/JobLevel.md), [Job Title](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/JobTitle.md), [Company Normalization](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/CompanyNormalization.md), and [Skills](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/Skills.md) APIs to enrich each work history accordingly. These additional enrichment calls can be enabled or disabled as desired using the **desired_enrichments** parameter; for example, a client who only needs Job Title and Skills classifications could use this parameter to construct a Parse and Normalize request that would only retrieve these classifications. This parameter is documented in further detail below.

The service is located at https://api.careerbuilder.com/core/parsing/normalizedresume. As usual, you will need OAuth core credentials to use this service. (*If you do not have these, please go [here](http://apitester.cbplatform.link/credentials) or email PlatformSoftware@careerbuilder.com to request core credentials.*)

Languages
============

The resume parsing portion of this service is backed by LLM parsing software, which supports the following languages: 

| Language |
|----------|
| English              |
| Dutch                |
| French (France)      |
| French (Canada)      |
| Italian              |
| Danish               |
| Polish               |
| Russian              |
| German               |
| Romanian             |
| Czech                |
| Chinese (Simplified) |
| Spanish              |
| Greek                |
| Hungarian            |
| Swedish              |
| Portuguese           |
| Slovak               |

Note that language detection is a built-in feature of the CV parsing software. There is no need to supply information about the document's language.

Request Structure
==========

This service supports the HTTP GET and POST methods. Because Base64-encoded documents can be quite large, POST is encouraged for production use.

The following parameters may be supplied in form body (for HTTP POST):

* **document** (Required) -- A .doc, .docx, .pdf, .rtf, .txt, .odt, or .wps document given in a BASE64 encoded string. 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **NOTE:** Documents with no or few newlines will be rejected by the parser.


* **desired_enrichments** (Required) -- A comma-separated list of the desired normalization calls to perform on the results of the resume parsing operation. The list of possible values is as follows (case-insensitive): **company_norm, geocoding, job_level, job_title_carotene, job_title_onet, school_norm, skills**. For example, a request with a desired_enrichments value equal to **job_level,skills,job_title_onet,company_norm** would receive job level classifications, skills extractions, ONet job title classifications, and company normalizations. If no additional enrichments are needed, the value **"none"** may be supplied to skip all post-parsing classifications and simply return the parsed resume data.

  * Note that the **experience_months_by_job_category** field relies on job title classifications, and will only appear in the response if either the **job_title_onet** or **job_title_carotene** enrichment is requested. (This field will be computed using the most recent version of Carotene that is available, or the most recent version of ONet if no Carotene versions are available)

* **return_date_of_birth** (Optional) -- If this parameter is provided then the service will return a filled date of birth field when it is included on the document. If not provided then this will return an empty string. Note, this field was made available mainly for our international clients. It can be considered age discriminatory in the US and should only be used in special circumstances.

Response Structure
============
```
{
  "data": {
    "clean_resume_text": string,
    "raw_resume_text": string,
    "resume_html": string,
    "has_managed_others": boolean,
    "formatted_name": string,
    "first_name": string,
    "middle_name": string,
    "last_name": string,
    "date_of_birth": string,
    "affix": string,
    "email_address": string,
    "home_number": string,
    "office_number": string,
    "mobile_number": string,
    "fax_number": string,
    "pager_number": string,
    "country": string,
    "zip": string,
    "city": string,
    "state": string,
    "address_line1": string,
    "is_currently_employed": boolean,
    "most_recent_employer": string,
    "most_recent_job_title": string,
    "experience_months": integer,
    "experience_months_by_job_category": [
      {
        "job_category_code": string,
        "job_category_description": string,
        "months": integer
      },
      {
        "job_category_code": string,
        "job_category_description": string,
        "months": integer
      }
    ],
    
    "last_job_months": integer,
    "number_of_jobs": integer,
    "highest_degree_type": string,
    "languages": [
      {
        "language_code": string,
        "comments": string,
        "speak": string,
        "read": string,
        "write": string
      }
    ],
    "resume_education_histories": [
      {
        "school_normalization": {
          "school_normalization_v1": [
            {
              "normalized_school_name": string,
              "id": string,
              "country": string,
              "confidence": double
            }
          ]
        },
        "school_name": string,
        "address_type": string,
        "city": string,
        "state": string,
        "country": string,
        "degree_major": string,
        "degree_date": string,
        "degree_type": string,
        "degree_comments": string,
        "attendance_start_date": string,
        "attendance_end_date": string,
        "educational_measure_system": string,
        "measure_system_value": string,
        "measure_system_lowest": string,
        "measure_system_highest": string
      }
    ],
    "employments": [
      {
        "job_titles": {
          "onet17": [
            {
              "title": string,
              "id": string,
              "confidence": double
            },
  	    ...
          ],
	  "carotenev3.3": [
            {
              "title": string,
              "id": string,
              "confidence": double,
              "minor_title": string,
              "minor_id": string
            },
            ...
          ],
        },
        "job_level": {
          "1.0": {
            "text": string,
            "level": string,
            "description": string
          }
        },
        "skills": {
          "8.0": [
            {
              "skilldid": string,
              "normalized_term": string,
              "confidence": float,
              "type": string
            },
            ...
          ]
        },
        "company_normalization": {
          "1.0": {
            "company_depot": {
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
              "data_version": "string"
            }
          }
        },
        "city": string,
        "state": string,
        "country": string,
        "website": string,
        "job_title": string,
        "employer_name": string,
        "start_date": string,
        "end_date": string,
        "description": string,
        "job_type": string,
        "duration": integer,
        "is_current_position": boolean,
        "is_last_item": boolean,
        "experience": string,
        "full_text": string       
      }
    ],
    "skills": {
      "8.0": [
        {
          "skilldid": string,
          "normalized_term": string,
          "confidence": float
          "type": string
        },
        ...
      ]
    },
    "job_titles": {
      "onet17": [
        {
          "title": string,
          "id": string,
          "confidence": double
        },
        ...
      ],
     "carotenev3.3": [
        {
          "title": string,
          "id": string,
          "confidence": double,
          "minor_title": string,
          "minor_id": string
        },
        ...
      ],
    },
    "geography": {
      "2.0": [
        {
          "admin_areas": [
            {
              "long_name": string,
              "short_name": string,
              "level": integer
            },
            {
              "long_name": string,
              "short_name": string,
              "level": integer
            }
          ],
          "country": string,
          "country_code": string,
          "landmark": string,
          "latitude": float,
          "locality": string,
          "location_type": string,
          "longitude": float,
          "postal_code": string,
          "street_address": string,
          "sublocality": string,
          "metropolitan_statistical_area": {
            "title": string,
            "code": integer
          },
          "designated_market_areas": [
            string
          ],
          "viewport": {
            "northeast": {
              "lat": 50.74228898029149,
              "lng": 7.118058980291502
            },
            "southwest": {
              "lat": 50.7395910197085,
              "lng": 7.115361019708498
            },
            "suggested_radius_miles": 0.11033198564043084
          }
        }
      ]
    }
  }
}
```

PII Scrubbing
-----------
The service makes a best attempt at scrubbing Personally Identifiable Information (i.e. Social Security Numbers and Driver's License Numbers). PII scrubbing logic is applied to the `raw_resume_text` and `resume_html` fields only. Anything found to resemble a SSN or Driver's License Number in these fields will be replaced with either: "\*\*\*Social Security Number Removed\*\*\*" or "\*\*\*Driver's License Number Removed\*\*\*". Parsed data fields (such as the phone number fields) will therefore be unaffected by false-positive PII removals.

In addition, the service attempts to scrub Date Of Birth from the same set of fields. Text resembling a date of birth will be replaced with "\*\*\*Date of Birth Removed\*\*\*". To see how to disable this feature please review the `return_date_of_birth` field in the [Request structure](#request-structure) section.

If PII is scrubbed from the response then the field `scrubbed_sensitive_info` will be set to `true` on the response.

Versioning
-----------
The data returned is unversioned. The current version is 1.0. We expect that each of our vendors return the same data for repeated calls, however we have not verified this systematically.  We will occasionally update our vendors which may change the output.  If we believe this change is significant we will communicate about it.  However, customers will not be able to specify vendor versions; we will not be running multiple versions of a parser.

Our general versioning strategy is available [here](/Versioning.md).
