# Semantic Skills Tagging API

https://api.careerbuilder.com/core/tagging/skills

This API supports the HTTP/GET and HTTP/POST methods.  

This API currently supports 2 versions, 2.0 (default) and 3.0, passed in the Accept header as the parameter version.  3.0 has a new taxonomy and is more accurate and has a few changes of field names and error formats to conform to standards.

| Parameter      | Parameter (2.0) | Required | Description |
|----------------|-----------------|----------|-------------|
| version        | version         | optional | Passed in via the Accept header. Possible values are "2", "2.0", and "3.0".  Defaults to 2.0 | 
| resume_content | resumecontent   | required | A string containing the resume content to be tagged |
|                | output          | optional | 2.0 only A string specifying the desired output format (JSON or XML). Default value is JSON. Standard header is used in 3.0 |
| lan            | lan             | optional | A string determining the language (total 22 languages supported) in which the input text is written. Default value is en. Note that the input parameter passing to language should be the language id. |
| threshold      | threshold       | optional | A double value between 0 and 1 controlling minimum relevancy scores for skill recognition. Higher values will more tightly restrict the returned skill tags. Default is 0.5. A threshold of 0 means all seed skill phrases recognized by exact string matching will be returned. Note that this parameter is only supported for inputs in English. |
| auto_thres     | auto_thres      | optional | A boolean value indicating whether automatic thresholding is desired. If enabled, then when input text contains 150 or fewer words, the threshold parameter will be overwritten to 0 and all confidence values will be overwritten to 1.0&#42;. Default is true.  Note that this parameter is only supported for inputs in en,fr, and de. |

&#42; A more detailed explanation of this functionality: The tagger uses “context” to define semantic relevancy.  If the input is too short (<= 150 words) to constitute a “context,” the tagger by default returns everything by direct matching, resulting in 0.0 confidence scores due to lack of context for relatedness. To avoid confusion, a pseudo score “1” is assigned to indicate “directly matched.” This feature is called “auto thresholding” (auto_thres).

Changes from V2 to V3 are 
* Added multiple sense disambiguations to 122 skills surface forms. 
* Added 249 new skills. 
* Merged 489 skills for de-duplication. 
* Renamed 68 skills. 
* Removed 61 skills.
 

The V2 taxonomy is [here](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/Skill/SkillsV2.csv)

The V3 taxonomy is [here](https://github.com/cbdr/DataScienceAPITaxonomies/blob/master/Skill/SkillsV3.csv)

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
      "normalized_term":"Apache Hadoop (Hadoop)",
      "confidence":1.0
    },
    {  
      "normalized_term":"Apache Hive",
      "confidence":1.0
    },
    {  
      "normalized_term":"Softwares",
      "confidence":0.0
    },
    {  
      "normalized_term":"Java Platform Enterprise Edition (Computing Platforms)",
      "confidence":0.0
    }
  ]
}
```

XML
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

Version 3.0:
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

XML
```
<errors>
    <error>
        <type>args</type>
        <message>resume_content must be provided.</message>
        <code>3</code>
    </error>
    <error>
        <type>another_error</type>
        <message>So you can see two</message>
        <code>42</code>
    </error>
</errors>
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
* cz
* dk
* de
* gr
* en
* es
* ee
* fi
* fr
* hu
* it
* lt
* lv
* mt
* nl
* pl
* pt
* ro
* sk
* si
* se

#Versioning
-----------
The data returned is versioned.  The current version is 3.0.  In order to maintain backwards compatibility the default version is 2.0 until 2.0 is sunset (then it will be newest). Likewise the API accepts a Version header with a value of "2" until 2.0 is sunset.

Our general versioning strategy is available [here](/Versioning.md).

