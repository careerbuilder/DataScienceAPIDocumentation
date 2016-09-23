Job Title Service
=============

Table of Contents
_________
- [Request Information](#request-information)
- [Sample Response](#sample-response)
- [Available Taxonomies](#taxonomies)
- [Translation Information](#translation-information)
- [Versioning](#versioning)



#Request Information


HTTP method: GET or POST
Parameters (query/form):
-        taxonomy (required) : classification taxonomy to use; accepted values are onet15, onet17, carotenev1, carotenev2, carotenev2_2, carotenev3, and carotenev3_1. Complete taxonomy lists can be found [here](https://github.com/cbdr/DataScienceAPITaxonomies/tree/master/JobTitle) (restricted to CBReadOnly).
-        title (required if description is empty) : job title
-        description (required if title is empty) : job description
-        contentLang : the language of the provided text. You should provide this if it is known, as it will improve the accuracy of our translation system. If this parameter is not specified, the service will attempt to detect the language of the text automatically. Accepted language codes are as follows: ar, bg, ca, zhCHS, zhCHT, cs, da, nl, en, et, tl, fi, fr, de, el, ht, he, hi, hu, id, it, ja, kn, ko, lv, lt, ms, mt, no, fa, pl, pt, ro, ru, sk, sl, es, sv, th, tr, uk, ur, vi, cy.
-        outputType : response format, defaults to json; allowable values are json, xml
 
Example: https://api.careerbuilder.com/core/classifier/jobtitle?taxonomy=onet15&title=janitor&contentLang=en&outputType=xml

#Sample Response


```
<titleList>
    <titles>
        <title>Janitors and Cleaners, Except Maids and Housekeeping Cleaners</title>
        <id>37-2011.00</id>
        <confidence>90.0</confidence>
    </titles>
    <titles>
        <title>First-Line Supervisors/Managers of Housekeeping and Janitorial Workers</title>
        <id>37-1011.00</id>
        <confidence>56.0</confidence>
    </titles>
</titleList>
```

#Taxonomies
Possible taxonomies (with links to full taxonomy results)

| Taxonomy | description |
|----------|--------------|
| [onet15](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/oNet15.md) | Original oNets used at CB (RETIRES 5/25/2015)|
| [onet17](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/oNet17.md) | Updated oNets |
| [carotenev1](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/CaroteneV1.md) | Original carotene list (RETIRES 5/25/2015)|
| [carotenev2](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/CaroteneV2.md) | Full update, more accurate and comprehensive (RETIRES 8/24/2015)|
| [carotenev2.2](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/CaroteneV2.2.md) | Renames duplicated job titles from carotenev2 (RETIRES 8/24/2015)|
| [carotenev3](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/CaroteneV3.md) | 7 removals, 139 updates, and 1,386 new titles.  [v2.2-v3 crosswalk](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/JobTitle/CaroteneV2_2ToV3CrossWalk.md)|
| carotenev3.1 | Adds disambiguation to v3 and includes minorTitle and minorId fields for hierarchical classification |

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

