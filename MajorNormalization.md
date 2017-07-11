MajorNormalizationService
=============

Table of Contents
_________
- [Request Information](#request-information)
- [Sample Response](#sample-response)
- [Versioning](#versioning)



Request Information
-----

HTTP method: GET or POST
Parameters (query/form):
      major_name (required) : The major name to be normalized
 
Example: https://api.careerbuilder.com/core/normalizedmajor?major_name=electrical engineering

Sample Response
-----

```
{
    "data": {
        "normalized_majors": [
            {
                "normalized_name": "Electrical Engineering",
                "cip_code": 15.001,
                "confidence": 1.0,
            }
        ]
    }
}
```


Response Information
-----

The response returns a single data node which contains a list of normalized majors. These normalized majors are ordered by the confidence score. Confidence scores range from 0.0 to 1.0. 

Versioning
-----------
The response from the Major Normalization Service is versioned with the current version being 1.0. The data set used to perform the normalization is unversioned.

Our general versioning strategy is available [here](/Versioning.md).
