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
 - __[Normalized Company](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/CompanyNormalization.md)__  <sup>(optional)</sup> - Normalized company data including name, website, etc.
 - __Skills Extraction__
    - __[CareerBuilder Skills Extraction](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/Skills.md)__ <sup>(optional)</sup> - When  `skillsv8` is requested as a `desired_enrichment`, the service will enrich the parsed job data using the CareerBuilder Skills API.
 - __Language Skills__ 
 - __[Geocoding](https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/Geocoding.md)__ <sup>(optional)</sup> - Location normalization for both company and job locations
 - __[Job Level](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/JobLevel.md)__ <sup>(optional)</sup> - Experience, or seniority level of the job posting
 - __Summary__ <sup>(optional)</sup> - Summary of the job created by LLM

Enrichments marked as <sup>__(optional)__</sup> must be requested using the `desired_enrichments` parameter, as they require additional calls to other services. For example, a client who only needs Job Title and Skills classifications could structure a Job Parse and Normalize request that would only retrieve these classifications. This parameter is documented in further detail below.

The service is located at https://api.careerbuilder.com/core/parsing/normalizedjob. As usual, you will need OAuth core credentials to use this service. (*If you do not have these, please go [here](http://apitester.cbplatform.link/credentials) or email PlatformSoftware@careerbuilder.com to request core credentials.*)

## Request Structure

This service supports the HTTP GET and POST methods. Because Base64-encoded documents can be quite large, POST is encouraged for production use.

The following parameters may be supplied in the query string (for HTTP GET) or form body (for HTTP POST):

* **document** (Required) -- A .doc, .docx, .pdf, .rtf, .txt, .odt, or .wps document given in a BASE64 encoded string.

