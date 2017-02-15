Semantic Overrides API
===================
Allows users and curators to add enrichments, remove enrichments, and make enrichments selected or unselected in real time for 
any canonical term and any taxonomy supported by Semantic 1.0

#Global Overrides
Global overrides are administrated by authorized curators via the Overrides UI tool. The tool is located at 
http://www.careerbuilder.com/nlUtils/SemanticOverridesTool/SemanticOverride.html?g=true
The tool will only work for those on an approved list. For questions about gaining access, send an email to 
DataScienceApplicationDevelopment@careerbuilder.com

#User Overrides
User overrides are only applied to Semantic Search requests with a corresponding user_id, 
allowing personalization of Semantic Search results. User overrides have priority over global overrides, and are applied to
any extracted keyword for which an override exists. 

##User Overrides Request

User overrides are added and deleted using a RESTful interface.

###HTTP method PUT

Adds or updates an override for the specified user_id, term, language, taxonomy, and enrichment combination. 
Since this endpoint modifies override state, it is restricted to certain OAuth client ids. For questions about access, send an email to 
DataScienceApplicationDevelopment@careerbuilder.com

Path parameter structure: https://api.careerbuilder.com/core/SemanticOverrides/user/{user_id}/{term}/{language}/{taxonomy}/{enrichment}

Parameters (URL-encoded path parameters):
-        user_id (required): A globally unique id to associate with the override request. 
Note that if your id collides with an existing id, the the existing id will be modified - we recommend using globally unique identifiers. 
-        term (required): the extracted keyword to which to apply the override. Note that special characters should be url-encoded. 
Note that the Matrix OAuth framework requires forward slashes in path parameters to be double-encoded (%252F)
-        language (required): The language to which the override applies. A two letter language code followed by underscore, followed by two letter country code. Required to use personalized overrides. Currently, allowed values are en_us, en_gb, fr_fr, de_de, and nl_nl.
-        taxonomy (required): The Taxonomy to which the override applies. Current valid taxonomies correspond to taxonomies returned by the Semantic Search API and are listed below.
-        enrichment (required): The enrichment to override. Whether to supply the enrichment id or name depends on the taxonomy, 
as outlined below. Enrichment ids are validated against the CB Taxonomy, while names can by any string.

| Taxonomy | Supply Enrichment Id or Name? |
|----------|--------------|
|RelatedSearchTermsV1 | Name | 
|RelatedSearchTermsRecruiterV1 | Name | 
|RelatedSearchTermsJobseekerV1 | Name|  
|TextKernelRelatedSearchTerms | Name |
|RawJobTitlesJobSeekerV1 | Name |
|RawJobTitlesRecruiterV1 | Name |
|CustomKeywords| Name |
|ONet17| Id |
|CaroteneV3 | Id |
|SkillsV4 | Id |

Parameters (Body parameters):
-         returned (required): boolean, defaults to false. Whether to return this enrichment, under this taxonomy, in queries containing this term and for this language and user_id.
-         selected (required): boolean, defaults to null. Sets state of the "selected" flag in the Semantic Search response for this enrichment, under this taxonomy, in queries containing this term and for this language and user_id.

Example Request
```
HTTP PUT
https://api.careerbuilder.com/core/SemanticOverrides/user/U123456789/java+developer/en_us/CaroteneV3/15.2/
Body:
returned=true&selected=true
```

The response will include no data unless an error occurred, in which case the response will be in CB API standard format. 

###HTTP Method DELETE

Removes and override for the specified user_id, term, language, taxonomy, and enrichment combination. The Semantic Search API will return its default behavior for this user_id/language before this override was added. 
Since this endpoint modifies override state, it is restricted to certain OAuth client ids. For questions about access, send an email to 
DataScienceApplicationDevelopment@careerbuilder.com

Path parameter structure: https://api.careerbuilder.com/core/SemanticOverrides/user/{user_id}/{term}/{language}/{taxonomy}/{enrichment}

Parameters:

Identical to PUT parameters, except that body parameters are ignored. 

Example Request
```
HTTP DELETE
https://api.careerbuilder.com/core/SemanticOverrides/user/U123456789/java+developer/en_us/CaroteneV3/15.2/
```

# Versioning information

Overrides are stored for versions of taxonomies and applied only to the same Taxonomy/version in the Semantic Search response. 
For example, an override for Carotene v3 will not apply to a hypothetical future Semantic Search version which returns Carotene v4. 
The API itself is currently unversioned.

