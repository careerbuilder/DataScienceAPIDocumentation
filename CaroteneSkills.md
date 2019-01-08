Carotene Skills
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

## Summary

The Carotene Skills service provides skills for a given carotene id. CaroteneSkills is available at 
`/core/caroteneskills`.


## Request Structure
Requests consist of:

| Field Name        | Type   |
|-------------------|--------|
|`carotene_id`      | string | 
|`carotene_version` | string |

Example cURL request:

```
curl -X POST \
  https://api.careerbuilder.com/core/caroteneskills \
  -H 'Accept: application/json;version=1.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
	"carotene_id": "11.0",
	"carotene_version": "CAROTENEV3"
      }'
```

Currently only Carotene v3 is supported. See the documentation for [Job Title service](https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/JobTitle.md)
for more information about carotene classification and carotene IDs.

## Response Structure
Response consist of a `carotene_id`, `carotene_title`, and list of `skills` for the requested 
`carotene_id`. Each item in the `skills` list contains `term`, `id`, and `description` 
strings, as well as a list of links to relevant videos.

```json
{
    "data": {
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
                        "url": "https://www.youtube.com/watch?v=CZicBw_mShE",
                        "video_title": "HOW TO INSPIRE YOUR SALES TEAM (SALES AND LEADERSHIP TRAINING INDIANAPOLIS)"
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
                        "url": "https://www.youtube.com/watch?v=OB2t8UvXMWU",
                        "video_title": "Creating a Sales and Business Development Strategy - Entrepreneurship 101 2009/10"
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
