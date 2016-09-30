ResumeParsingService
====================

Table of Contents
_____________

- [Summary](#summary)
- [Input Data Information](#input-data)
- [Sample Return Data](#sample-return-data)
- [Return Data Information:](#returned-data-objects)
- [Current Supported Languages](#current-supported-languages)
- [Current Default Languages](#current-language-defaults)
- [Versioning](#versioning)



#Summary
__________

**This service is deprecated. Please use our Parse & Normalize service instead, located at https://api.careerbuilder.com/core/parsing/normalizedresume and documented [here](/ParseAndNormalize.md). If you do not want any additional enrichments in your response, include the "desired_enrichments=none" parameter with your request.**

The service is located at https://api.careerbuilder.com/core/parsing/resume. As usual, you will need OAuth core credentials to use this service. (*If you do not have these please contact API and Auth to request credentials*)

Here is an example input: https://api.careerbuilder.com/core/parsing/resume?document=BASE64EncodedDocument

This service accepts POST Requests

We accept .DOC, .DOCX, .PDF, .RTF, .TXT, .ODT, and .WPS documents given in a BASE64 encoded string (Required)

The corresponding output is to careerbuilder API standards, with the data node holding all resume data.

Input Data
==========

Input should be provided in the POST request body, and content type should be either JSON or Form Vars (xml is not accepted)

document: We accept .doc, .docx, .pdf, .rtf, .txt, .odt, and .wps documents given in a BASE64 encoded string.

*All documents that contain Greek as their primary language or are sent with the service=hireability parameter must be 65 characters or more. Any document that does not meet this threshold will return a 400 Bad Request exception.*

Note that image files (including scanned resumes) are not supported at this time.

language: ISO 639-1 language code (i.e. en, en-gb, es-419), this is used if a service is not specified to determine the parsing service we wish to use. (optional)

service: The title of the resume parsing service you wish to use (currently available are "textkernel", "hireability", & "daxtra") (Optional) (This field is case sensitive)

output_type: allows you to control the output either json (default) or xml

return_date_of_birth: (Optional) If this parameter is provided then the service will return a filled date of birth field when it is included on the document. If not provided then this will return an empty string. Note, this field was made available mainly for our international clients. It can be considered age discriminatory in the US and should only be used in special circumstances.


Sample Return Data
============
```
{  
   "data":"RESUME-DATA-HERE",
   "timing":"TIMING-INFO-HERE",
   "forensics":"FORENSIC-DATA-HERE",
   "errors":"Error-DATA-HERE"
}
```

Possible Response Status Codes
=========================

ALL 4xx status codes should not be retried. ALL 5xx status codes should be retried.

 - 400 Bad Request: this is returned when the request is syntactically incorrect or malformed in some way
 - 409 Conflict: this is returned when a virus is detected in the document
 - 415 Unsupported Media Type: this is returned when our parsers detect that the document may be an image document
 - 500 Internal Server Error: this is returned when an uncaught exception is thrown somewhere within our code. Please let us know if you see 500's
 - 502 Bad Gateway: this is returned when one of our parsers or other downstream services has an unexpected problem
 - 504 Timeout: this will be returned as a 572 through the routing layer and should be retried after waiting for a minute or two


Returned Data Objects
=============================

Data

```
{  
   CleanResumeText = String
   RawResumeText =String
   ResumeHTML = String
   HRXML = String
   HasManagedOthers = Boolean
   FormattedName = String
   FirstName = String
   MiddleName = String
   LastName = String
   Affix = String
   EmailAddress = String
   HomeNumber = String
   OfficeNumber = String
   MobileNumber = String
   FaxNumber = String
   PagerNumber = String
   Country = String
   Zip = String
   State =String
   City = String
   AddressLine1 = String
   Nationality = String
   Civility = String
   DateOfBirth = String
   MaritalStatus = String
   Hobby = String
   Benefits = String
   Salary = String
   Reference = String
   DriversLicense = Boolean
   IsCurrentlyEmployed = String
   MostRecentEmployer = String
   MostRecentJobTitle = String
   ExperienceMonths = Int
   LastJobMonths = Int
   NumberOfJobs = Int
   HighestDegreeType = String
   Languages = Array.new
   ResumeEmployments = Array of Employments
   ResumeEducationHistories = Array of Educations
   SoftSkill = Array of Soft Skills
   CompSkill = Array of Computer-specific skills
}
```

Employment

```
{  
   City = String
   State = String
   Country = String
   Website = String
   JobTitle = String
   EmployerName = String
   StartDate = String
   EndDate = String
   Description =String
   JobType = String
   Duration = Int
   IsCurrentPosition = Boolean
   IsLastItem = Boolean
   Experience = String
   FullText = String
}
```

Education

```
{  
   SchoolName = String
   SchoolType = String
   AddressType = String
   City = String
   State = String
   Country = String
   DegreeMajor = String
   DegreeName = String
   DegreeDate = String
   DegreeType = String
   DegreeComments = String
   AttendenceStartDate = String
   AttendenceEndDate = String
   EducationalMeasureSystem = String
   MeasureSystemValue = String
   MeasureSystemLowest = String
   MeasureSystemHighest = String
   Subjects = String
   FullText = String
}
```
DegreeType Information
----------------
```
CE3 -> no known education history
CE31 -> high school or GED equivalent
CE32 -> associate’s degree
CE321 -> bachelor’s degree
CE3210 -> master’s degree
CE3211 -> doctorate
```

Current Supported Languages   
==============================
| Language Code | Language              | Daxtra | HireAbility | Textkernel |   |          |
|----|-----------------------|--------|--------|----|---|------------------|
| en | English               | X      | O      | X  |   |                  |
| nl | Dutch                 | X      | O      | X  |   | Supported        |
| fr | French                | X      | O      | X  |   | X                |
| it | Italian               | X      | O      | X  |   | ContactInfo Only |
| da | Danish                | X      | O      | X  |   | I                |
| pl | Polish                | X      | O      | X  |   | Not Supported    |
| ru | Russian               | X      | O      | X  |   | O                |
| de | German                | X      | O      |  X |   |                  |
| ro | Romanian              | X      | O      |  X |   |                  |
| cz | Czech                 | X      | O      |  X |   |                  |
| zh | Chinese (Simplified)  | X      | O      |  X |   |                  |
| zh | Chinese (Traditional) | X      | O      |  X |   |                  |
| es | Spanish               | X      | O      |  X |   |                  |
| ca | Catalan               | X      | O      |  O |   |                  |
| gl | Galician              | O      | O      |  O |   |                  |
| eu | Basque                | O      | O      |  O |   |                  |
| el | Greek                 | O      | X      |  O |   |                  |
| hu | Hungarian             | X      | O      |  X |   |                  |
| sv | Swedish               | X      | O      |  X |   |                  |
| no | Norwegian             | X      | O      |  O |   |                  |
| pt | Portuguese            | O      | O      |  X |   |                  |
| sk | Slovak                | X      | O      |  X |   |                  |
| ja | Japanese              | X      | O      |  O |   |                  |

Current Language Defaults
===========================
| Language Code | Language   | HireAbility | Daxtra | Textkernel |
|----|-----------------------|--------|--------|------------|
| zh | Chinese (Simplified)  |        | X      |            |
| zh | Chinese (Traditional) |        | X      |            |
| id | Indonesian            |        | X      |            |
| hi | Hindi                 |        | X      |            |
| pl | Polish                |        |        |      X     |
| ro | Romanian              |        |        |      X     |
| it | Italian               |        |        |      X     |
| da | Danish                |        |        |      X     |
| hu | Hungarian             |        |        |      X     |
| sk | Slovak                |        |        |      X     |
| ja | Japanese              |        | X      |            |
| en-gb | English (British)  |        |        |      X     |
| el | Greek                 | X      |        |            |
|    | Other Languages       |        |        |      X     |


#Versioning
-----------
The data returned is unversioned. The current version is 1.0. We expect that each of our vendors return the same data for repeated calls, however we have not verified this systematically.  We will occasionally update our vendors which may change the output.  If we believe this change is significant we will communicate about it.  However, customers will not be able to specify vendor versions; we will not be running multiple versions of a parser.  The language to parser mapping is unversioned.  We will update it as we understand each vendor's capabilities more fully.  If you need to stay on the same parser, please specify it explicitly.

Our general versioning strategy is available [here](/Versioning.md).
