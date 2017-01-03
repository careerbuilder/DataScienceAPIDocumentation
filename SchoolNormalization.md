SchoolNormalizationService
=============

Table of Contents
_________
- [Request Information](#request-information)
- [Sample Response](#sample-response)
- [Versioning](#versioning)
- [Supported Countries](#supported-countries)



#Request Information


HTTP method: GET or POST
Parameters (query/form):
-        query (required) : The educational institution information to be queried.
-        country (optional) : 2 digit ISO country code of educational institution.
 
Example: https://api.careerbuilder.com/core/normalizedschools?query=georgia%20tech&country=us

#Sample Response


```
{
    "data": {
        "normalized_schools": [
            {
                "normalized_school_name": "Georgia Institute of Technology",
                "id": "53bff579e4b04710d09fa98d",
                "confidence": 2.0,
                "country": "US",
                "city": "Atlanta",
                "state": "GA",
                "ipeds_id": 139755",
                "ipeds_name": "Georgia Institute of Technology-Main Campus"
            }
        ]
    }
}
```


#Response Information

The response returns a single data node which contains a list of normalized schools. These normalized schools are ordered by the confidence score. Each normalized shool has a normalized school name (string), a unique ID (string), a confidence (double), a country (string), a city (string), a state (string), an ipeds_id (string), and an ipeds_name (string). Confidence scores range from 0.0 to 2.0.


#Versioning
-----------
The response from the School Normalization Service is versioned with the current version being 1.0. The data set used to perform the normalization is unversioned.

Our general versioning strategy is available [here](/Versioning.md).
