Taxonomy Service
=============

Table of Contents
_________
- [Request Information](#request-information)
- [Sample Response](#sample-response)
- [Available Taxonomies](#available-taxonomies)
- [Versioning](#versioning)



#Request Information


HTTP method: GET
Parameters: The only parameter accepted by this service is the name of the desired taxonomy, provided as part of the URL path.

https://api.careerbuilder.com/core/taxonomy/{taxonomyName}
 
Example: https://api.careerbuilder.com/core/taxonomy/ ([Desired Taxonomy](#available-taxonomies))/

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
            },
            {
                "id":"1",
                "title":"Title1",
                "description":"Description1",
                "type":"type"
            }
        ]
    }
}
```


#Response Information
----------
The response returns a single data node which maps each language code supported by the taxonomy to a list of the taxonomy's elements for that language.


#Available Taxonomies
-----------
onet15, onet17, carotenev1, carotenev2, carotenev2_2, skillsv2, skillsv3, skillsv4.


#Versioning
-----------
The response from the Taxonomy Service is versioned with the current version being 1.0. The data sets retrieved are also versioned. These data sets are upgraded at will in order to improve their accuracy. Requests made to this service will always use the latest version of the requested data set.

Our general versioning strategy is available [here](/Versioning.md).