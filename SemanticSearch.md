Semantic Search API
===================

The semantic search API supports two endpoints: /query and /document.
The /query endpoint interprets and provides related entities for queries.
The /document endpoint parses a document, either a job or resume, and returns the most relevant semantic entities.

# Versions

-	[Version 2](SemanticSearch2)
-	[Version 1](SemanticSearch1)

## Versioning

The desired version must be supplied as Accept header in the HTTP request:

```
Accept: application/json;version=2.0
```

Different versions of the API may use different versions of taxonomies. This versioning is stable over the life of the contract version. For example, version 0.8 will always return job_titles with data version CaroteneV3. New contract versions will sometimes include contract upgrades to taxonomy versions.

Our general versioning strategy is available [here](Versioning.md).

# Upgrade Information


## API changes from 1.0 to 2.0

- General
	- Changed the semantics of the 'language' request parameter to mean the language of enrichments to be used. In version 1.0 the 'language' parameter was only used to look up the personal overrides.
	- Added request parameter 'relationships_threshold'.
	- Added new error messages.
- Document Endpoint
	- Removed the 'title' request parameter.
	- Removed the 'content' request parameter.
	- Added the 'document' request parameter (base64 encoded binary file).
	- Added the 'type' request parameter ("JOB" or "RESUME").
	- Removed the 'summary' field from the response.
	- Removed the sub field 'id' from the 'extracted_keywords' response field.
	- Added the 'parsed_input' field to the response.
	- Added relationships to the extracted_keywords.

## API changes from 0.8 to 1.0

- General
	- Replaced 'version' GET/POST request parameter with 'version' HTTP Accept header.
