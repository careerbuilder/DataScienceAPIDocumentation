# Job Recommendation service
# Contents

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response Structure](#response-structure)
- [Versioning](#versioning)

----------------------------
# Summary

Given a job ID and, optionally, a algorithm name and a host site, the Job Recommendation Service provides recommendations of relevant and similar jobs.

----------------------------
# Request structure
The service supports `GET` and `POST` requests to:
https://api.careerbuilder.com/consumer/recommendations/platform.

OAUTH credentials are **required** and Documentation for CareerBuilder authentication is provided [here](https://apimanagement.cbplatform.link/#/oauth/faq) by the Platform Software team.

The request parameters:

| Param    | Type | Required | Description |
|----------|------|----------|-------------|
| `jobID` | String | **True** | This needs to be a valid jobdid.
| `hostSite` | String | **False** | Hostsite for source `jobID`. Currently supports US, UK, CA. Defaults to US if omitted 
| `personalizationAlgorithm` | String | **False** | Current supported algorithms are `gbr_only`(**default**), `gbr_programmatic`, `gbr_wfh`, `gbr_scale_ten`, `gbr_mix_a`, `gbr_mix_b`, `gbr_mix_b_ranked`, `flex_rec` 


&nbsp;
### Full Request Example

```json
{
  "jobID": "J3T2QQ6CJCRBZ6JS46VS",
  "hostSite": "US",
  "personalizationAlgorithm": "gbr_only"
}
```
----------------------
# Response Structure

All responses with an HTTP status of 200 will consist of a JSON object with a top-level "data" node containing a list of `records`, which is composed of recommended job metadata and its `score`.
This example is showing an example of a programmatic job.
If this not a programmatic job, "partner_id" will be set to 0 and "is_programmatic" to false 

Example response body:
```JSON
{
    "data": {
        "records": [
            {
                "city": "Chicago",
                "state": "IL",
                "date_posted": "2022-02-22T01:06:10.0000000Z",
                "company_name": "Button Chrysler Jeep Dodge Ram",
                "expire_date": "2022-03-21T23:59:59.0000000Z",
                "id": "J4R3CJ5X89FNYFPC25K",
                "has_quick_apply": false,
                "title": "Chrysler Certified Automotive Technician",
                "score": 0.7234,
                "location": "US-IL-Chicago",
                "job_details_url": "https://api.careerbuilder.com/v1/joblink?DID=J4R3CJ5X89FNYFPC25K&recid=TR659D6723C6384E9480F9A42071B1AB69",
                "description": "  Chrysler Certified Automotive Technician ...",
                "pay": "$60,000 - $95,000/Year",
                "company_image_url": "",
                "partner_id": "PGMPTNR",
                "is_programmatic": true,
                "work_from_home": "true",
                "product_id": "JCPGM2"
            }
        ],
        "rec_id": "TR659D6723C6384E9480F9A42071B1AB69"
    }
}
```

&nbsp;

-----------
# Versioning
The current API version is 1.0. The data returned from the service is unversioned.

Our general versioning strategy is available [here](/Versioning.md).


# Job Recommendation service - JobSearch Endpoint
# Contents

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response Structure](#response-structure)

----------------------------
# Summary

Given a carotene ID and, other params such as location, talentnetworkdidcsv and job did, the Job Recommendation Service provides recommendations of relevant and similar jobs.
This service will trigger a jobsearch v4 based on the given parameters.
----------------------------
# Request structure
The service supports `GET` and `POST` requests to:
https://api.careerbuilder.com/consumer/jobs/recommendations.

OAUTH credentials are **required** and Documentation for CareerBuilder authentication is provided [here](https://apimanagement.cbplatform.link/#/oauth/faq) by the Platform Software team.

The request parameters:

| Param                 | Type   | Required   | Description                                                           |
|-----------------------|--------|------------|-----------------------------------------------------------------------|
| `carotene_id`         | String | **True**   | This needs to be a valid carotene id.                                 |
| `host_site`           | String | **True**   | Hostsite for source `jobID`. Value is "US"                            |
| `talentnetworkdidcsv` | String | **False**  | This needs to be a valid talentnetworkdidcsv                          |
| `job_did`             | String | **False**  | This needs to be a valid jobdid.                                      |
| `lat`                 | String | **False**  | This needs to be a valid latitude.                                    |
| `lng`                 | String | **False**  | This needs to be a valid longitude.                                   |
| `count_limit`         | String | **False**  | This needs to be a valid integer. count_limit range is from 1 to 100. |

&nbsp;
### Full Request Example

```json
{
  "carotene_id": "15.1272",
  "host_site": "US",
  "lat":32.81402,
  "lng":-96.94889,
  "count_limit": "10"
}
```
----------------------
# Response Structure

Example response body:
```JSON
{
  "total_pages": "2",
  "total_count": "14",
  "first_item_index": "1",
  "last_item_index": "10",
  "data": {
    "results": [
      {
        "distance": "Nearby",
        "location": "TX - Irving",
        "hostsite": "US",
        "country_code": "US",
        "job_branding_icons": "",
        "id": "J3R7TZ6DFLZ4856891B",
        "job_title": "Big Data Developer",
        "display_city": "Irving",
        "admin_area_1": "TX",
        "latitude": "32.81402",
        "longitude": "-96.94889",
        "company_name": "Mindtree",
        "company_id": "",
        "discrete_field_1": "",
        "discrete_field_2": "",
        "discrete_field_3": "",
        "discrete_field_4": "",
        "discrete_field_5": "",
        "discrete_field_6": "",
        "discrete_field_7": "",
        "discrete_field_8": "",
        "discrete_field_9": "",
        "discrete_field_10": "",
        "job_details_url": "http://swarm.careerbuilder.com/v1/joblink?DID=J3R7TZ6DFLZ4856891B&TrackingID=306B089E-C58B-4A70-BFF8-9AA0FBE593A7&HostSite=us",
        "employment_type": "Full-Time",
        "employment_type_code": "JTFT",
        "education_required": "Not Specified",
        "education_required_code": "DRNS",
        "experience_required": "Not Specified",
        "experience_required_code": "",
        "external_client_key": "10440_3153205202",
        "pay": "N/A",
        "skills": [
          "Data Pipeline"
        ],
        "street_address_1": "",
        "street_address_2": "",
        "onet15_code": "15-1031.00",
        "onet15_friendly_title": "Computer Software Engineers, Applications",
        "onet17_code": "15-1132.00",
        "onet17_friendly_title": "Software Developers, Applications",
        "carotene2p2_code": "15.1272",
        "carotene2p2_friendly_title": "Data Developer",
        "carotene3_code": "15.1272",
        "carotene3_friendly_title": "Data Developer",
        "normalized_title": "Data Developer",
        "date_posted": "2022-07-09T12:37:10-04:00",
        "job_level": "3",
        "apply_requirements": [
          "IsExternal"
        ],
        "account_did": "A7909S6JFDSX73HW6K0",
        "city": "Irving",
        "description_teaser": "Big Data Developer Irving, TX Full Time Job Description Minimum experience of 6 years working as a Bigdata developer Ability to develop data integration and transformation code pipelines in object-oriented scripting language: PySpark Python in Spark Exper",
        "display_job_id": "LI13-10440_3153205202",
        "apply_url": "https://click.appcast.io/track/enrecy0?cs=lxb&exch=4s&jg=2wis&bid=lqoIfZS7Ja_LCveufxvy9g==&ob=Xxps_NXaD8VEVqiwSSFTWg==",
        "company_image_url": "",
        "applications": "0",
        "mastercommunitylist": [
          "CMAL"
        ]
      }
    ]
  }
}
```

&nbsp;

-----------

# Job Recommendation service - Wants Endpoint
# Contents

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response Structure](#response-structure)

----------------------------
# Summary

Given a job title and, other params such as location filter and compensation filter then the Job Recommendation Service provides recommendations of relevant and similar jobs.
This service will trigger a jobsearch v1 based on the given parameters.
----------------------------
# Request structure
The service supports `GET` and `POST` requests to:
https://api.careerbuilder.com/consumer/recommendations/wants.

Version 2.0 is available and provide same response than platform endpoint
Request is the same model

OAUTH credentials are **required** and Documentation for CareerBuilder authentication is provided [here](https://apimanagement.cbplatform.link/#/oauth/faq) by the Platform Software team.

The request parameters:

| Param                | Type         | Required  | Description                                 |
|----------------------|--------------|-----------|---------------------------------------------|
| `title`              | String       | **False**  | This needs to be a valid job title.         |
| `compensationFilter` | Object       | **False** | This needs to be a valid compensationFilter |
| `hostSite`           | String       | **False**  | Hostsite for source `title`. Value is "US"  |
| `locationFilters`    | List[Object] | **False**  | This needs to be a valid locationFilter.    |

&nbsp;
### Full Request Example

```json
{
  "title":"Report Analyst",
  "hostSite":"US",
  "compensationFilter":{
    "type":"ANNUALIZED_TOTAL_AMOUNT",
    "range":{
      "min":{
        "currencyCode":"USD",
        "units":0
      },
      "max":{
        "currencyCode":"USD",
        "units":9999999
      }
    }
  },
  "locationFilters":[
    {
      "latLng":{
        "latitude":32.81402,
        "longitude":-96.94889
      },
      "distanceInMiles":50
    }
  ]
}
```
----------------------
# Response Structure V1 / Default

Example response body:
```JSON
{
  "forensics": [],
  "timing": {
    "time_received": "2022-07-19T09:02:00.000Z",
    "time_elapsed_milliseconds": 476
  },
  "data": [
    {
      "title": "Associate Report Analyst - Telecommute",
      "distance": 9,
      "score": 1.0,
      "jobDID": "J3R7WC68GS0N5J30Q1K",
      "employmentTypes": [
        "FULL_TIME"
      ],
      "description": "UnitedHealthcare is a company that's on the rise. We're expanding in multiple directions, across borders and, most of all, in the way we think. Her...",
      "applicationUrls": [
        "https://api.careerbuilder.com/v1/application/applylink?JobDID=J3R7WC68GS0N5J30Q1K&TrackingID=HY8NXZ3D&HostSite=US"
      ],
      "startDate": "2022-06-25T04:17:15.000Z",
      "endDate": "2022-03-25T00:00:00.000Z",
      "compensationInfo": {
        "type": "BASE",
        "min": {
          "units": 0,
          "nanos": ""
        },
        "max": {
          "units": 0,
          "nanos": ""
        },
        "entries": [
          {
            "type": "BASE",
            "unit": "",
            "range": {
              "min": {
                "units": 0
              },
              "max": {
                "units": 0
              }
            }
          }
        ]
      },
      "companyID": "C8E59J6ZM4TR74GZK7L",
      "companyTitle": "UnitedHealth Group",
      "jobLocations": [
        {
          "locationType": "LOCALITY",
          "postalAddress": {
            "regionCode": "US",
            "postalCode": "",
            "administrativeArea": "TX",
            "locality": "Dallas",
            "addressLines": [
              "TX - Dallas"
            ]
          },
          "latLng": {
            "latitude": 32.77666,
            "longitude": -96.79699
          },
          "radiusMeters": 12
        }
      ],
      "onet": "13-1111.00",
      "jobTypeCodes": [
        "JN010"
      ],
      "upgradeList": [
        "JCPR0"
      ],
      "applyRequirements": [
        "IsExternal"
      ],
      "displayFields": {
        "display_city": "Dallas",
        "hide_compensation": true
      },
      "logo": "https://secure.icbdr.com/MediaManagement/FF/MH17FF6DB5Q8R948GFF.jpg"
    }
  ]
}
```


# Response Structure V2

Example response body:
```JSON
{
    "data": {
        "records": [
            {
                "city": "Denton",
                "state": "TX",
                "date_posted": "2023-04-09T07:37:40.1730000Z",
                "company_name": "UNT System",
                "expire_date": "2023-06-03T11:03:38.0270000Z",
                "id": "J3M0WQ70VQQB2RGPRL2",
                "has_quick_apply": false,
                "title": "UNT WISE Senior Administrative Specialist",
                "score": 1.0,
                "location": "US-TX-Denton",
                "job_details_url": "https://wwwtest.api.careerbuilder.com/v1/joblink?DID=J3M0WQ70VQQB2RGPRL2&recid=TR46E608F79F604FEEB46562438604D051",
                "description": "&lt;b&gt;Position Details&lt;/b&gt;&lt;br /&gt;&lt;br /&gt;Position Information&lt;br /&gt;&lt;br /&gt;UNT System Overview &lt;br /&gt;&lt;br /&gt;Welcome to the University of North Texas System . UNT System includes the University of North Texas in Denton , the University of North Texas at Dallas and the University of North Texas Health Science Center in Fort Worth. We are the only university system based exclusively in the robust Dallas-Fort Worth region and we are committed to transforming lives and creating economic opportunity through education. We are growing with the DFW region, enrolling a record 47,000+ students across our system and awarding nearly 12,000 degrees each year.&lt;br /&gt;&lt;br /&gt;Posting Title &lt;br /&gt;UNT WISE Senior Administrative Specialist&lt;br /&gt;&lt;br /&gt;Department &lt;br /&gt;UNT-UNTWISE - Workplace Inclusion-135351&lt;br /&gt;&lt;br /&gt;Job Location &lt;br /&gt;Denton&lt;br /&gt;&lt;br /&gt;Full Time/Part Time &lt;br /&gt;Full-Time&lt;br ...",
                "text_pay": "N/A",
                "partner_id": "PGMPTNR",
                "is_programmatic": true,
                "work_from_home": "false",
                "product_id": "JCPGM2",
                "apply_requirements": [
                    "IsExternal"
                ],
                "job_type": "JTFT",
                "master_community_list": [
                    "CMAL",
                    "CMIN"
                ]
            }
        ],
        "rec_id": "TR46E608F79F604FEEB46562438604D051"
    }
}
```


&nbsp;

-----------
