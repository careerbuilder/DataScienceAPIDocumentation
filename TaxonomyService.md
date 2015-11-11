Taxonomy Service
=============

Table of Contents
_________
- [Request Information](#request-information)
- [Response Information](#response-information)
- [Sample Response](#sample-response)
- [Available Taxonomies](#available-taxonomies)
- [Versioning](#versioning)



#Request Information


HTTP method: GET
Parameters: The only parameter accepted by this service is the name of the desired taxonomy, provided as part of the URL path. Taxonomy names are case-sensitive and must be entered in lowercase.

https://api.careerbuilder.com/core/taxonomy/ {[Desired Taxonomy](#available-taxonomies)} /
 
Example: https://api.careerbuilder.com/core/taxonomy/carotenev1


#Response Information

The response returns a single data node which maps each language code supported by the taxonomy to a list of the taxonomy's elements for that language.


|Response Item|Always Returned|Description|
|----------|----------|-------------|
| id | X | The unique identifier of the taxonomy entry |
| title | X | The title of the taxonomy entry |
| description | | A brief description for the taxonomy entry |
| type | | Describes the taxonomy entry's type |
| normalized name | | The normalized name for the skill title |


#Sample Response


```
{
    "data": {
        "en": [
            {
                "id":"0",
                "title":"Title0",
                "description":"Description0",
                "type":"type"
                "normalized_name":"Normalized Name"
            },
            {
                "id":"1",
                "title":"Title1",
                "description":"Description1",
                "type":"type"
                "normalized_name":"Normalized Name"
            }
        ]
    }
}
```


#Available Taxonomies
-----------
onet15, onet17, carotenev1, carotenev2, carotenev2_2, carotenev3, skillsv2, skillsv3, skillsv4, naics2007.


#Versioning
-----------
The response from the Taxonomy Service is versioned with the current version being 1.0. The data sets retrieved are also versioned. These data sets are upgraded at will in order to improve their accuracy. Requests made to this service will always use the latest version of the requested data set.

Our general versioning strategy is available [here](/Versioning.md).
