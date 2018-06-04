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
`company_id` and `carotene_id` pair or `onet_id`. If both `carotene_id` and `onet_id` are supplied,
then the service first attempts to return work activities for the `carotene_id`. If none are 
found then it falls back to using the `onet_id`. Company Work Activities is available at 
`/core/company/workactivities`.

## Request Structure

Requests consist of a `company_id` string as well as at least one of `carotene_id` string or 
`onet_id` string.

```json 
{
  "company_id": "NCf01eccfd-78f5-43e5-98a1-9d6415ea4e7a",
  "carotene_id": "13.308829999999999",
  "onet_id": "53-6041.00"
}
```

## Response Structure

The response contains a list of work activities associated with the requested `company_id` and 
`carotene_id` pair or `onet_id`.

```json
{
    "data": {
        "work_activities": [
            "Assist with the administration of various employee benefits programs, including medical, dental, vision benefits",
            "Facilitate weekly new-hire benefit orientations",
            "Reconcile invoices from benefits vendors and providers"
        ]
    }
}
```
## Versioning
The current version is 1.0. 

Version must be specified in the Accept header. E.g. ```application/json;version=1.0```. 

Our general versioning strategy is available [here](/Versioning.md).