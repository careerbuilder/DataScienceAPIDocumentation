VectorService
=============

#### Contents

- [Description](#description)
- [Request](#request)
- [Request Structure](#request-structure)
- [Sample Request](#sample-request)
- [Response Structure](#response-structure)
- [Versioning](#versioning)

## Description

This service provides vector representations through two separate conversion methodologies. Firstly, the service accepts a CaroteneID and a set of skills; this vector transformation is referred to as Carotene Skills. The second transformation is called Semantic Fingerprint; Semantic Fingerprint takes a document, a job or resume, with other optional parameters and returns a vector representation.


The service is located at https://api.careerbuilder.com/core/document/vectors.  This service requires CareerBuilder OAuth 2.0 client credentials. For more information please see the authorization [documentation](/Readme.md#access).

Requests can be sent in the form of GET or POST (form or json).

## Request

A request can be broken out into two distinct types identified through a parameter called `vector_type`. Available  `vector_types` are: **carotene_skills** and **semantic_fingerprint**.

### Carotene Skills

| Parameter  | Type | Required |  Description |
|------------|------|----------|--------------|
| carotene_id | String | True | A carotene ID as provided by our Job Title Classification service which uniquely identifies a job title.
| carotene_version   | String | True | The version of Carotene that `carotene_id` is associated with. Currently the only accepted version is **CAROTENEV3**.
| skills | List | False | A list of skill objects.

Each skill object is made up of a unique number/letter identifier provided as a String.

| Parameter  | Type | Required |  Description |
|------------|------|----------|--------------|
| id | String | False | A Skill ID as provided from our Skills tagging service [here](https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/Skills.md)

#### CaroteneSkills Only Request Sample
```
{
  "carotene_id": "11.0",
  "carotene_version": "CAROTENEV3",
  "skills": [
    {
      "id": "KSH9TO"
    }
  ],
  "vector_types": ["carotene_skills"]
}

```

### Semantic Fingerprint
Semantic Fingerprint requests are processed by a deep learning model produced by Textkernel. The model can process both job and resume documents identified by the `document_type` param. If a `document` is provided the `document_type` is required and requests sent for this vector transformation will fail without it.

| Parameter  | Type | Required |  Description |
|------------|------|----------|--------------|
| document_type | String | True | The type of document being sent. Accepted values are: **job** and **resume**.
| document   | String | False | The base64 string representation of a file.
| job_title | String | False | The job title associated with the job description document being sent.
| job_requirements | String | False | The requirements associated with the job document.

#### SemanticFingerprint Only Request Sample
```
{
  "document": "amF2YQo=",
  "job_title": "Engineer",
  "job_requirements": "requirements for an engineer",
  "document_type": "job",
  "vector_types": ["semantic_fingerprint"]
}
```

## All Vector Types Sample Request
```
{
  "carotene_id": "11.0",
  "carotene_version": "CAROTENEV3",
  "skills": [
    {
      "id": "KSH9TO"
    }
  ],
  "document": "amF2YQo=",
  "job_title": "Engineer",
  "job_requirements": "requirements for an engineer",
  "document_type": "job",
  "vector_types": ["carotene_skills", "semantic_fingerprint"]
}
```


## Response Structure

All responses with an HTTP status of 200 will consist of a JSON object with a top-level "data" node. The response will contain each `vector_type` requested and an associated list of decimal numbers representing the computed vectors for the request. `vector_types` not requested will not show in the response.

### Sample Response
```
{
  "data": {
    "carotene_skills": [
      0.0636,
      -0.223,
      0.1679,
      -0.0998,
      -0.0065,
      -0.1273,
      0.1725,
      0.1435,
      -0.1266,
      0.0733,
      0.0182,
      -0.072,
      -0.0932
    ],
    "semantic_fingerprint": [
      0.0,
      0.0,
      0.0,
      0.0,
      0.1276723385424494,
      0.0,
      0.0,
      0.0,
      0.0,
      0.0,
      0.1177265279162421,
      0.0,
      0.0,
      0.0,
      0.15675720352247738
    ]
}

```

## Versioning
The current API version is 1.0. The data returned from the service is unversioned.

Our general versioning strategy is available [here](/Versioning.md).