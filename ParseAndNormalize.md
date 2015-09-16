Parse and Normalize API
====================

Table of Contents
_____________

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

Summary
===========

This service is an enhanced version of our [Resume Parsing API](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/ResumeParsing.md) that parses a Base64-encoded resume and further enriches the parsed data by issuing calls to several other classification APIs. Specifically, the service currently makes one call to our [Geography API](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/Geography.md) to normalize the candidate's location information; one call to the [Skills API](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/Skills.md) to parse skills from the complete text of the resume; and one call per work history item to the [Job Level](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/JobLevel.md), [Job Title](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/JobTitle.md), and [Skills](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/Skills.md) APIs to enrich each work history accordingly. As such, the Parse and Normalize API is highly network-intensive and necessarily slower than the Resume Parsing API alone; clients who do not need the additional classifications provided by the Parse and Normalize API are encouraged to use the Resume Parsing API instead.

The service is located at https://api.careerbuilder.com/core/parsing/normalizedresume. As usual, you will need OAuth core credentials to use this service. (*If you do not have these, please go [here](http://apitester.cbplatform.link/credentials) or email PlatformSoftware@careerbuilder.com to request core credentials.*)

Request Structure
==========

This service supports the HTTP GET and POST methods. Because Base64-encoded documents can be quite large, POST is encouraged for production use.

The following parameters may be supplied in the query string (for HTTP GET) or form body (for HTTP POST):

* **service** (Optional) -- The resume parsing service you wish to use. Accepted values are "sovren", "textkernel", and "daxtra". If this parameter is not provided, the service will automatically select the preferred parser for the document's language. [Click here for the current list of service defaults by language.](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/ResumeParsing.md#current-language-defaults)

* **document** (Required) -- A .doc, .docx, .pdf, .rtf, .txt, .odt, .wps, and .pages documents given in a BASE64 encoded string.  Please note that the .pages format is not accepted by Textkernel; you will need to specify another parser.

* **language** (Optional) -- ISO 639-1 language code. Default is "en". Because the Parse and Normalize service interacts with several of our other APIs, the only languages the service can support are those supported by every service upon which it depends. Currently the following language codes are supported: bg, de, en, es, fi, fr, hu, it, lt, lv, mt, nl, pl, pt, ro, sk. (Please let us know if your application requires support for languages not on this list.)


Response Structure
============
```
{
  "data": {
    "clean_resume_text": string,
    "raw_resume_text": string,
    "has_managed_others": boolean,
    "formatted_name": string,
    "first_name": string,
    "middle_name": string,
    "last_name": string,
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
                    "normalized_schools": [
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
          "onet15": [
            {
              "title": string,
              "id": string,
              "confidence": integer
            },
            [... more onet15 results]
          ],
          [... results for other taxonomies]
        },
        "job_level": {
          "1.0": {
            "text": string,
            "level": string,
            "description": string
          }
        },
        "skills": {
          "2.0": [
            {
              "skilldid": string,
              "normalized_term": string,
              "confidence": float
            },
            [... more skillsV2 results]
          ],
          "3.0": [
            {
              "skilldid": string,
              "normalized_term": string,
              "confidence": float
            },
            [... more skillsV3 results]
          ]
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
        "is_current_position": boolean
      }
    ],
    "skills": {
      "2.0": [
        {
          "skilldid": string,
          "normalized_term": string,
          "confidence": float
        },
        [... more skillsV2 results]
      ],
      "3.0": [
        {
          "skilldid": string,
          "normalized_term": string,
          "confidence": float
        },
        [... more skillsV3 results]
      ]
    },
    "job_level": {
      "1.0": {
        "text": string,
        "level": string,
        "description": string
      }
    },
    "job_titles": {
      "onet15": [
        {
          "title": string,
          "id": string,
          "confidence": integer
        },
        [... more onet15 results]
      ],
      [... results for other taxonomies]
    },
    "geography": {
      "1.0": [
        {
          "admin_areas": [
            {
              "long_name": string,
              "short_name": string,
              "name": string,
              "level": integer
            },
            {
              "long_name": string,
              "short_name": string,
              "name": string,
              "level": integer
            }
          ],
          "city": string,
          "country": string,
          "country_code": string,
          "landmark": string,
          "latitude": float,
          "location_type": string,
          "longitude": float,
          "postal_code": string,
          "street_address": string,
          "sublocality": string
        }
      ]
    }
  }
}
```

#Versioning
-----------
The data returned is unversioned. The current version is 1.0. We expect that each of our vendors return the same data for repeated calls, however we have not verified this systematically.  We will occasionally update our vendors which may change the output.  If we believe this change is significant we will communicate about it.  However, customers will not be able to specify vendor versions; we will not be running multiple versions of a parser.  The language to parser mapping is unversioned.  We will update it as we understand each vendor's capabilities more fully.  If you need to stay on the same parser, please specify it explicitly.

Our general versioning strategy is available [here](/Versioning.md).
