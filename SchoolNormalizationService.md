Job Title Service
=============

Table of Contents
_________
- [Request Information](#request-information)
- [Sample Response](#sample-response)
- [Supported Countries](#countries)
- [Versioning](#versioning)



#Request Information


HTTP method: GET or POST
Parameters (query/form):
-        query (required) : query 
-        country (optional) : country. Accepted language codes are as follows: ar, bg, ca, zhCHS, zhCHT, cs, da, nl, en, et, tl, fi, fr, de, el, ht, he, hi, hu, id, it, ja, kn, ko, lv, lt, ms, mt, no, fa, pl, pt, ro, ru, sk, sl, es, sv, th, tr, uk, ur, vi, cy.
 
Example: https://api.careerbuilder.com/core/

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
JobTitle versions the API contract and taxonomies separately.  The API version controls the format of the API call and response.  The taxonomy version controls what data is returned for the call.  The current API version is 1.0.

Calls that do not specify an english language have unversioned data.  We use a machine translation waterfall to translate these into english if needed, and neither our code nor our vendors are versioned.

English Carotene calls have versioned data.  More languages will become versioned as we expand our native language Carotene capabilities.

English ONet calls are unversioned.  We will occasionally update our vendor and will not version that change.  In addition very rarely our vendor will return different results for the same input.  Neither of these will introduce new items into the taxonomy.

Our general versioning strategy is available [here](/Versioning.md).

