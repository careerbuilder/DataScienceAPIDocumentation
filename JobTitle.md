Job Title Service
=============

Table of Contents
_________
- [Request Information](#request-information)
- [Sample Response](#sample-response)
- [Available Taxonomies](#taxonomies)
- [Languages](#languages)
- [Translation Information](#translation-information)
- [Versioning](#versioning)



#Request Information


HTTP method: GET or POST
Parameters (query/form):

(This API is in transition to match CareerBuilder API standards. To provide peaceful transition, we support versions 0.9 and 1.0. Version is passed in the Accept header as the parameter version (Ex. Accept:application/json;version=1.0). 0.9 is the default version if user does not specify explicitly and all our existing users use this version by default. We highly encourage new users to pass version 1.0 in the Accept header. In version 0.9, all input parameters and output json fields are in camel case, while in version 1.0, input parameters and output json fields are in lowercase with underscores. Version 0.9 is scheduled for retirement by the end of Q1 2017.)
-        taxonomy (required) : classification taxonomy to use; accepted values are onet17, carotenev3, and carotenev3_1. Complete taxonomy lists can be found [here](https://github.com/cbdr/DataScienceAPITaxonomies/tree/master/JobTitle) (restricted to CBReadOnly). A single title/description may be classified against multiple taxonomies in a single request by providing multiple taxonomies in this parameter, separated by the pipe ("|") character (see example query below).
-        title (required if description is empty) : job title
-        description (required if title is empty) : job description
-        contentLang (use contentLang in version 0.9 and use content_lang in version 1.0) (optional) : the language of the provided text. You should provide this if it is known, as it will improve the accuracy of our translation system. If this parameter is not specified, the service will attempt to detect the language of the text automatically. Accepted language codes are listed below.

Version 1.0 Example: https://api.careerbuilder.com/core/classifier/jobtitle?title=Janitor&taxonomy=carotenev3.1|onet17&content_lang=auto
#Sample Response
```
{
    "data": {
        "onet17": [
            {
                "title": "Janitors and Cleaners, Except Maids and Housekeeping Cleaners",
                "id": "37-2011.00",
                "confidence": 90
            },
            {
                "title": "First-Line Supervisors of Housekeeping and Janitorial Workers",
                "id": "37-1011.00",
                "confidence": 56
            }
        ],
        "carotenev3.1": [
            {
                "title": "Janitor",
                "id": "37.1",
                "confidence": 1,
                "minor_title": "Building Cleaning and Pest Control Workers",
                "minor_id": "2000"
            }
        ]
    }
}
```

#Taxonomies
Possible taxonomies (with links to full taxonomy results)

| Taxonomy | description |
|----------|--------------|
| [onet17](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/oNet17.md) | Updated oNets |
| [carotenev3](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/CaroteneV3.md) | 7 removals, 139 updates, and 1,386 new titles.  [v2.2-v3 crosswalk.md](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/CaroteneV2_2ToV3CrossWalk.md) [Carotenev2.2 to v3 Crosswalk.xlsx](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/Carotenev2.2%20to%20v3%20Crosswalk.xlsx)|
| [carotenev3.1](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/CaroteneV3.1.csv) | 4 additions, 78 updates, and 28 removals. Adds disambiguation to v3 and includes minorTitle and minorId fields (in version 1.0, they are minor_title and minor_id) for hierarchical classification. [Carotenev3 to v3.1 Crosswalk.xlsx](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/Carotenev3%20to%20v3.1%20Crosswalk.xlsx) |

#Languages
Languages accepted are 
* ar
* bg
* ca
* cs
* cy
* da
* de
* el
* en
* es
* et
* fa
* fi
* fr
* ht
* he
* hi
* hu
* id
* it
* ja
* kn
* ko
* lv
* lt
* ms
* mt
* nl
* no
* pl
* pt
* ro
* ru
* sk
* sl
* sv
* th
* tl
* tr
* uk
* ur
* vi
* zhCHS
* zhCHT

#Translation Information

We use translation vendors for non-English requests. These vendors are unversioned and update and improve their algorithms at will. These translations are cached but may be refreshed and change over time. Due to this it is possible that non-English job titles may differ over time, though we expect the difference to be minimal.

When "auto" is supplied as the contentLang parameter and our service's built-in language detector identifies the text as non-English, we will rely on our translation vendors to detect the language of the text in addition to translating it.

#Versioning
-----------
JobTitle versions the API contract and taxonomies separately.  The API version controls the format of the API call and response.  The taxonomy version controls what data is returned for the call.  The current API version is 1.0.

Calls that do not specify an english language have unversioned data.  We use a machine translation waterfall to translate these into english if needed, and neither our code nor our vendors are versioned.

English Carotene calls have versioned data.  More languages will become versioned as we expand our native language Carotene capabilities.

English ONet calls are unversioned.  We will occasionally update our vendor and will not version that change.  In addition very rarely our vendor will return different results for the same input.  Neither of these will introduce new items into the taxonomy.

Our general versioning strategy is available [here](/Versioning.md).

