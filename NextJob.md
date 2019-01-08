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
Assistant and Office Manager are common next jobs for Receptionists. Additionally it returns the 
set of skills associated with the requested carotene ID, as well as the skills the carotene ID has
in common with its next jobs and the missing skills a person should acquire to pursue the next job. 

The Next Job service is available at `/core/careerpath/nextjob`.

## Request Structure

Requests consist of:

| Field name        | Type   |
|:------------------|:-------|
|`carotene_id`      | string |
|`carotene_version` | string |
                     
Example cURL request:

```
curl -X POST \
  https://api.careerbuilder.com/core/careerpath/nextjob \
  -H 'Accept: application/json;version=1.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
	"carotene_id": "11.0",
	"carotene_version": "carotenev3"
      }'
```

Note that currently only Carotene v3 is supported. See the documentation for [Job Title service](https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/JobTitle.md)
for more information about carotene classification and carotene IDs.

## Response Structure

The response consist of a `carotene_id`, `carotene_title`, and list of `skills` for the requested 
`carotene_id`. Each item in the `skills` list contains `term`, `id` strings, and `description` 
strings as well as  list of links to relevant videos. The `next_jobs` sections is 
a list consisting of objects containing a `carotene_id` and `carotene_title` string for the next job
as well as a list of `common_skills` and list of `missing_skills`.

```json
{
    "data": {
        "carotene_id": "11.0",
        "carotene_title": "sales manager (management)",
        "skills": [
            {
                "term": "manage sale team",
                "id": "KS123X777H5WFNXQ6BPM",
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
                "common_skills": [
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
                                "url": "https://www.youtube.com/watch?v=OB2t8UvXMWU",
                                "video_title": "Creating a Sales and Business Development Strategy - Entrepreneurship 101 2009/10"
                            }
                        ]
                    },
                    {
                        "term": "negotiation",
                        "id": "KS126X663B21NB77ZHSP",
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
                        "description": "Sales are activities related to selling or the number of goods or services sold in a given time period. The seller or the provider of the goods or services complete a sale in response to an acquisition, appropriation, requisition or a direct interaction with the buyer at the point of sale.",
                        "videos": []
                    },
                    {
                        "term": "ensure customer satisfaction",
                        "id": "KS122LN6CLX3P61KWSP2",
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
The current version of the service is 1.0. 

Version must be specified in the Accept header. E.g. `application/json;version=1.0`. 

Our general versioning strategy is available [here](/Versioning.md).