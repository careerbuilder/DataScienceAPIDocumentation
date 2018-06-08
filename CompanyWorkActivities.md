Company Work Activities
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

## Summary

The Company Work Activities service provides a list of work activities associated with a 
`company_id` and `carotene_id` or `onet_id`. Company Work Activities is available at 
`/core/company/workactivities`.

## Request Structure

Requests consist of a `company_id` string, at least one of `carotene_id` string or 
`onet_id` string, and a `carotene_version` string (required if `carotene_id` is present) and 
`onet_version` string (required if `onet_id` is present).

```json 
{
  "company_id": "NCf01eccfd-78f5-43e5-98a1-9d6415ea4e7a",
  "carotene_id": "51.31",
  "carotene_version": "carotenev3",
  "onet_id": "53-6041.00",
  "onet_version": "onet17"
}

```

Available taxonomies are `onet17` for `onet_version` and `carotenev3` for `carotene_version`.

## Response Structure

The response contains a list of work activities associated with the requested `company_id` and 
one or both of `carotene_id` and `onet_id`.

```json
{
    "data": {
        "onet_work_activities": [
            {
                "activity": "Review work orders or schedules to determine operations or procedures."
            },
            {
                "activity": "Develop program goals or plans."
            },
            {
                "activity": "Monitor surroundings to detect potential hazards."
            }
        ],
        "carotene_work_activities": [
            {
                "activity": "Specialize in skilled trades"
            },
            {
                "activity": "Staff through aerotek commercial staffing, a division of aerotek"
            },
            {
                "activity": "Recruit and screens professionals in areas cnc machining"
            }
        ]
    }
}
```
## Versioning
The current version of the service is 1.0. 

Currently ONet17 and Carotene v3 classifications are available.

Version must be specified in the Accept header. E.g. ```application/json;version=1.0```. 

Our general versioning strategy is available [here](/Versioning.md).