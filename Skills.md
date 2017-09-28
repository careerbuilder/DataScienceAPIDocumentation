# Semantic Skills Tagging API

https://api.careerbuilder.com/core/tagging/skills

This API supports the HTTP/GET and HTTP/POST methods.  

This API currently supports versions 4.1 and 4.2. Version is passed in the Accept header as the parameter version (Ex. Accept:application/json;version=4.1). 4.2 contains the latest taxonomy and incorporates feedback received about specific terms since the release of V4/4.1.

**This API is also available as a locally-runnable JAR. Clients with JVM-based applications may find this useful to avoid the overhead of repeated network calls. More information can be found [here](https://github.com/cbdr/SkillsExtractor).**

| Parameter (4.1/4.2) | Required | Description |
|----------------|----------------|-----------------|
| version        | Yes | Passed in via the Accept header. Possible values are "4.1" or "4.2". | 
| content        | Yes | A string containing the content to be tagged |
| language            | No | A string determining the language (total 22 languages supported) in which the input text is written. Default value is en. Note that the input parameter passing to language should be the language id. |
| threshold      | No | A double value between 0 and 1 controlling minimum relevancy scores for skill recognition. Higher values will more tightly restrict the returned skill tags. Default is 0.5. A threshold of 0 means all seed skill phrases recognized by exact string matching will be returned. Note that this parameter is only supported for inputs in English. |
| auto_thres     | No | A boolean value indicating whether automatic thresholding is desired. If enabled, then when input text contains 150 or fewer words, the threshold parameter will be overwritten to 0 and all confidence values will be overwritten to 1.0&#42;. Default is true.  Note that this parameter is only supported for inputs in en, fr, and de. |
| return_related_skills | No | A boolean value indicating whether to return related skills for each extracted skill. Defaults to false. Related skills contain the same fields as extracted skills, with confidence scores indicating their relevance to the extracted skill. |

&#42; A more detailed explanation of this functionality: The tagger uses “context” to define semantic relevancy.  If the input is too short (<= 150 words) to constitute a “context,” the tagger by default returns everything by direct matching, resulting in 0.0 confidence scores due to lack of context for relatedness. To avoid confusion, a pseudo score “1” is assigned to indicate “directly matched.” This feature is called “auto thresholding” (auto_thres).

The V4 taxonomy can be found through the TaxonomyAPI [here](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/TaxonomyService.md)

Response
-----------

The returned response contains only normalized names in accordance with Wikipedia. All returned skills are sorted first by relevancy and then alphabetically.

A sample response with data will look like this:

JSON
```
{  
  "response":[  
    {  
      "skilldid": "KS120076FGP5WGWYMP0F",
      "skill_id": "KSH9TO",
      "normalized_term":"Apache Hadoop (Hadoop)",
      "confidence":1.0
      "type": "Hard Skill"
    }
  ]
}
```
If the return_related_skills request parameter is set to true, the reponse will look like this:

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

And an error response will look like this:

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

A note on `skilldid` vs. `skill_id`: These two fields both serve the same purpose of acting as a unique identifier for a skill. The latter was introduced more recently and is more compact as compared to `skilldid`. New applications should prefer usage of the `skill_id` field. The `skilldid` field may be deprecated in a future version of the service.

Languages
-----------
The following languages are supported by this API.  The English taxonomy is significantly larger than other languages.
* bg
* cs
* cz (Deprecated -- use cs instead for Czech)
* da
* de
* dk (Deprecated -- use da instead for Danish)
* ee (Deprecated -- use et instead for Estonian)
* el
* en
* es
* et
* fi
* fr
* gr (Deprecated -- use el instead for Greek)
* hu
* it
* lt
* lv
* mt
* nl
* pl
* pt
* ro
* se (Deprecated -- use sv instead for Swedish)
* si
* sk
* sv

#Versioning
-----------
The data returned is versioned.  The current version is 4.1. A version must be supplied to this service; if no version is supplied, a 400 Bad Request error will be returned.

Our general versioning strategy is available [here](/Versioning.md).

