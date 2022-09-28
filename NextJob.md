Next Job
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)


## Summary

The Next Job service returns common next jobs for a given carotene ID. For example, Administrative 
Assistant and Office Manager are common next jobs for Receptionists. Additionally it returns carotene description and the 
set of skills associated with the requested carotene ID, as well as the skills the carotene ID has
in common with its next jobs and the missing skills a person should acquire to pursue the next job. 
Moreover, for each job we provide active jobs count based on location passed to the endpoint.
Salary will be provided based on the following criteria::
** if postal_code or cbsa_code is provided we will return for specific area
** if not we will provide nation wide data
You can get this salary per hour or per year.

OG data is now supported, you can pass hostite and country to query only on OG data.


The Next Job service is available at `/core/careerpath/nextjob`.

## Request Structure

Requests consist of:

| Field name        | Type   |Required|
|:------------------|:-------|:-------|
|`carotene_id`      | string |    Y   |
|`carotene_version` | string |    Y   |
|`salary_period`    | string |    Y   | // "HOUR" OR "YEAR"
|`cbsa_code`        | string |    N   |
|`postal_code`      | string |    N   |
|`locality`         | string |    N   |
|`admin_area`       | string |    N   |
|`hostsite`         | string |    N   |
|`country`          | string |    N   |
                     
Example cURL request with cbsa_code:

```
curl -X POST \
  https://api.careerbuilder.com/core/careerpath/nextjob \
  -H 'Accept: application/json;version=2.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
	"carotene_id": "11.0",
  "carotene_version": "carotenev3",
  "salary_period": "YEAR",
  "cbsa_code": "77000",
  "locality": "Chicago",
  "admin_area": "IL"
      }'
```

Example cURL request with postal_code:

```
curl -X POST \
  https://api.careerbuilder.com/core/careerpath/nextjob \
  -H 'Accept: application/json;version=2.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
	"carotene_id": "11.0",
	"carotene_version": "carotenev3",
  "salary_period": "YEAR",
  "postal_code": "60601",
  "locality": "Chicago",
  "admin_area": "IL"
      }'
```

Note that currently only Carotene v3 is supported. See the documentation for [Job Title service](https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/JobTitle.md)
for more information about carotene classification and carotene IDs.

## Response Structure

The response consist of a `carotene_id`, `carotene_title`, `carotene_description`, `salary`,`active_job_count`  and list of `skills` for the requested 
`carotene_id`. Each item in the `skills` list contains `term`, `id`, `type` strings, and `description` 
strings as well as list of links to relevant videos. The `next_jobs` sections is 
a list consisting of objects containing a `carotene_id`, `carotene_title`, `carotene_description`, `confidence`, `salary`,`active_job_count` and  string for the next job as well as a list of `common_skills` and list of `missing_skills`.

