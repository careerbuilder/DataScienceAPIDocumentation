# Resolved Title Service

**URL:** https://www.api.careerbuilder.com/core/classifier/resolvedtitle

This endpoint supports the both HTTP/GET and HTTP/POST methods.

## Request

### 1.0 Request Body Example:

```json
{
  "target_titles": "Software Engineer|Database Administrator|Systems Administrator|Java Software Developer",
  "raw_title": "Java developer",
  "raw_description": "Come write some java code and do other stuff I guess maybe."
}
```

<table>
    <tr>
    <td colspan="4"><b>Request Fields</b></td>
    </tr>
    <tr>
        <td><b>1.0</b></td>
        <td><b>0.9</b></td>
        <td><b>Required</b></td>
        <td><b>Comment</b></td>
    </tr>
    <tr>
        <td><pre>target_titles</pre></td>
        <td><pre>targetTitles</pre></td>
        <td>true</td>
        <td>A pipe delimeted list of possible titles to resolve to.  A single title may be supplied if the user simply wishes to test whether an input is a good match for a specified title.</td>
    </tr>
    <tr>
        <td><pre>raw_title</pre></td>
        <td><pre>rawTitle</pre></td>
        <td>true</td>
        <td>The title of the job offer to be classified.</td>
    </tr>
        <tr>
        <td><pre>raw_description</pre></td>
        <td><pre>rawDescription</pre></td>
        <td>false</td>
        <td>The description of the job offer, possibly empty.</td>
    </tr>
</table>

### Version Parameter:

This API is in transition to match CareerBuilder API standards.  To provide peaceful transition, we support versions 0.9 and 1.0.  Version is passed in the Accept header as the parameter version (Ex. `Accept:application/json;version=1.0`). 

If the version is not explicitly specified in the Accept header then the service defaults to version 0.9.  We **highly encourage** new users to pass version 1.0 in the Accept header. 

In version 0.9, all input parameters and output json fields are in camel case.  In version 1.0, input parameters and output json fields are in snake case (lowercase with underscores).  Version 0.9 is scheduled for retirement by the end of Q1 2017.
    
## Response

#### 1.0 Sample Response:
```json
{
    "data": {
        "title": "Network engineer",
        "confidence": 1
    }
}
```

#### 0.9 Sample Response:
```json
{
    "title": "Network engineer",
    "confidence": 1
}
```


<table>
    <tr>
    <td colspan="4"><b>Response Fields</b></td>
    </tr>
    <tr>
        <td><b>Name</b></td>
        <td><b>Comment</b></td>
    </tr>
    <tr>
        <td><pre>title</pre></td>
        <td>The best match for rawTitle, selected from targetTitles.  If no good match is found, this field will contain the string "No good match".</td>
    </tr>
    <tr>
        <td><pre>confidence</pre></td>
        <td> The classifier's confidence factor for the assignment.  This is a normalized float value [0,1] but without any scale or reference point.  It is used for ordering matches by the classifier but should not be used for thresholding.  This field will be absent if no good match is found.</td>
</table>

## Versioning
-----------
ResolvedTitle is currently on version 1.0, and the data returned from this API is versioned.

Our general versioning strategy is available [here](/Versioning.md).

