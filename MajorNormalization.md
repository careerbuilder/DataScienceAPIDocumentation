Major Normalization Service
=============

Table of Contents
_________
- [Request Information](#request-information)
- [Response](#response)
- [Versioning](#versioning)



Request Information
-----

HTTP method: GET or POST

Parameters (query/form):


<table>
    <tr>
    <td colspan="4"><b>Request Fields</b></td>
    </tr>
    <tr>
        <td><b>Name</b></td>
        <td><b>Required</b></td>
        <td><b>Comment</b></td>
    </tr>
    <tr>
        <td><pre>major_name</pre></td>
        <td>true</td>
        <td>A string containing the major name to be normalized</td>
    </tr>
</table>
 
Example: https://api.careerbuilder.com/core/normalizedmajor?major_name=electrical%20engineering


Response
-----

The response contains "normalized_majors" and "data_version" nodes under the "data" node. "normalized_majors" is an array of normalized major json objects and each object may contain the following fields.


| Name           | Always present | Type   |
|----------------|----------------|--------|
| normalized_name| Yes            | string | 
| cip_code       | No             | string |
| confidence     | Yes            | number |

These normalized majors are ordered by the confidence score. Confidence scores range from [0, 1].

Sample Response

```
{
    "data": {
        "normalized_majors": [
            {
                "normalized_name": "Electrical Engineering",
                "cip_code": "15.001",
                "confidence": 0.8885
            },
            {
                "normalized_name": "Double Major",
                "confidence": 0.05077937173529645
            },
        ],
        "data_version": "1.0.0"
    }
}
```


Versioning
-----------
The data that backs the Major Normalization Service is versioned. There is a "data version" field in the response that contains the current version.

Our general versioning strategy is available [here](/Versioning.md).
