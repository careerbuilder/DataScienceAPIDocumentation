# Semantic Skills Tagging API

https://api.careerbuilder.com/core/tagging/skills

This API supports the HTTP/GET and HTTP/POST methods.  

This API currently supports the following versions, 2.0 (default), 3.0, 4.0 and 4.1, passed in the Accept header as the parameter version (Ex. Accept:version=4.1). 4.1 contains the latest taxonomy and is more accurate and has a few changes of field names and error formats to conform to standards.

| Parameter (4.1) | Parameter (3.0 - 4.0) | Parameter (2.0) | Required | Description |
|----------------|----------------|-----------------|----------|-------------|
| version        | version        | version         | optional | Passed in via the Accept header. Possible values are "2", "2.0", and "3.0".  Defaults to 2.0 | 
| content        | resume_content | resumecontent   | required | A string containing the resume content to be tagged |
|                |                | output          | optional | 2.0 only A string specifying the desired output format (JSON or XML). Default value is JSON. Standard header is used in 3.0 |
| lan            | lan            | lan             | optional | A string determining the language (total 22 languages supported) in which the input text is written. Default value is en. Note that the input parameter passing to language should be the language id. |
| lan            | lan            | threshold       | optional | A double value between 0 and 1 controlling minimum relevancy scores for skill recognition. Higher values will more tightly restrict the returned skill tags. Default is 0.5. A threshold of 0 means all seed skill phrases recognized by exact string matching will be returned. Note that this parameter is only supported for inputs in English. |
| auto_thres     | auto_thres     | auto_thres      | optional | A boolean value indicating whether automatic thresholding is desired. If enabled, then when input text contains 150 or fewer words, the threshold parameter will be overwritten to 0 and all confidence values will be overwritten to 1.0&#42;. Default is true.  Note that this parameter is only supported for inputs in en,fr, and de. |

&#42; A more detailed explanation of this functionality: The tagger uses “context” to define semantic relevancy.  If the input is too short (<= 150 words) to constitute a “context,” the tagger by default returns everything by direct matching, resulting in 0.0 confidence scores due to lack of context for relatedness. To avoid confusion, a pseudo score “1” is assigned to indicate “directly matched.” This feature is called “auto thresholding” (auto_thres).

Changes from V4 to V4.1 are 
* changed resume_content parameter to content
 

The V2 taxonomy is [here](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/Skill/SkillsV2.csv) RETIRES 5/4/2015

The V3 taxonomy is [here](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/Skill/SkillsV3.csv) RETIRES 8/3/2015

The V4 taxonomy can be found through the TaxonomyAPI [here](https://github.com/cbdr/DataScienceAPIDocumentation/blob/master/TaxonomyService.md)

The V2 to V3 crosswalk is [here](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/Skill/SkillsV2ToSkillsV3Crosswalk.md)



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
      "normalized_term":"Apache Hadoop (Hadoop)",
      "confidence":1.0
      "type": "Hard Skill"
    }
  ]
}
```

XML *not available in versions 2.0+
```
<response>
    <skill>
        <normalized_term>SQL Programming</normalized_term>
        <confidence>1.0</confidence>
    </skill>
    <skill>
        <normalized_term>Apache Hadoop (Hadoop)</normalized_term>
        <confidence>0.0</confidence>
    </skill>
    <skill>
        <normalized_term>Ruby on Rails</normalized_term>
        <confidence>0.0</confidence>
    </skill>
</response>
```
And an error response will look like this:

Version 3.0 and higher:
JSON
```
{  
  "errors":[  
    {  
      "type":"args",
      "message":"resume_content must be provided.",
      "code":3
    },
    {  
      "type":"another_error",
      "message":"So you can see two",
      "code":42
    }
  ]
}
```
Version 2.0:

JSON
```
{  
  "errors":[  
    "Unsupported language."
  ]
}
```

XML
```
<errors>
    <error>Unsupported language.</error>
</errors>
```

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
The data returned is versioned.  The current version is 4.1.  In order to maintain backwards compatibility the default version is 2.0 until 2.0 is sunset (then it will be newest). Likewise the API accepts a Version header with a value of "2" until 2.0 is sunset.

Our general versioning strategy is available [here](/Versioning.md).

