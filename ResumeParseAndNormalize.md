Resume Parse and Normalize API
====================

Table of Contents
_____________

- [Summary](#summary)
- [Languages](#languages)
- [Parser Default](#parser-default)
  - [Request structure default](#request-structure-default)
  - [Response structure default](#response-structure-default)
  - [PII Scrubbing](#pii-scrubbing)
- [Parser OpenAI Monster TK](#parser-openai-monster-tk)
  - [Request structure TK](#request-structure-tk)
  - [Response structure TK](#response-structure-tk)
- [Versioning](#versioning)

Summary
===========

This service parses a Base64-encoded resume, then (optionally) further enriches the parsed data by issuing calls to several other classification APIs. Specifically, the service currently makes one call to our [Geocoding API](https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/Geocoding.md) to normalize the candidate's location information; one call to the [Skills API](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/Skills.md) to parse skills from the complete text of the resume; and one call per work history item to the [Job Level](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/JobLevel.md), [Job Title](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/JobTitle.md), [Company Normalization](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/CompanyNormalization.md), and [Skills](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/Skills.md) APIs to enrich each work history accordingly. These additional enrichment calls can be enabled or disabled as desired using the **desired_enrichments** parameter; for example, a client who only needs Job Title and Skills classifications could use this parameter to construct a Parse and Normalize request that would only retrieve these classifications. This parameter is documented in further detail below.

The service is located at https://internal.api.careerbuilder.com/core/parsing/normalizedresume. As usual, you will need OAuth core credentials to use this service. (*If you do not have these, please go [here](http://apitester.cbplatform.link/credentials) or email PlatformSoftware@careerbuilder.com to request core credentials.*)

**Key Features:**
- Configurable enrichments via `desired_enrichments`
- Automatic language detection
- Two distinct parser types with different response structures

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

Parser Default
==============

###Request Structure Default

This service supports the HTTP GET and POST methods. Because Base64-encoded documents can be quite large, POST is encouraged for production use.

The following parameters may be supplied in form body (for HTTP POST):

* **document** (Required) -- A .doc, .docx, .pdf, .rtf, .txt, .odt, or .wps document given in a BASE64 encoded string. 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **NOTE:** Documents with no or few newlines will be rejected by the parser.


* **desired_enrichments** (Required) -- A comma-separated list of the desired normalization calls to perform on the results of the resume parsing operation. The list of possible values is as follows (case-insensitive): **company_norm, geocoding, job_level, job_title_carotene, job_title_onet, school_norm, skills**. For example, a request with a desired_enrichments value equal to **job_level,skills,job_title_onet,company_norm** would receive job level classifications, skills extractions, ONet job title classifications, and company normalizations. If no additional enrichments are needed, the value **"none"** may be supplied to skip all post-parsing classifications and simply return the parsed resume data.

  * Note that the **experience_months_by_job_category** field relies on job title classifications, and will only appear in the response if either the **job_title_onet** or **job_title_carotene** enrichment is requested. (This field will be computed using the most recent version of Carotene that is available, or the most recent version of ONet if no Carotene versions are available)

* **return_date_of_birth** (Optional) -- If this parameter is provided then the service will return a filled date of birth field when it is included on the document. If not provided then this will return an empty string. Note, this field was made available mainly for our international clients. It can be considered age discriminatory in the US and should only be used in special circumstances.

###Response Structure Default
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

Parser OPENAI MONSTER TK
========================
###Request Structure TK

The following parameters may be supplied in form body (for HTTP POST):

* **document** (Required) -- A .doc, .docx, .pdf, .rtf, .txt, .odt, or .wps document given in a BASE64 encoded string.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **NOTE:** Documents with no or few newlines will be rejected by the parser.

* **desired_enrichments** (Required) -- **None**

* **parser**(Required)   -- **OPENAI_MONSTER_TK**
```
curl --location 'https://internal.api.careerbuilder.com/core/parsing/normalizedresume' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer token' \
--data '{"document":"documentbase64",
    "desired_enrichments": "none",
    "force_refresh": true,
    "parser": "OPENAI_MONSTER_TK"
}'
```

###Response Structure TK
```
{
    "Value": {
        "ResumeData": {
            "ContactInformation": {
                "CandidateName": {
                    "FormattedName": "Rose Blane",
                    "GivenName": "Rose",
                    "FamilyName": "Blane"
                },
                "Telephones": [
                    {
                        "Raw": "410 500 8400",
                        "Normalized": "+1-410-500-8400"
                    }
                ],
                "EmailAddresses": [
                    "caregiver@gmail.com"
                ],
                "Location": {
                    "CountryCode": "US",
                    "PostalCode": "20902",
                    "Regions": [
                        "MD"
                    ],
                    "Municipality": "Silver Spring",
                    "StreetAddressLines": [
                        "11 Monticello Ave"
                    ]
                },
                "WebAddresses": []
            },
            "ProfessionalSummary": "Registered Nurse, with 8 years of experience treating patients in a wide variety of clinical settings, seeking case management position",
            "Education": {
                "EducationDetails": [
                    {
                        "Text": "Master of Science, Clinical Nurse Leadership, University of Maryland, Baltimore, MD December 2022– December 2024 (GPA: 3.8/4.0)",
                        "SchoolName": {
                            "Raw": "University of Maryland",
                            "Normalized": ""
                        },
                        "SchoolType": "university",
                        "Location": {
                            "CountryCode": "US",
                            "Regions": [
                                "MD"
                            ],
                            "Municipality": "Baltimore"
                        },
                        "Degree": {
                            "Name": {
                                "Raw": "Master of Science, Clinical Nurse Leadership"
                            },
                            "Type": "masters"
                        },
                        "Majors": [
                            "Clinical Nurse Leadership"
                        ],
                        "StartDate": {
                            "Date": "2022-12-01",
                            "FoundYear": true,
                            "FoundMonth": true
                        },
                        "EndDate": {
                            "Date": "2024-12-01",
                            "FoundYear": true,
                            "FoundMonth": true
                        }
                    },
                    {
                        "Text": "Bachelor of Arts, History, Yeshiva University, New York City, NY May 2003 (GPA: 3.7/4.0)",
                        "SchoolName": {
                            "Raw": "Yeshiva University",
                            "Normalized": ""
                        },
                        "SchoolType": "university",
                        "Location": {
                            "CountryCode": "US",
                            "Regions": [
                                "NY"
                            ],
                            "Municipality": "New York City"
                        },
                        "Degree": {
                            "Name": {
                                "Raw": "Bachelor of Arts, History"
                            },
                            "Type": "bachelors"
                        },
                        "Majors": [
                            "History"
                        ],
                        "EndDate": {
                            "Date": "2003-05-01",
                            "FoundYear": true,
                            "FoundMonth": true
                        }
                    }
                ]
            },
            "EmploymentHistory": {
                "Positions": [
                    {
                        "Employer": {
                            "Name": {
                                "Raw": "Washington Adventist Hospital",
                                "Normalized": ""
                            },
                            "Location": {
                                "CountryCode": "US",
                                "Regions": [
                                    "MD"
                                ],
                                "Municipality": "Takoma Park"
                            }
                        },
                        "IsCurrent": true,
                        "JobTitle": {
                            "Raw": "Registered Nurse, Float Pool",
                            "Normalized": ""
                        },
                        "StartDate": {
                            "Date": "2010-04-01",
                            "FoundYear": true,
                            "FoundMonth": true
                        },
                        "EndDate": {
                            "Date": "2025-05-07",
                            "FoundYear": false,
                            "FoundMonth": false
                        },
                        "Description": "*\tProvide compassionate, efficient, and highly competent care to an extremely diverse patient population, including Medical-Surgical, Cardiac/ Telemetry, Post-Partum, Behavioral Health and Post Anesthesia patients\n*\tInitiate communication with medical and allied health care staff regarding changes in patient acuity and plan of care\n*\tEnsure the safe and timely discharge of patients from the hospital by participating in daily multi- disciplinary rounds with case management, social workers and practitioners\n*\tMentor and supervise newly hired nursing staff as well as experienced nurses trained for lower acuity populations\n*\tTriage and assess post-operative patients via telephone, referring high risk patients to physician for follow up\n*\tRated as “Exceeding Expectations” and  “demonstrating very high level performance in all areas of responsibility,” in supervisor’s evaluation for 2016\n"
                    }
                ]
            },
            "Skills": {
                "Raw": [
                    {
                        "Name": "Medical-Surgical",
                        "MonthsExperience": {
                            "Value": "182"
                        },
                        "LastUsed": {
                            "Value": "2025-05-07"
                        }
                    },
                    {
                        "Name": "Cardiac/ Telemetry",
                        "MonthsExperience": {
                            "Value": "182"
                        },
                        "LastUsed": {
                            "Value": "2025-05-07"
                        }
                    },
                    {
                        "Name": "Post-Partum",
                        "MonthsExperience": {
                            "Value": "182"
                        },
                        "LastUsed": {
                            "Value": "2025-05-07"
                        }
                    },
                    {
                        "Name": "Behavioral Health",
                        "MonthsExperience": {
                            "Value": "182"
                        },
                        "LastUsed": {
                            "Value": "2025-05-07"
                        }
                    },
                    {
                        "Name": "Post Anesthesia",
                        "MonthsExperience": {
                            "Value": "182"
                        },
                        "LastUsed": {
                            "Value": "2025-05-07"
                        }
                    },
                    {
                        "Name": "patient acuity",
                        "MonthsExperience": {
                            "Value": "182"
                        },
                        "LastUsed": {
                            "Value": "2025-05-07"
                        }
                    },
                    {
                        "Name": "plan of care",
                        "MonthsExperience": {
                            "Value": "182"
                        },
                        "LastUsed": {
                            "Value": "2025-05-07"
                        }
                    },
                    {
                        "Name": "case management",
                        "MonthsExperience": {
                            "Value": "182"
                        },
                        "LastUsed": {
                            "Value": "2025-05-07"
                        }
                    },
                    {
                        "Name": "social workers",
                        "MonthsExperience": {
                            "Value": "182"
                        },
                        "LastUsed": {
                            "Value": "2025-05-07"
                        }
                    },
                    {
                        "Name": "nursing staff",
                        "MonthsExperience": {
                            "Value": "182"
                        },
                        "LastUsed": {
                            "Value": "2025-05-07"
                        }
                    }
                ]
            },
            "Certifications": [
                {
                    "Name": "Maryland Registered Nurse License # R185223"
                },
                {
                    "Name": "ACLS and PALS certified"
                }
            ],
            "Licences": [
                {
                    "Name": "Maryland Registered Nurse License # R185223"
                },
                {
                    "Name": "ACLS and PALS certified"
                }
            ],
            "LanguageCompetencies": [
                {
                    "LanguageCode": "en"
                }
            ],
            "Achievements": [
                "Rated as “Exceeding Expectations” and  “demonstrating very high level performance in all areas of responsibility,” in supervisor’s evaluation for 2016"
            ],
            "References": [],
            "ResumeMetadata": {
                "ReservedData": {
                    "Phones": [
                        "410 500 8400"
                    ],
                    "Names": [
                        "Rose Blane"
                    ],
                    "EmailAddresses": [
                        "caregiver@gmail.com"
                    ],
                    "Urls": []
                },
                "PlainText": "\n\n\nRose Blane \n11 Monticello Ave\nSilver Spring, MD 20902\n410 500 8400\ncaregiver@gmail.com\n\nSUMMARY:  Registered Nurse, with 8 years of experience treating patients in a wide variety of clinical settings, seeking case management position .\n \nWORK EXPERIENCE \n \nRegistered Nurse, Washington Adventist Hospital, Float Pool, Takoma Park, MD \nApril 2010-present\n· Provide compassionate, efficient, and highly competent care to an extremely diverse patient population, including Medical-Surgical, Cardiac/ Telemetry, Post-Partum, Behavioral Health and Post Anesthesia patients\n· Initiate communication with medical and allied health care staff regarding changes in patient acuity and plan of care\n· Ensure the safe and timely discharge of patients from the hospital by participating in daily multi- disciplinary rounds with case management, social workers and practitioners\n· Mentor and supervise newly hired nursing staff as well as experienced nurses trained for lower acuity populations\n· Triage and assess post-operative patients via telephone, referring high risk patients to physician for follow up \n· Rated as “Exceeding Expectations” and  “demonstrating very high level performance in all areas of responsibility,” in supervisor’s evaluation for 2016\n \nRegistered Nurse, Washington Adventist Hospital, Intermediate Care Unit, Takoma Park, MD\nApril 2009-April 2010\n· Provided safe and effective care to critically ill patients, including post tracheostomy and ventilator dependent patients\n· Provided patient and family education regarding disease progression, infection prevention, medication regimen, and detailed discharge instructions\n\nSKILLS\n· Excellent verbal and written communication skills\n· Highly computer literate, including expertise in Cerner electronic medical records, Microsoft Office products and Windows operating systems\n\nEDUCATION \n \nMaster of Science, Clinical Nurse Leadership, University of Maryland, Baltimore, MD December 2022– December 2024 (GPA: 3.8/4.0)\n\t\nBachelor of Arts, History, Yeshiva University, New York City, NY \nMay 2003 (GPA: 3.7/4.0) \n\nLICENSES \n \nMaryland Registered Nurse License # R185223\nACLS and PALS certified\n\n\n\n"
            }
        }
    }
}
```


Versioning
-----------
The data returned is unversioned. The current version is 1.0. We expect that each of our vendors return the same data for repeated calls, however we have not verified this systematically.  We will occasionally update our vendors which may change the output.  If we believe this change is significant we will communicate about it.  However, customers will not be able to specify vendor versions; we will not be running multiple versions of a parser.

Our general versioning strategy is available [here](/Versioning.md).