* **url** (Optional) -- When parsing html, the Job Parser has custom parsing rules for [certain domains](#domains-with-custom-parsing-rules) that can improve the quality of the parse. This field allows users who are sending html to specify the url of the document and thereby improve parsing quality for domains which have custom parsing rules. Users including this field should use the full url of the document.

* **job_title** (Optional) -- The job title for the document. _**Required if desired_enrichments JOB_LEVEL  is requested**_

* **language** (Optional) -- The language of the document, specified using language codes (e.g., "en" for English, "fr" for French).

* **Company Information Attributes**  
  * **company_name** (Optional): The name of the company associated with the job or vacancy. _**Required if Enrichment.COMPANY_NORM is requested.**_
  * **company_address** (Optional): The street address of the company.
  * **company_locality** (Optional): The locality or city where the company is located.
  * **company_admin_area** (Optional): The administrative area or region (e.g., state or province) where the company is located.
  * **company_postal_code** (Optional): The postal code of the company's address.
  * **company_country** (String): The country where the company is located.

* **Vacancy Location Attributes**
  * **vacancy_address** (Optional): The specific street address where the vacancy is based, if different from the company's main address.
  * **vacancy_locality** (Optional): The locality or city where the vacancy is located.
  * **vacancy_admin_area** (Optional): The administrative area or region of the vacancy location.
  * **vacancy_postal_code** (Optional): The postal code for the vacancy's address.
  * **vacancy_country** (Optional): The country where the vacancy is located.

**If desired_enrichments GEOCODING is requested, at least one location-related attribute (either in the company or vacancy section) must be provided.**

* **desired_enrichments** (Required) -- A comma-separated list, without spaces, of the desired normalization calls to perform on the results of the job parsing operation. 


    This list of possible values for **desired_enrichments** and the enrichments they correspond to are as follows:
    
    |desired_enrichments | Enrichment(s) |
    | --- | --- |
    | `company_norm` | Normalized Company Name |
    | `geocoding` | Company Geography, Job Geography |
    | `job_level` | Job Level |
    | `job_title_carotene` | Carotene Job Title Classifications |
    | `job_title_onet` | ONET Job Title Classifications |
    | `skillsv8` |  Careerbuilder Skills V8 Extraction |
    | `summary` |  Summary from the job created by LLM|

    For example, a request with a desired_enrichments value equal to `job_level,skillsv5,job_title_onet,company_norm` would receive job level classifications, skills V5 extractions, ONet job title classifications, and company normalizations. The API does not currently allow callers to request only certain versions of a classification service. The value `none` must be supplied to return none of the optional enrichments.

  

## Response Structure
Salary information is coming from the legacy Monster Salary parsing service. Please note that the "parsed" node will not be present if we are unable to find any salary information.

```
{
    "data": {
        "parsed": {
            "salary": {
                "from": "35000",
                "to": "38000",
                "time_scale": "ANNUAL",
                "currency": "USD"
            }
        },
        "normalized": {
            "normalized_company": {
                "normalized_companies": [
                    {
                        "confidence": "float",
                        "normalized_name": "string",
                        "id": "string",
                        "naics_code": "string",
                        "naics_description": "string",
                        "duns_number": "string",
                        "website": "string",
                        "country": "string",
                        "state": "string",
                        "postal_code": "string",
                        "city": "string",
                        "address": "string",
                        "company_size": "integer",
                        "is_staffing_company": "boolean"
                    }
                ],
                "master_company": {
                    "confidence": "float",
                    "normalized_name": "string",
                    "id": "string",
                    "naics_code": "string",
                    "naics_description": "string",
                    "duns_number": "string",
                    "website": "string",
                    "country": "string",
                    "state": "string",
                    "postal_code": "string",
                    "city": "string",
                    "address": "string",
                    "company_size": "integer",
                    "is_staffing_company": "boolean"
                },
                "data_version": "string"
            },
            "skills": {
                "8.0": [
                    {
                        "type": "string",
                        "skilldid": "string",
                        "normalized_term": "string",
                        "extracted_term": "string",
                        "confidence": "float"
                    }
                ]
            },
            "summary": "string",
            "company_geographies": {
                "2.0": [
                    {
                        "admin_areas": [
                            {
                                "long_name": "string",
                                "short_name": "string",
                                "level": "integer"
                            }
                        ],
                        "country": "string",
                        "country_code": "string",
                        "formatted_address": "string",
                        "latitude": "float",
                        "locality": "string",
                        "location_type": "string",
                        "longitude": "float",
                        "partial_match": "boolean",
                        "place_id": "string",
                        "postal_code": "string",
                        "street_address": "string",
                        "metropolitan_statistical_area": {
                            "title": "string",
                            "code": "integer"
                        },
                        "viewport": {
                            "northeast": {
                                "lat": "float",
                                "lng": "float"
                            },
                            "southwest": {
                                "lat": "float",
                                "lng": "float"
                            },
                            "suggested_radius_miles": "float"
                        },
                        "designated_market_areas": ["string"]
                    }
                ]
            },
            "job_geographies": {
                "2.0": [
                    {
                        "admin_areas": [
                            {
                                "long_name": "string",
                                "short_name": "string",
                                "level": "integer"
                            }
                        ],
                        "country": "string",
                        "country_code": "string",
                        "formatted_address": "string",
                        "latitude": "float",
                        "locality": "string",
                        "location_type": "string",
                        "longitude": "float",
                        "partial_match": "boolean",
                        "place_id": "string",
                        "postal_code": "string",
                        "street_address": "string",
                        "metropolitan_statistical_area": {
                            "title": "string",
                            "code": "integer"
                        },
                        "viewport": {
                            "northeast": {
                                "lat": "float",
                                "lng": "float"
                            },
                            "southwest": {
                                "lat": "float",
                                "lng": "float"
                            },
                            "suggested_radius_miles": "float"
                        },
                        "designated_market_areas": ["string"]
                    }
                ]
            },
            "document_language": "string"
        },
        "description": "string",
        "score": "float",
        "score_bucket": "string",
        "quality_check_list": {
            "is_spam": "boolean",
            "is_for_multiple_roles": "boolean",
            "has_misleading_content": "boolean",
            "is_less_than400_characters": "boolean",
            "is_job_info_incomplete": "boolean",
            "does_title_not_match_description": "boolean",
            "poor_quality": {
                "is_poor_quality": "boolean",
                "reason": {
                    "is_job_info_incomplete": "boolean",
                    "does_title_not_match_desc": "boolean"
                }
            }
        },
        "is_spam": "boolean"
    }
}
```
Job score is based on the quality of the job title and description:

1. **Title Score (up to 50 points)**:
   - Must be under 100 characters.
   - Must be well-formatted (not fully capitalized and free of undesirable characters).
   - Must be properly classified with sufficient confidence.

2. **Description Score (up to 50 points)**:
   - Must include at least two sentences.
   - Length must be between 2500-5500 characters.
   - Must meet formatting standards (not fully capitalized and free of undesirable characters).

3. **Total Score and Bucketing**:
   - The combined score ranges from 0 to 100.
   - Categorized into buckets based on score:
     - `VERY_LOW`: 0 points or flagged as spam
     - `LOW`: 1-30 points
     - `MEDIUM`: 31-70 points
     - `GOOD`: 71-100 points

This scoring method evaluates both the title and description to determine overall job quality.


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
