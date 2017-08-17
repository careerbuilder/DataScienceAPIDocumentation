DataScienceAPIDocumentation
===========================

This repo houses all published Data Science API documentation.

You will need OAuth core credentials to use these services. (*If you do not have these please contact API and Auth to request credentials*)

We provide .net and java wrappers for our APIs, which can be found [here](https://github.com/cbdr/DataScienceServices). (Note: this repository may only be viewed by members of the cbdr GitHub organization.)

Our general API versioning documentation can be found [here](Versioning.md).

APIs:
----
[Company Normalization](CompanyNormalization.md): Attempts to normalize a company name and provide standardized data such as location and NAICS classification information.

[Geocoding](Geocoding.md): Attempts to normalize structured or unstructured location information to a known geographic entity and return rich addressing information for that entity.

[GeoEntity](GeoEntity.md): Provides lists of geographic entities of a particular type, such as postal codes or first-level administrative areas. 

[Job Title](JobTitle.md): Classifies a job title/description against the specified version(s) of the ONet and/or Carotene taxonomies.

[Skills](Skills.md): Extracts normalized skill names from text and computes confidence scores for each.

[Job Parse & Normalize](JobParseAndNormalize.md): Parses job postings and (optionally) normalizes through most other services offered.

[Resume Parse & Normalize](ResumeParseAndNormalize.md): Parses resumes and (optionally) normalizes through most other services offered.

[Job Level](JobLevel.md): Provides a normalized job level (entry-level, mid-level, executive, etc.) for a job title.

[Keyword Stuffing Detector](KeywordStuffingDetector.md): Indicates if a job has been keyword stuffed (overloaded with spammy/irrelevant keywords in an attempt to get it ranked more highly in search results)

[Resolved Title](ResolvedTitle.md): Attempts to resolve a job title to the closest match from a given list of target titles.

[Semantic Search](SemanticSearchV2.md): Parses a query into intended phrases and identifies matching and related normalized entities.

[Semantic Overrides](SemanticOverrides.md): Allows users and curators to add enrichments, remove enrichments, and make enrichments selected or unselected for Semantic Search v1.0 and above

[School Normalization](SchoolNormalization.md): Attempts to normalize a school name and provide standardized data such as location and IPEDs classification information.

[Taxonomy Service](TaxonomyService.md): Returns full lists of classification taxonomies such as Carotene and Skills.

[Virus Scanner](VirusScanner.md): Scans a file for viruses and returns the virus definition if applicable.
