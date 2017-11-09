Job Level Classifier
=================


Table of Contents
_______________

- [Summary](#summary)
- [Input Requirements](#input-requirements)
- [Sample Response](#sample-response)
- [Response Information](#response-information)
- [Versioning](#versioning)


# Summary
 
This service will attempt to determine the level (i.e. seniority, expected experience) of a job based on its title. It will return a value from 1 to 5, along with a brief description.
 
The service is located at https://api.careerbuilder.com/core/tagging/joblevel. As usual, you will need OAuth credentials to use this service.


# Input Requirements

HTTP method: GET or POST Parameters (query/form): 
* title: (required) the job title.
 
## Version:
There are currently two API versions, 0.9 and 1.0. Default version is 0.9 and will be retired soon. Customers are encouraged to update the version to 1.0.
The difference between 1.0 and 0.9 is that 1.0 returns a `data` node in the json response and removes `text` field.

Here is an example input:

```
HTTP GET
Accept: application/json;version=1.0
https://api.careerbuilder.com/core/tagging/joblevel?title=software engineer
```

# Sample Response
For version 1.0
```
{
  "data": {
    "level": 3,
    "description": "Experienced (non-Manager)"
  }
}
```

For version 0.9
```
{
  "text": "software engineer",
  "description": "Experienced (non-Manager)",
  "level": "3"
}
```

# Response Information


The output will always contain the following three fields for a valid input:
 
text: the provided title

level: an integer between 1 and 5 indicating the job’s level

description: a brief description corresponding to the level
 
Following are all of the possible level + description pairs:

| Level | Description                                          |
|-------|------------------------------------------------------|
| 1     | Internship                                           |
| 2     | Entry Level                                          |
| 3     | Experienced (non-Manager)                            |
| 4     | Manager (Manager, Supervisor of Staff)               |
| 5     | Executive (VP, SVP, Department Head, President, etc) |

# Versioning
-----------
Our general versioning strategy is available [here](/Versioning.md).
 
Please email DataScienceApplicationDevelopment@careerbuilder.com if you have any questions or concerns.
