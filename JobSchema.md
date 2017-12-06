Job Schema Service
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

## Summary

Job Schema Service (JSS) takes a request containing data for a job posting and returns a response conforming to the [schema.org](﻿http://schema.org/) [JobPosting schema](﻿http://schema.org/JobPosting).

## Request Structure
```
{
  "job_data": {
    "job_display_url": string,
    "job_title": string,
    "description": string,
    "requirements": string,
    "start_date": string,
    "end_date": string,
    "employee_type": string,
    "company_data": {
      "image_url": string,
      "company_url": string,
      "company_did": string,
      "company_name": string
    },
    "industries": [
      string
    ],
    "location": {
      "city": string,
      "state": string,
      "street_address": string,
      "postal_code": string,
      "country_code": string,
      "latitude": string,
      "longitude": string
    },
    "required_education": string,
    "min_experience": number,
    "max_experience": number,
    "pay_info": {
      "currency_code": string,
      "pay_low": number,
      "pay_high": number,
      "pay_period": string,
      "pay_bonus": number,
      "pay_commission": number
    },
    "onet": {
      "code": string,
      "title": string
    },
    "display_job_id": string,
    "job_did": string,
    "talent_network_did": string
  },
  "jpan_data": {
    "experience_level": {
      "max_years": number,
      "min_years": number
    },
    "education_level": {
      "description": string
    },
    "working_hours": string,
    "extracted_text": {
      "job": {
        "text": string
      },
      "candidate": {
        "text": string
      },
      "conditions": {
        "text": string
      }
    },
    "skills": {
      "4.0": [
        {
          "normalized_term": string
        },
      ]
    }
  },
  "company_normalization_data": {
    "master_company": {
      "naics_description": string
    }
  },
  "salary_data": {
    "regional_estimate": {
      "percentiles": [
        {
          "percentile": 10,
          "annual_salary": number,
          "hourly_salary": number
        },
        {
          "percentile": 50,
          "annual_salary": number,
          "hourly_salary": number
        },
        {
          "percentile": 90,
          "annual_salary": number,
          "hourly_salary": number
        }
      ]
    }
  }
}
```

### Additional constraints:
- ```job_data.start_date``` and ```job_data.end_date``` must be in [ISO 8601](﻿https://en.wikipedia.org/wiki/ISO_8601) format with UTC offset.
    - e.g. ```2017-10-30T15:05:36.8+05:00```
- Valid values for ```job_data.employee_type``` are:
    - ```JTFT```
    - ```JTPT```
    - ```JTFP```
    - ```JTCT```
    - ```JTSE```
    - ```JTIN```
    - ```JTPD```

## Response Structure
```
{
    "data": {
        "@context": "http://schema.org",
        "@type": "JobPosting",
        "url": string,
        "title": string,
        "description": string,
        "estimatedSalary": {
            "@type": string,
            "currency": string,
            "unitText": string,
            "percentile10": number,
            "median": number,
            "percentile90": number
        },
        "industry": string,
        "datePosted": string,
        "validThrough": string,
        "employmentType": [
            string
        ],
        "hiringOrganization": {
            "@type": "Organization",
            "@id": string,
            "name": string,
            "image": string,
            "logo": string,
            "sameAs": string,
            "url": string
        },
        "jobLocation": {
            "@type": "Place",
            "address": {
                "@type": "PostalAddress",
                "addressLocality": string,
                "addressRegion": string,
                "streetAddress": string,
                "postalCode": string,
                "addressCountry": string
            },
            "geo": {
                "@type": "GeoCoordinates",
                "latitude": string,
                "longitude": string
            }
        },
        "educationRequirements": string,
        "experienceRequirements": string,
        "baseSalary": {
            "@type": "MonetaryAmount",
            "currency": string,
            "value": {
                "@type": "QuantitativeValue",
                "minValue": number,
                "maxValue": number,
                "unitText": string
            }
        },
        "occupationalCategory": [
            string
        ],
        "qualifications": string,
        "responsibilities": string,
        "jobBenefits": string,
        "skills": string,
        "workHours": string,
        "incentiveCompensation": string,
        "salaryCurrency": string,
        "identifier": {
            "@type": "PropertyValue",
            "propertyID": string,
            "value": string
        }
    }
}
```
