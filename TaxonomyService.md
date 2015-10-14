Taxonomy
=============

Table of Contents
_________
- [Request Information](#request-information)
- [Sample Response](#sample-response)
- [Available Taxonomies](#available-taxonomies)
- [Versioning](#versioning)



#Request Information


HTTP method: GET
Parameters (query/form):
-        No Parameters
 
Example: https://api.careerbuilder.com/core/taxonomy/([Desired Taxonomy](#available-taxonomies))

#Sample Response


```
{
    "data": {
        "en": [
            {
                "id":"0",
                "title":"Title",
                "description":"Description",
                "type":"type"
            }
        ]
    }
}
```


#Response Information
----------
The response returns a single data node which contains a list of languages. Each language contains a list of taxonomies within that language.


#Available Taxonomies
-----------
onet15, onet17, carotenev1, carotenev2, carotenev2_2, skillsv2, skillsv3, skillsv4.


#Versioning
-----------
The response from the School Normalization Service is versioned with the current version being 1.0. The data set used to perform the normalization is also versioned (currently 1.0.0). This data set is upgraded at will in order to improve accuracy of the classifier. Requests made to this service will always use the latest version of the data set.

Our general versioning strategy is available [here](/Versioning.md).
