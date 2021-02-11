# Semantic Skills Tagging API

The Semantic Skills Tagging service (available at https://api.careerbuilder.com/core/tagging/skills) takes raw text and tags surface forms of skills which appear in the text, returning a list of normalized skills and a confidence score.

This API supports the HTTP/GET and HTTP/POST methods.  

Currently supported versions are 4.1, 4.2, 5.0, and 8.0. Version is passed in the `Accept` header as the parameter version (E.g. ```Accept:application/json;version=8.0```).

**For versions < 8.0, this API is also available as a locally-runnable JAR. Clients with JVM-based applications may find this useful to avoid the overhead of repeated network calls. More information can be found [here](https://github.com/cbdr/SkillsExtractor).**

Taxonomies can be found through the [Taxonomy API](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/TaxonomyService.md).

Request
------- 

| Parameter             | 4.1                    | 4.2                  | 5.0                  | 8.0                  | Description | Default value |
|-----------------------|------------------------|----------------------|----------------------|----------------------|-------|------|
| `version`               | Required             | Required             | Required             | Required             | Passed in via the Accept header. Possible values are "4.1", "4.2", or "5.0". | - |
| `content`               | Required             | Required             | Required             | Required<sup>*</sup> |  A string containing the content to be tagged | - |
| `language`              | Optional             | Optional             | Optional             | Optional             | A string containing an ISO 639-1 code indicating the language in which the input text is written. If `language` is not provided in the request then English is used as the default. | English |
| `threshold`             | Optional<sup>†</sup> | Optional<sup>†</sup> | Optional<sup>†</sup> | Optional             | A double value between 0 and 1 controlling minimum relevancy scores for skill recognition. Higher values will more tightly restrict the returned skill tags. A threshold of 0 means all seed skill phrases recognized by exact string matching will be returned.  | 0.5 |
| `auto_threshold`        | Optional             | Optional             | Optional             | -                    | A boolean value indicating whether automatic thresholding is desired. If enabled, then when input text contains 150 or fewer words, the threshold parameter will be overwritten to 0 and all confidence values will be overwritten to 1.0<sup>‡</sup>. Default is true.  Note that this parameter is only supported for inputs in en, fr, and de. | - |
| `return_related_skills` | Optional             | Optional             | Optional             | -                    | A boolean value indicating whether to return related skills for each extracted skill. Defaults to false. Related skills contain the same fields as extracted skills, with confidence scores indicating their relevance to the extracted skill. | - |

<sup>*</sup> In version 8.0 the `content` field has a limit of 50,000 characters after which subsequent characters are ignored.

<sup>†</sup> Prior to version 8.0 `threshold` was only supported for English `content`.

<sup>‡</sup> A more detailed explanation of this functionality: The tagger uses “context” to define semantic relevancy.  If the input is too short (<= 150 words) to constitute a “context,” the tagger by default returns everything by direct matching, resulting in 0.0 confidence scores due to lack of context for relatedness. To avoid confusion, a pseudo score “1” is assigned to indicate “directly matched.” This feature is called “auto thresholding”.

#### Example request

```
curl -L -X POST 'https://wwwtest.api.careerbuilder.com/core/tagging/skills' \
-H 'Accept: application/json;version=8.0' \
-H 'Content-Type: application/json' \
-H 'Authorization: <BEARER_TOKEN>' \
--data-raw '{
  "content": "sed (\"stream editor\") is a Unix utility that parses and transforms text, using a simple, compact programming language. sed was developed from 1973 to 1974 by Lee E. McMahon of Bell Labs,[1] and is available today for most operating systems.",
  "language": "en",
  "threshold": 0.91
}'
```


Response
--------

The returned response contains only normalized names in accordance with Wikipedia. All returned skills are sorted first by relevancy and then alphabetically. 

The response structure of version 8.0 differs slightly from previous version.

## 8.0

```
{
  "data": {
    "meta": {
      "taxonomy_version": "2020-12-22T08:42:47"
    },
    "skills": [
      {
        "type": "IT Skill",
        "skilldid": "KS120076FGP5WGWYMP0F",
        "normalized_term": "Java (Programming Language)",
        "matches": [
          {
            "begin": 15,
            "end": 18,
            "likelihood": 1.0,
            "surface_form": "java"
          }
        ],
        "confidence": 1.0
      }
    ],
    "truncated": false,
    "version": "2.2.0"
  }
}
```

-   `matches` contains matched `surface_form`s for a given `normalized_term` and the character positions in the request's `content` field at which the `surface_form` begins and ends. 
- `likelihood` and `confidence` range between 0.0 and 1.0.
- Possible `type` values:
    - Certification
    - IT Skill
    - Language
    - Professional Skill
    - Soft Skill
- `truncated` will be `true` if the request's `content` field was larger than the aforementioned 50,000 character limit.


## 4.1, 4.2, 5.0


```
{
  "response": [
    {
      "skilldid": "KS120076FGP5WGWYMP0F",
      "skill_id": "KSH9TO",
      "normalized_term": "Apache Hadoop (Hadoop)",
      "confidence": 1,
      "type": "Hard Skill",
      "related_skills": [
        {
          "skilldid": "KS124KT6K427LFSF9NQC",
          "skill_id": "KSYFG5",
          "normalized_term": "MapReduce",
          "confidence": 0.830889,
          "type": "Hard Skill"
        }, 
           (...)
      ]
    }
  ]
}
```

Note that if `return_related_skills` is not set on the request then no `related_skills` will be present on the response.

## Error

```
{  
  "errors": [  
    {  
      "type": "empty content",
      "message": "content must be provided.",
      "code": 0
    }
  ]
}
```

Languages
-----------

The following table lists available languages and which versions they are available in.

| Language    | [ISO 639-1 code](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) | <=5.0 | 8.0 |
|-------------|-------------------------------------------------------------------------|-------|-----|
| Bulgarian   | bg                                                                      | ✓     |     |
| Czech       | cs                                                                      | ✓     |     |
| Danish      | da                                                                      | ✓     | ✓   |
| Dutch       | nl                                                                      | ✓     | ✓   |
| English     | en                                                                      | ✓     | ✓   |
| Estonian    | et                                                                      | ✓     |     |
| Finnish     | fi                                                                      | ✓     |     |
| French      | fr                                                                      | ✓     | ✓   |
| German      | de                                                                      | ✓     |     |
| Greek       | el                                                                      | ✓     |     |
| Hungarian   | hu                                                                      | ✓     |     |
| Italian     | it                                                                      | ✓     | ✓   |
| Latvian     | lv                                                                      | ✓     |     |
| Lithuanian  | lt                                                                      | ✓     |     |
| Maltese     | mt                                                                      | ✓     |     |
| Polish      | pl                                                                      | ✓     |     |
| Portuguese  | pt                                                                      | ✓     |     |
| Romanian    | ro                                                                      | ✓     |     |
| Sinhala     | si                                                                      | ✓     |     |
| Slovak      | sk                                                                      | ✓     |     |
| Spanish     | es                                                                      | ✓     | ✓   |
| Swedish     | sv                                                                      | ✓     |     |

Versioning
-----------
The data returned is versioned.  The current version is 8.0. A version must be supplied to this service; if no version is supplied, a 400 Bad Request error will be returned.

Our general versioning strategy is available [here](/Versioning.md).

