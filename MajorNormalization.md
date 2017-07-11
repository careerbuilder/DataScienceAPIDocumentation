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
