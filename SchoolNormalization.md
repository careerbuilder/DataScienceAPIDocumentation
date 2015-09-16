SchoolNormalizationService
=============

Table of Contents
_________
- [Request Information](#request-information)
- [Sample Response](#sample-response)
- [Supported Countries](#supported-countries)
- [Versioning](#versioning)



#Request Information


HTTP method: GET or POST
Parameters (query/form):
-        query (required) : query 
-        country (optional) : country. Accepted language codes are [here](#supported-countries)
 
Example: https://api.careerbuilder.com/core/normalizedschools?query=georgia tech&country=us

#Sample Response


```
{
    "data": {
        "normalized_schools": [
            {
                "normalized_school_name": "Georgia Institute of Technology",
                "id": "53bff579e4b04710d09fa98d",
                "country": "US",
                "surface_form": "georgia tech",
                "confidence": 2
            }
        ]
    }
}
```

#Supported Countries
Possible taxonomies (with links to full taxonomy results)

| Taxonomy | description |
|----------|--------------|
| [onet15](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/oNet15.md) | Original oNets used at CB |
| [onet17](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/oNet17.md) | Updated oNets |
| [carotenev1](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/CaroteneV1.md) | Original carotene list |
| [carotenev2](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/CaroteneV2.md) | Full update, more accurate and comprehensive |
| [carotenev2.2](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/CaroteneV2.2.md) | Renames duplicated job titles from carotenev2 |

#Translation Information

We use translation vendors for non-English requests. These vendors are unversioned and update and improve their algorithms at will. These translations are cached but may be refreshed and change over time. Due to this it is possible that non-English job titles may differ over time, though we expect the difference to be minimal.

When "auto" is supplied as the contentLang parameter and our service's built-in language detector identifies the text as non-English, we will rely on our translation vendors to detect the language of the text in addition to translating it.

#Versioning
-----------
The response from the School Normalization Service is versioned with the current version being 1.0. The data set used to preform the normalization is also versioned with the current version being 1.0.0, however customers will not have the ability to use older versions of the data set as the updates are done to improve the accuracy of the results rather, and will not impact the response.

Our general versioning strategy is available [here](/Versioning.md).

