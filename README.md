DataScienceAPIDocumentation
===========================

This repo houses all published Data Science API documentation.

You will need OAuth core credentials to use these services. (*If you do not have these please contact API and Auth to request credentials*)

We provide .net and java wrappers for our APIs, which can be found [here](https://github.com/cbdr/DataScienceServices). (Note: this repository may only be viewed by members of the cbdr GitHub organization.)

Our general API versioning documentation can be found [here](Versioning.md).

APIs:
----
[Geocoding](Geocoding.md): Provides geolocation data.

[Geovalidation](Geovalidation.md): Provides validated geolocation data.

[JobTitle](JobTitle.md): Provides normalized job titles like Carotene and oNet.

[Skills](Skills.md): Provides normalized skills.

[ParseAndNormalize](ParseAndNormalize.md): Parses resumes and normalizes through most other services offered.

[JobLevel](JobLevel.md): Provides a normalized job level for a job title.

[KeywordStuffingDetector](KeywordStuffingDetector.md): Indicates if a job has been keyword stuffed.

[RelevantSkills](RelevantSkills.md): Provides a list of relevant skills for a given skill.

[ResolvedTitle](ResolvedTitle.md): Attempts to resolve a job title to the closest match from a given list of target titles.

[SemanticSearch](SemanticSearch.md): Parses a query into intended phrases and identifies matching and related normalized entities.