```json
{
    "data": {
        "carotene_id": "11.0",
        "carotene_title": "sales manager (management)",
        "carotene_description": "Plan, direct, or coordinate the actual distribution or movement of a product or service to the customer. Coordinate sales distribution by establishing sales territories, quotas, and goals and establish training programs for sales representatives. Analyze sales statistics gathered by staff to determine sales potential and inventory requirements and monitor the preferences of customers.",
        "salary": {
          "currency": "USD",
          "percentile_10": 21870.0,
          "percentile_25": 30660.0,
          "percentile_75": 48340.0,
          "percentile_90": 59100.0,
          "median": 38500.0,
          "mean": 39690.0,
          "employment_per_thousand_jobs": 12.311,
          "wage_percent_relative_standard_error": 1.2,
          "salary_period": "YEAR"
        },
       "active_job_count": 567,
        "skills": [
            {
                "term": "manage sale team",
                "id": "KS123X777H5WFNXQ6BPM",
                "type": "Professional Skill",
                "description": "Sales are activities related to selling or the number of goods or services sold in a given time period. The seller or the provider of the goods or services complete a sale in response to an acquisition, appropriation, requisition or a direct interaction with the buyer at the point of sale.",
                "videos": [
                    {
                        "url": "https://www.youtube.com/watch?v=Qnd8j8rxEB0",
                        "video_title": "Tips to Build and Manage an Excellent Sales Team"
                    },
                    {
                        "url": "https://www.youtube.com/watch?v=dfzozk7eGF4",
                        "video_title": "3 Key Skills for Effective Sales Management"
                    }
                ]
            },
            {
                "term": "business development",
                "id": "KS1212B6QR5SK1LSD4S4",
                "description": "Business development entails tasks and processes to develop and implement growth opportunities within and between organizations. It is a subset of the fields of business, commerce and organizational theory.",
                "videos": [
                    {
                        "url": "https://www.youtube.com/watch?v=i5Xwy-XBVgk",
                        "video_title": "The 10 Keys to Business Development"
                    },
                    {
                        "url": "https://www.youtube.com/watch?v=PEM1BY-cJN8",
                        "video_title": "Breakfast with Bakke Norman - SBA Financing presented by Jeremy Price"
                    }
                ]
            }
        ],
        "next_jobs": [
            {
                "carotene_id": "11.13",
                "carotene_title": "business development manager (management)",
                "carotene_description": "Plan, direct, or coordinate marketing policies and programs, such as determining the demand for products and services offered by a firm and its competitors, and identify potential customers. Develop pricing strategies with the goal of maximizing the firm's profits or share of the market while ensuring the firm's customers are satisfied. Oversee product development or monitor trends that indicate the need for new products and services.",
                "confidence": 0.016686638076838227,
                "salary": {
                  "currency": "USD",
                  "percentile_10": 21870.0,
                  "percentile_25": 30660.0,
                  "percentile_75": 48340.0,
                  "percentile_90": 59100.0,
                  "median": 38500.0,
                  "mean": 39690.0,
                  "employment_per_thousand_jobs": 12.311,
                  "wage_percent_relative_standard_error": 1.2,
                  "salary_period": "YEAR"
                },
                "active_job_count": 128,
                "common_skills": [
                    {
                        "term": "business development",
                        "id": "KS1212B6QR5SK1LSD4S4",
                        "type": "Professional Skill",
                        "description": "Business development entails tasks and processes to develop and implement growth opportunities within and between organizations. It is a subset of the fields of business, commerce and organizational theory.",
                        "videos": [
                            {
                                "url": "https://www.youtube.com/watch?v=i5Xwy-XBVgk",
                                "video_title": "The 10 Keys to Business Development"
                            },
                            {
                                "url": "https://www.youtube.com/watch?v=OB2t8UvXMWU",
                                "video_title": "Creating a Sales and Business Development Strategy - Entrepreneurship 101 2009/10"
                            }
                        ]
                    },
                    {
                        "term": "negotiation",
                        "id": "KS126X663B21NB77ZHSP",
                        "type": "Professional Skill",
                        "description": "Negotiation comes from the Latin neg (no) and otsia (leisure) referring to businessmen who, unlike the patricians, had no leisure time in their industriousness;  it held the meaning of business (le nÃ©goce in French) until the 17th century when it took on the diplomatic connotation as a dialogue between two or more people or parties intended to reach a beneficial outcome over one or more issues where a conflict exists with respect to at least one of these issues. Thus, negotiation is a process of combining divergent positions into a joint agreement under a decision rule of unanimity.",
                        "videos": [
                            {
                                "url": "https://www.youtube.com/watch?v=oy0MD2nsZVs",
                                "video_title": "Negotiation Skills Top 10 Tips"
                            },
                            {
                                "url": "https://www.youtube.com/watch?v=rCmvMDrCWjs",
                                "video_title": "Conducting Effective Negotiations"
                            }
                        ]
                    }
                ],
                "missing_skills": [
                    {
                        "term": "meet sales objectives",
                        "id": "KS123X777H5WFNXQ6BPM",
                        "type": "Professional Skill",
                        "description": "Sales are activities related to selling or the number of goods or services sold in a given time period. The seller or the provider of the goods or services complete a sale in response to an acquisition, appropriation, requisition or a direct interaction with the buyer at the point of sale.",
                        "videos": []
                    },
                    {
                        "term": "ensure customer satisfaction",
                        "id": "KS122LN6CLX3P61KWSP2",
                        "type": "Professional Skill",
                        "description": "Customer satisfaction (often abbreviated as CSAT, more correctly CSat) is a term frequently used in marketing. It is a measure of how products and services supplied by a company meet or surpass customer expectation.",
                        "videos": [
                            {
                                "url": "https://www.youtube.com/watch?v=LbIB4KW9aZY",
                                "video_title": "How To Serve Customers Better To Ensure Customer Satisfaction"
                            },
                            {
                                "url": "https://www.youtube.com/watch?v=XK3cNcuvuMs",
                                "video_title": "5 Steps To Improve Customer Satisfaction"
                            }
                        ]
                    }
                ]
            }
        ]
    }
}
```

## Versioning
The current version of the service is 2.0. 

Version must be specified in the Accept header. E.g. `application/json;version=2.0`. 

Our general versioning strategy is available [here](/Versioning.md).
