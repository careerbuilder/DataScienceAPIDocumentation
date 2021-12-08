Programmatic Filter Service
=============

Table of Contents
_________
- [Summary](#summary)
- [Request Information](#request-information)
- [Sample Response](#sample-response)

# Summary
This service takes a location and job title and/or description. The job title and/or description are used to call `Job Title` service. `Job Title` returns an onet id, which is then used to find the item in the ProgrammaticFilter table in DynamoDB with a `pk` key composed of the `onet_id` and location info (from request). 

If the item with matched key is in the table, that means that the amount of active jobs in the area is not over the threshold and we can post more job postings with the same `onet_id` for this area. In this case, 
we return `true` for response. If it is not in the table, we return `false`.

# Request Information

HTTP method: GET or POST (form or JSON)

Parameters:

* `title` (required if description is empty) : job title
* `description` (required if title is empty) : job description
* `locality`: The locality, such as the city or neighborhood, that corresponds to an address.
* `admin_area`: The subdivision name in the country or region for an address.

```
curl --location --request POST 'localhost:8080/programmatic/filter' \
--header 'x-proxy-secret-token: {TOKEN}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "title": "Police Officer",
    "description": "Supervisor of detectives",
    "locality": "Atlanta",
    "admin_area": "GA"
}'

```
This service will call jobtitle, so please get a valid jwt token to send request when running locally.

# Sample Response
```
{
    "data": {
        "not_over_threshold": true
    }
}
```
