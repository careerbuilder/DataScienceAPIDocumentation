Company Job Description
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Caching](#Caching)
- [Versioning](#versioning)

## Summary

The Company Job Description provides the list of jobs and the job details a company is offering.
Company Job Description is available at `/core/company/jobdescriptions`.


## Request Structure
Requests consist of  `company_id` string:
                     `carotene_id` string:
                     `carotene_version` string:
                     `city` string:
                     `state` string:
                     `country` string:
                     

```json
{  
   "company_id":"NC0542a8a1-7810-4c6e-80db-d748f41ec521",
   "carotene_id":"51.18",
   "carotene_version":"caroteneV3_1",
   "city":"New Iberia",
   "state":"LA",
   "country":"US"
}
```



## Response Structure
Response consist of a list of `job_descriptions` where each item contains a `id` string which is job id and `description` string which is description for that particular job.

```json
{
  "data": {
    "job_descriptions": [
      {
        "id": "JD700fe7a9-ef66-3800-9f4b-212d831976c8",
        "description": "Inspect and maintain electric welding equipment, including checking leads"
      },
      {
        "id": "JD4f29e519-bfa7-37dc-a02f-aec49b5bb6d2",
        "description": "Checking for porosity, slag traps, and undercut, checking and adjusting component alignment, and monitoring and adjusting welding machine temperature and polarity"
      },
      {
        "id": "JD0c729f1f-93b0-35fd-a1c4-20e88c47923b",
        "description": "Remove, replace, and connect oxygen and acetylene bottles"
      },
      {
        "id": "JD1d014c76-4de1-3006-86aa-623e7c0fa4e6",
        "description": "Assemble and weld pipe components into pipe spools or completed pipe assemblies"
      },
      {
        "id": "JD3b3feda5-89c5-379f-b7d6-16b7d0782921",
        "description": "Checking scaffolding, welding tent, and availability of a fire watch and fire extinguishing equipment"
      },
      {
        "id": "JD465e88c7-6d72-3fa3-a43b-23a027409deb",
        "description": "Using GTAW (TIG) and SMAW (Stick Rod) welding procedures"
      },
      {
        "id": "JDe84dae36-0b7e-3a7e-8416-5d89ef3cbe76",
        "description": "Inspect and maintain gas equipment, including changing gauges, repairing hoses"
      }
    ]
  }
}
```


##Caching
Company Job Description uses 2 types of caching. 
1) Memory    : Primary Caching
2) DynamoDB  : Secondary Caching


## Versioning
The current version of the service is 1.0. 

Version must be specified in the Accept header. E.g. `application/json;version=1.0`. 

Our general versioning strategy is available [here](/Versioning.md).
