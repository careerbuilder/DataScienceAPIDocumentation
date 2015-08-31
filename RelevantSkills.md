# Relevant Skills API

https://api.careerbuilder.com/core/tagging/relevantskills

This URL supports the HTTP/GET and HTTP/POST methods.  

This API currently supports 2 versions, 2.0 (default) and 3.0, passed in the Accept header as the parameter version.  3.0 has a new taxonomy and is more accurate and has a few changes of field names and error formats to conform to standards.

| Parameter      | Parameter (2.0) | Required | Description |
|----------------|-----------------|----------|-------------|
| Version        | Version         | optional | Passed in via the Accept header. Possible values are "2", "2.0", and "3.0".  Defaults to 2.0 | 
| skill_name | skillname   | required | A pipe separated string containing the skill names to be parsed |
|    output            | output          | optional | 2.0 only A string specifying the desired output format (JSON or XML). Default value is JSON.|
| combine     |       | optional | A boolean value indicating whether or not to concatenate skillnames by pipe|

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
  "skills_data": [
    {
      "skill_name": "c",
      "relevant_skills": [
        {
          "normalized_name": "C++ (Programming Language)",
          "score": 0.784706
        },
        {
          "normalized_name": "Java",
          "score": 0.727388
        },
        (…lots more C-related skills…)
      ]
    },
    {
      "skill_name": "hadoop",
     "relevant_skills": [
        {
          "normalized_name": "Apache HBase (BigTable Implementations)",
          "score": 0.851156
        },
        {
          "normalized_name": "Apache Hive",
          "score": 0.849868
             },
        (…lots more Hadoop-related skills…)
    ]
    }
]
}
```

XML
```
<response>
    <skills_data>
        <skill_name>writing</skill_name>
        <relevant_skill>
            <normalized_name>Stand-Up Comedy</normalized_name>
            <score>0.591823</score>
        </relevant_skill>
        <relevant_skill>
            <normalized_name>Hapkido</normalized_name>
            <score>0.588954</score>
        </relevant_skill>
        <relevant_skill>
            <normalized_name>Creative Writing (Writing)</normalized_name>
            <score>0.588566</score>
        </relevant_skill>
        (…lots more skills…)
    </skills_data>
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
      "message":"skill_name must be provided.",
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
        <message>skill_name must be provided.</message>
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
  "skillsData": [
    {
      "skillName": "c",
      "relevantSkills": [
        {
          "normalizedName": "C++ (Programming Language)",
          "score": 0.784706
        },
        {
          "normalizedName": "Java",
          "score": 0.727388
        },
        (…lots more C-related skills…)
      ]
    },
    {
      "skillName": "hadoop",
     "relevantSkills": [
        {
          "normalizedName": "Apache HBase (BigTable Implementations)",
          "score": 0.851156
        },
        {
          "normalizedName": "Apache Hive",
          "score": 0.849868
             },
        (…lots more Hadoop-related skills…)
    ]
    }
]
}
```

XML
```
<response>
    <skillsData>
        <skillName>writing</skillName>
        <relevantSkill>
            <normalizedName>Stand-Up Comedy</normalizedName>
            <score>0.591823</score>
        </relevantSkill>
        <relevantSkill>
            <normalizedName>Hapkido</normalizedName>
            <score>0.588954</score>
        </relevantSkill>
        <relevantSkill>
            <normalizedName>Creative Writing (Writing)</normalizedName>
            <score>0.588566</score>
        </relevantSkill>
        (…lots more skills…)
    </skillsData>
</response>
```

JSON
```
{  
  "errors":[  
    "Error: A valid skill name must be provided."
  ]
}
```

XML
```
<errors>
    <error>Error: A valid skill name must be provided.</error>
</errors>
```

#Versioning
-----------
The data returned is versioned.  The current version is 3.0.  In order to maintain backwards compatibility the default version is 2.0 until 2.0 is sunset (then it will be newest). Likewise the API accepts a Version header with a value of "2" until 2.0 is sunset.

Our general versioning strategy is available [here](/Versioning.md).
