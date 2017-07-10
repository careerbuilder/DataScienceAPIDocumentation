# Resolved Title Service

**URL:** https://www.api.careerbuilder.com/core/classifier/resolvedtitle

This endpoint supports the both HTTP/GET and HTTP/POST methods.

## Request

### Request Body Example:

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
        <td><b>Name</b></td>
        <td><b>Required</b></td>
        <td><b>Comment</b></td>
    </tr>
    <tr>
        <td><pre>target_titles</pre></td>
        <td>true</td>
        <td>A pipe delimited list of possible titles to resolve to.  A single title may be supplied if the user simply wishes to test whether an input is a good match for a specified title.</td>
    </tr>
    <tr>
        <td><pre>raw_title</pre></td>
        <td>true</td>
        <td>The title of the job offer to be classified.</td>
    </tr>
        <tr>
        <td><pre>raw_description</pre></td>
        <td>false</td>
        <td>The description of the job offer, possibly empty.</td>
    </tr>
</table>

### Version Parameter:

*Resolved Title Service* is currently on version 1.0.  Version **MUST BE** specified in the Accept header (Ex. `Accept:application/json;version=1.0`).

## Response

#### Sample Response:
```json
{
    "data": {
        "title": "Network engineer",
        "confidence": 1
    }
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

