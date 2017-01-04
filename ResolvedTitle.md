

**URL:** https://www.api.careerbuilder.com/core/classifier/resolvedtitle

This endpoint supports the both HTTP/GET and HTTP/POST methods.

**Parameters**
+(This API is in transition to match CareerBuilder API standards. To provide peaceful transition, we support versions 0.9 and 1.0. Version is passed in the Accept header as the parameter version (Ex. Accept:application/json;version=1.0). 0.9 is the default version if user does not specify explicitly and all our existing users use this version by default. We highly encourage new users to pass version 1.0 in the Accept header. In version 0.9, all input parameters and output json fields are in camel case, while in version 1.0, input parameters and output json fields are in lowercase with underscores. Version 0.9 is scheduled for retirement by the end of Q1 2017.)

**targetTitles** (required, **target_titles** in version 1.0): A string containing two or more pipe-delimited job titles to test the input against. For example: Software Engineer|Database Administrator|Systems Administrator. A single title may be supplied if the user simply wishes to test whether an input is a good match for a specified title.

**rawTitle** (required, **raw_title** in version 1.0): The title of the job offer to be classified.

**language** (optional): Default will be en (English). If you know the language specific classifier to use, this allows classification based on a language specific data set. Currently supported values are en, it (Italian), sv (Swedish), and de (German).

**rawDescription** (optional, **raw_description** in version 1.0): The description of the job offer, possibly empty.

**Response**

The returned JSON response contains the following fields:

**title**: The best match for rawTitle, selected from targetTitles. If no good match is found, this field will contain the string "No good match".

**confidence**: The classifier's confidence factor for the assignment. This is a normalized float value [0,1] but without any scale or reference point. It is used for ordering matches by the classifier but should not be used for thresholding. This field will be absent if no good match is found.

Sample Response (version 1.0):
```
{
    "data": {
        "title": "Network engineer",
        "confidence": 1
    }
}
```
Sample Response (version 0.9)
```
{
    "title": "Network engineer",
    "confidence": 1
}
```

#Versioning
-----------
ResolvedTitle is currently on version 1.0, and the data returned from this API is versioned.

Our general versioning strategy is available [here](/Versioning.md).

