Estimated Applications Service
====================

Table of Contents
_____________

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

Summary
-----------
The Estimated Applications service returns applications numbers expected per chanel for a onet code and, optionally, a location (admin_area and locality).

Applications info can be requested in either daily,weekly,monthly numbers.

e.g.: How much does applicant for `Customer service representative` may I get per `month` in `Chicago`?

The estimated applications service is available at `/core/estimated_applications`.

Request Structure
-----------

Requests consist of:

| Parameter        | Require/Optional | Type | Description                                                                                                                                   |
|:-----------------|:-------|:-------|:----------------------------------------------------------------------------------------------------------------------------------------------|
| `onet_code`      | Yes | string | A unique id representing a onet_code job title classification https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/oNet17.md |
| `locality`       | Optional | string | An existing city.                                                                                                                             |
| `admin_area_1`   | Optional | string | A valid state.                                                                                                                                |
| `period`         | Yes | string | Possible values are "day", "week", "month".                                                                                                   |


Example cURL request : 

```
curl -X POST \
  https://api.careerbuilder.com/core/estimated_applications \
  -H 'Accept: application/json;version=1.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
      "locality": "atlanta",
      "admin_area_1": "ga",
      "onet_code": "19-1031.03",
      "period": "day"
      }'
```

Response Structure
-----------
The returned response is an expected array with application per type and channel :

We bring back response for each type of jobs :
* Internal
* External
* Programmatic

For each type we provide expect number per channel. 
If we do not have data, null is returned.
List of channels :
* ajb
* djr 
* email_aa 
* job_alert 
* post_apply 
* jdp 
* newsletter 
* email_after_apply 
* jrp 
* jobrecpage 
* colab 
* organic_search 
* talent_network 
* company_detail_pages 
* sponsored_jobs 
* ret 
* other 

daily response eg:
```json
{
    "data": {
        "internal": {
            "ajb": null,
            "djr": 2,
            "email_aa": 3,
            "job_alert": 4,
            "post_apply": 5,
            "jdp": 6,
            "newsletter": 26,
            "email_after_apply": 27,
            "jrp": 28,
            "jobrecpage": 29,
            "colab": 30,
            "organic_search": 31,
            "talent_network": 21,
            "company_detail_pages": 33,
            "sponsored_jobs": 34,
            "ret": 35,
            "other": 36
        },
        "external": {
            "ajb": 8,
            "djr": 9,
            "email_aa": 1,
            "job_alert": 2,
            "post_apply": 3,
            "jdp": 4,
            "newsletter": 46,
            "email_after_apply": 47,
            "jrp": 48,
            "jobrecpage": 49,
            "colab": 50,
            "organic_search": 51,
            "talent_network": 52,
            "company_detail_pages": 53,
            "sponsored_jobs": 54,
            "ret": 55,
            "other": 56
        },
        "programmatic": {
            "ajb": 6,
            "djr": 7,
            "email_aa": 8,
            "job_alert": 9,
            "post_apply": 1,
            "jdp": 2,
            "newsletter": 46,
            "email_after_apply": 47,
            "jrp": 48,
            "jobrecpage": 49,
            "colab": 50,
            "organic_search": 51,
            "talent_network": 52,
            "company_detail_pages": 53,
            "sponsored_jobs": 54,
            "ret": 55,
            "other": 56
        }
    }
}
```
Versioning
-----------
The response from the Estimated Application Service is versioned with the current version being 1.0. 

Our general versioning strategy is available [here](/Versioning.md).
