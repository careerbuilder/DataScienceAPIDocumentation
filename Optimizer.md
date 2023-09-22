Optimizer Service
====================

Table of Contents
_____________

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Versioning](#versioning)

Summary
-----------
Optimizer service is a tool that helps customers find the best application for their needs and budget. It does this by taking into account a variety of factors, including the customer's budget and the price of the application.

 The service will then present the customer with a list of expected applications.

This information can help the customer make an informed decision about which "solution" to choose.

The Optimizer service  is available at `/core/optimizer`.

Request Structure
-----------

Requests consist of:

| Parameter             | Require/Optional | Type    | Description                             |
|:----------------------|:-----------------|:--------|:----------------------------------------|
| `budget`              | Yes              | float   | Total Customer budget                   |
| `obj_type`            | yes              | string  | Objective to optimize "Budget" or "EOI" |
| `dynamic_upper_bound` | yes              | boolean | ?                                       |
| `constraints`         | Yes              | string  | Array of constraints                    |

Constraints example :

| Parameter          | Require/Optional | Type    | Description                     |
|:-------------------|:-----------------|:--------|:--------------------------------|
| `id`               | Yes              | string  | JobDID                          |
| `price`            | yes              | float   | Price of the application ?      |
| `cost`             | yes              | float   | Cost of the application ?       |
| `min_applications` | yes              | boolean | Minimum applications required ? |
| `max_applications` | Optional         | string  | Maximum applications required ? |

Example cURL request : 

```
curl -X POST \
  https://api.careerbuilder.com/core/optimizer \
  -H 'Accept: application/json;version=1.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
        "budget": 2000,
        "obj_type": "budget",
        "dynamic_upper_bound": "false",
        "constraints": [
            {
                "id": "Job1",
                "price": 40,
                "cost": 20,
                "min_applications":12
            },
            {
                "id": "Job2",
                "price": 30,
                "cost": 20,
                "min_applications": 6
            }
        ]
      }'
```

Response Structure
-----------
The returned response is a status and optimized solutions for expected jobs.
You will have in each solution expected application

```json
{
    "data": {
        "status": "Optimal",
        "solutions": [
            {
                "id": "Job1",
                "applications": 14
            },
            {
                "id": "Job2",
                "applications": 48
            }
        ]
    }
}
```
Versioning
-----------
The response from the Optimizer Service is versioned with the current version being 1.0. 

Our general versioning strategy is available [here](/Versioning.md).
