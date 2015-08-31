

**URL:** https://www.api.careerbuilder.com/core/classifier/resolvedtitle

This endpoint supports the HTTP/POST method only.

**Parameters**

**targetTitles** (required): A string containing two or more pipe-delimited job titles to test the input against. For example: Software Engineer|Database Administrator|Systems Administrator. A single title may be supplied if the user simply wishes to test whether an input is a good match for a specified title.

**rawTitle** (required): The title of the job offer to be classified.

**language** (optional): Default will be en (English). If you know the language specific classifier to use, this allows classification based on a language specific data set. Currently supported values are en, it (Italian), sv (Swedish), and de (German).

**rawDescription** (optional): The description of the job offer, possibly empty.

**Response**

The returned JSON response contains the following fields:

**title**: The best match for rawTitle, selected from targetTitles. If no good match is found, this field will contain the string "No good match".

**confidence**: The classifier's confidence factor for the assignment. This is a normalized float value [0,1] but without any scale or reference point. It is used for ordering matches by the classifier but should not be used for thresholding. This field will be absent if no good match is found.

#Versioning
-----------
ResolvedTitle is currently on version 1.0, and the data returned from this API is versioned.

Our general versioning strategy is available [here](/Versioning.md).

