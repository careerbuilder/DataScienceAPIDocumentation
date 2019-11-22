Autocomplete Service
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Examples](#examples)
- [Versioning](#versioning)

## Summary

DSAD's Autocomplete Service is an HTTP REST webservice for autocompleting queries on one of several possible taxonomies. Currently supported taxonomies and their relative API routes are as follows:

| Taxonomy    | Relative API Route |
|----------|-------------|
| Carotene V1 | `/core/autocomplete/carotenev1` |
| Carotene V2 | `/core/autocomplete/carotenev2` |
| Carotene V2.2 | `/core/autocomplete/carotenev2_2` |
| Carotene V3 | `/core/autocomplete/carotenev3` |
| Carotene V3.1 | `/core/autocomplete/carotenev3_1` |
| NAICS 2007 | `/core/autocomplete/naics2007` |
| Normalized Companies | `/core/autocomplete/normalizedcompanies` |
| Normalized Majors | `/core/autocomplete/normalizedmajors` |
| Normalized Schools | `/core/autocomplete/normalizedschools` |
| ONet15 | `/core/autocomplete/onet15` |
| ONet17 | `/core/autocomplete/onet17` |
| Skills V4 | `/core/autocomplete/skillsv4` |
| Skills V5 | `/core/autocomplete/skillsv5` |
| Multiple taxonomies | `/core/autocomplete/taxonomies` |

### Request Structure

Most supported taxonomies follow the same simple request format that consists of the following fields:

| Field    | Description | Required? |
|----------|-------------|-----------|
| `query` | The text to be autocompleted. For example, the query "nur" on a Skills taxonomy may yield an autocomplete suggestion of "Nursing". | Yes |
| `limit` | The number of results to be returned. Acceptable values are integers between 1 and 100 (inclusive). Defaults to 20. | No |

&nbsp;

The Normalized Schools taxonomy also uses an additional request parameter:

| Field    | Description | Required? |
|----------|-------------|-----------|
| `school_level` | The type of school to which the results should be limited. Accepted values are `secondary` and `postsecondary` (case-insensitive). | No |

The Multi taxonomy endpoint needs the following additional parameter

| Field    | Description | Required? |
|----------|-------------|-----------|
| `taxnomies` | List of taxonomies used for retrieving autocomplete results based on the query param | Yes |


Requests may be provided as HTTP GET requests with query parameters, or as HTTP POST requests with either URL-encoded form or JSON content.

### Response Structure

All responses with an HTTP status of 200 will consist of a JSON object with a top-level `data` node containing the following:

| Field    | Description |
|----------|-------------|
| `results` | A JSON array containing zero or more result objects. Each result represents a normalized entity from the requested taxonomy. The result object structure is described in more detail below. |

&nbsp;

Normalized Company, Normalized Major, and Normalized School responses will also contain the following in the `data` node:

| Field    | Description |
|----------|-------------|
| `data_version` | A string representing the current version of data backing the autocomplete service for the selected taxonomy. This is typically represented as a string in the YYYY-MM-DD format, but this format is not guaranteed. |

&nbsp;

The structure of the result objects is as follows for all taxonomies supported by the service:

| Field    | Description |
|----------|-------------|
| `suggestion` | The autocompletion suggestion for the provided query. As an example, "Nursing" may be the value of this field for the query "nur" on a Skills taxonomy. |
| `id` | The unique identifier string for this result. |

&nbsp;

Result objects for the Normalized Schools taxonomy also include an additional field:

| Field    | Description |
|----------|-------------|
| `school_level` | The type of the school entity, either `SECONDARY` or `POSTSECONDARY`. |

&nbsp;

Normalized Company, Major, and School entities maintain an internal "popularity" score and results for these taxonomies will be returned in descending sorted order according to this score. Other taxonomies are not scored in this way and will return their results in alphabetical order.

&nbsp;

For multiple taxonomies the response should have the following structure

| Field    | Description |
|----------|-------------|
| `results` | A JSON array containing zero or more result objects. Each result represents an object  with a list of results  from each requested taxonomy. The result object structure in this case will be very similar to the individual taxonomy response |


All responses will be returned as JSON.

## Examples

Following are several request-response examples that demonstrate usage of the service. These examples are accompanied with links to the CB API tester page that demonstrate the requests in action. Note that you must be connected to the internal CB network and add the "API Management" Okta application to your Okta profile in order to access the API tester page.

**Carotene V3 autocompletion for "account" with maximum 3 results [link](https://apimanagement.cbplatform.link/#routes/tester?preURL=https%3A%2F%2Fapi.careerbuilder.com%2F&postURL=core%2Fautocomplete%2Fcarotenev3%3Fquery%3Daccount%26limit%3D3&method=get&contentType=application%2Fjson&acceptType=application%2Fjson&version=1.0&region=productionus&flow=client_credentials&userDid=&accountDid=&headers=&body=)**

**Request**

`https://www.api.careerbuilder.com/core/autocomplete/carotenev3?query=account&limit=3`

**Response**
```json
{
  "data": {
    "results": [
      {
        "suggestion": "Accountant",
        "id": "13.0"
      },
      {
        "suggestion": "Account Leader",
        "id": "41.10310"
      },
      {
        "suggestion": "Account Liaison",
        "id": "41.30981"
      }
    ]
  }
}
```

&nbsp;

**Normalized Company autocompletion for "careerb" [link](https://apimanagement.cbplatform.link/#routes/tester?preURL=https%3A%2F%2Fapi.careerbuilder.com%2F&postURL=core%2Fautocomplete%2Fnormalizedcompanies%3Fquery%3Dcareerb&method=get&contentType=application%2Fjson&acceptType=application%2Fjson&version=1.0&region=productionus&flow=client_credentials&userDid=&accountDid=&headers=&body=)**

**Request**

`https://www.api.careerbuilder.com/core/autocomplete/normalizedcompanies?query=careerb`

**Response**
```json
{
  "data": {
    "results": [
      {
        "suggestion": "Careerbuilder, LLC",
        "id": "NC842f69a0-b621-48f6-b53c-9b3301e9aab7"
      }
    ],
    "data_version": "2018-03-15"
  }
}
```

&nbsp;

**Normalized School autocompletion for "west", limited to postsecondary schools and maximum 10 results [link](https://apimanagement.cbplatform.link/#routes/tester?preURL=https%3A%2F%2Fapi.careerbuilder.com%2F&postURL=core%2Fautocomplete%2Fnormalizedschools%3Fquery%3Dwest%26school_level%3Dpostsecondary%26limit%3D10&method=get&contentType=application%2Fjson&acceptType=application%2Fjson&version=1.0&region=productionus&flow=client_credentials&userDid=&accountDid=&headers=&body=)**

**Request**

`https://www.api.careerbuilder.com/core/autocomplete/normalizedschools?query=west&school_level=postsecondary&limit=10`

## Sample Response

```json
{
  "data": {
    "results": [
      {
        "suggestion": "Western Michigan University",
        "id": "53bff579e4b04710d09fe614",
        "school_level": "POSTSECONDARY"
      },
      {
        "suggestion": "West Virginia University",
        "id": "53bff579e4b04710d09fe5e8",
        "school_level": "POSTSECONDARY"
      },
      {
        "suggestion": "Western Illinois University",
        "id": "53bff579e4b04710d09fe60e",
        "school_level": "POSTSECONDARY"
      },
      {
        "suggestion": "Western Washington University",
        "id": "53bff579e4b04710d09fe634",
        "school_level": "POSTSECONDARY"
      },
      {
        "suggestion": "Western Kentucky University",
        "id": "53bff579e4b04710d09fe613",
        "school_level": "POSTSECONDARY"
      },
      {
        "suggestion": "Western Governors University",
        "id": "53bff579e4b04710d09fe60c",
        "school_level": "POSTSECONDARY"
      },
      {
        "suggestion": "West Chester University of Pennsylvania",
        "id": "53bff579e4b04710d09fe5b4",
        "school_level": "POSTSECONDARY"
      },
      {
        "suggestion": "Western Carolina University",
        "id": "53bff579e4b04710d09fe603",
        "school_level": "POSTSECONDARY"
      },
      {
        "suggestion": "Westchester Community College",
        "id": "53bff579e4b04710d09fe5f2",
        "school_level": "POSTSECONDARY"
      },
      {
        "suggestion": "Westwood College",
        "id": "53bff579e4b04710d09fe656",
        "school_level": "POSTSECONDARY"
      }
    ],
    "data_version": "2018-03-29"
  }
}
```

**Multiple taxonomies request (carotenev3, school_norm]) [link](https://apimanagement.cbplatform.link/?#routes/tester?preURL=https%3A%2F%2Fwwwtest.api.careerbuilder.com%2F&postURL=%2Fcore%2Fautocomplete%2Ftaxonomies&method=post&contentType=application%2Fjson&acceptType=application%2Fjson&version=1.0&region=staging&flow=client_credentials&userDid=&accountDid=&headers=&body={+%0D%0A+++%22query%22%3A%22java%22%2C%0D%0A+++%22limit%22%3A3%2C%0D%0A+++%22taxonomies%22+%3A+[%22carotenev3%22%2C+%22school_norm%22]%0D%0A})**

**Request**

```json
{ 
   "query":"java",
   "limit":3,
   "taxonomies" : ["carotenev3", "school_norm"]
}
```
## Sample Response

```json
{
  "data": {
    "results": [
      {
        "results": [
          {
            "suggestion": "Java Analyst",
            "id": "15.11674"
          },
          {
            "suggestion": "Java Architect",
            "id": "15.128"
          },
          {
            "suggestion": "Java Developer",
            "id": "15.2"
          }
        ],
        "taxonomy": "CAROTENEV3"
      },
      {
        "results": [
          {
            "suggestion": "Jawaharlal Nehru Technological University",
            "id": "53bff579e4b04710d09fb35f",
            "school_level": "POSTSECONDARY"
          },
          {
            "suggestion": "Jazan University",
            "id": "53bff579e4b04710d09fb36f",
            "school_level": "POSTSECONDARY"
          }
        ],
        "data_version": "2018-03-29",
        "taxonomy": "SCHOOL_NORM"
      }
    ]
  }
}
```

## Versioning
The current API version is 1.0. API version must be specified in the Accept header, e.g. ```application/json;version=1.0```.

The data version is dependent on the requested taxonomy. Normalized Company, Major, and School taxonomies are unversioned and subject to change over time; the service will always use the latest version of data available. Carotene, NAICS, ONet, and Skills taxonomies are strongly versioned; a version of any of these taxonomies will never change throughout its lifetime.

Our general versioning strategy is available [here](/Versioning.md).
