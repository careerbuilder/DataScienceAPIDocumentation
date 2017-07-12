Major Normalization Service
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

Sample Response
-----

```
{
    "data": {
        "normalized_majors": [
            {
                "normalized_name": "Electrical Engineering",
                "cip_code": 15.001,
                "confidence": 4.13334
            }
        ],
        "data_version":"1.0.0"
    }
}
```


Response Information
-----

The response returns a single data node which contains a list of normalized majors and the current data version. These normalized majors are ordered by the confidence score. _At this point, Confidence scores are not normalized, and should not be taken literally. An update will release soon that will normalize confidence scores.

Versioning
-----------
The data that backs the Major Normalization Service is versioned. The data version is returned at the "data" node level.

Our general versioning strategy is available [here](/Versioning.md).
