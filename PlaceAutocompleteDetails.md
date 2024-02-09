Place Autocomplete Details
==========================

#### Table of Contents
_______

- [Summary](#summary)
- [Request Structure](#request-structure)
- [Response Structure](#response-structure)
- [Versioning](#versioning)

## Summary

Place Autocomplete Details (`/core/geography/place/autocompletedetails`) service that wraps the functionalities of both the Place Autocomplete 
and Place Details APIs. The Place Autocomplete API generates place predictions based on user input, and for the first prediction, 
it invokes the Place Details API to retrieve the defined geographic and place fields ie `address_components`, `formatted_address`,
`geometry`, `place_id` and `types`.


## Request Structure

|Field|Type|Required|Example|Note|
|----|----|----|----|----|
|`input`|`string`|True|`"Harbi"`||
|`session_token`|`string`|True|-|Must be UUID.|

#### Examples

````json
{
    "input": "La rochette",
    "session_token": "<UUID_TOKEN>"
}
````

## Response Structure

The response contains a detail response

Each detail response contains:
- `address_components`
- `place_id`
- `formatted_address`
- `geometry`
- `types`

#### Example
```json
{
  "data": {
    "geo": {
      "address_components": [
        {
          "long_name": "La Rochette",
          "short_name": "La Rochette",
          "types": [
            "LOCALITY",
            "POLITICAL"
          ]
        },
        {
          "long_name": "Seine-et-Marne",
          "short_name": "Seine-et-Marne",
          "types": [
            "ADMINISTRATIVE_AREA_LEVEL_2",
            "POLITICAL"
          ]
        },
        {
          "long_name": "Île-de-France",
          "short_name": "IDF",
          "types": [
            "ADMINISTRATIVE_AREA_LEVEL_1",
            "POLITICAL"
          ]
        },
        {
          "long_name": "France",
          "short_name": "FR",
          "types": [
            "COUNTRY",
            "POLITICAL"
          ]
        },
        {
          "long_name": "77000",
          "short_name": "77000",
          "types": [
            "POSTAL_CODE"
          ]
        }
      ],
      "formatted_address": "77000 La Rochette, France",
      "geometry": {
        "location": {
          "latitude": 48.508348,
          "longitude": 2.662553
        }
      },
      "place_id": "ChIJmQVNw7_w5UcRYXRdIUEpBgo",
      "types": [
        "LOCALITY",
        "POLITICAL"
      ]
    }
  }
}
```

## Versioning

The current version of the service is 1.0. 

Version must be specified in the Accept header. E.g. `application/json;version=1.0`. 

Our general versioning strategy is available [here](/Versioning.md).

# Batch Place Autocomplete Details With CallBack


#### Table of Contents
_______

- [Summary](#batch-autocomplete-summary)
- [Request Structure](#batch-request)
- [Response Structure](#batch-response)
- [CallBack Response Structure](#callback-response)
- [CallBack Error Response Structure](#callback-error-response)
- [Versioning](#batch-versioning)

## Batch Autocomplete Summary

Place Autocomplete Details (`/core/geography/place/autocompletedetails-batch`) is for batch processing 
and accepts a list of inputs. It wraps the functionalities of both the Place Autocomplete and Place Details APIs. 
The Place Autocomplete API generates place predictions corresponding to the provided input.
Subsequently, from the list of predictions, the service selects the first prediction
and then triggers the Place Details API to retrieve the defined geographic and place fields ie
`address_components`, `formatted_address`, `geometry`, `place_id` and `types` providing an aggregated information
of all the inputs and is posted to the specified `call_back_uri`.


## Batch Request
The API accepts a list of inputs along with an ID.

````json
{
  "geo":[
    {
      "id":"bjea562179-169a-5dea-b87f-f08010b9cbfc",
      "input":"chicago"
    },
    {
      "id":"ajea562179-069a-4dea-a87f-e08010b9cbfc",
      "input":"norcross"
    }
  ],
  "session_token": "<UUID_TOKEN>",
  "call_back_uri": "https://api.careerbuilder.com/handle_call_back"
}
````
**Note**: Ensure that the `call_back_uri` is a valid URI routed through the Zuul layer.

## Batch Response
Upon receiving a request, the API immediately validates the request information which includes `call_back_uri` and return 
a 200 response, ensuring that the request was successfully received.

#### Example
```json
{
  "data": {
    "status": "OK",
    "call_back_uri": "https://api.careerbuilder.com/handle_call_back"
  }
}
```

## CallBack Response
Following the initial response, the API proceeds to process each input. For the first prediction generated 
by the Autocomplete API, the API queries the Details endpoint to retrieve the defined geographic and 
place information. Once all inputs are processed, the API posts a response to the specified `call_back_uri` URL,
providing the aggregated information.

#### Example
```json
[
  {
    "id":"bjea562179-169a-5dea-b87f-f08010b9cbfc",
    "geo":{
      "address_components":[
        {
          "long_name":"Chicago",
          "short_name":"Chicago",
          "types":[
            "LOCALITY",
            "POLITICAL"
          ]
        },
        {
          "long_name":"Cook County",
          "short_name":"Cook County",
          "types":[
            "ADMINISTRATIVE_AREA_LEVEL_2",
            "POLITICAL"
          ]
        },
        {
          "long_name":"Illinois",
          "short_name":"IL",
          "types":[
            "ADMINISTRATIVE_AREA_LEVEL_1",
            "POLITICAL"
          ]
        },
        {
          "long_name":"United States",
          "short_name":"US",
          "types":[
            "COUNTRY",
            "POLITICAL"
          ]
        }
      ],
      "formatted_address":"Chicago, IL, USA",
      "geometry":{
        "location":{
          "latitude":41.8781136,
          "longitude":-87.6297982
        }
      },
      "place_id":"ChIJ7cv00DwsDogRAMDACa2m4K8",
      "types":[
        "LOCALITY",
        "POLITICAL"
      ],
      "metropolitan_statistical_area":{
        "title":"Chicago-Naperville-Elgin, IL-IN-WI",
        "code":16980
      },
      "designated_market_areas":[
        {
          "name":"Chicago, IL",
          "id":602
        }
      ]
    }
  },
  {
    "id":"ajea562179-069a-4dea-a87f-e08010b9cbfc",
    "geo":{
      "address_components":[
        {
          "long_name":"Norcross",
          "short_name":"Norcross",
          "types":[
            "LOCALITY",
            "POLITICAL"
          ]
        },
        {
          "long_name":"Gwinnett County",
          "short_name":"Gwinnett County",
          "types":[
            "ADMINISTRATIVE_AREA_LEVEL_2",
            "POLITICAL"
          ]
        },
        {
          "long_name":"Georgia",
          "short_name":"GA",
          "types":[
            "ADMINISTRATIVE_AREA_LEVEL_1",
            "POLITICAL"
          ]
        },
        {
          "long_name":"United States",
          "short_name":"US",
          "types":[
            "COUNTRY",
            "POLITICAL"
          ]
        }
      ],
      "formatted_address":"Norcross, GA, USA",
      "geometry":{
        "location":{
          "latitude":33.9411081,
          "longitude":-84.2137443
        }
      },
      "place_id":"ChIJqTKlbTih9YgRVjJ2xmfcfJQ",
      "types":[
        "LOCALITY",
        "POLITICAL"
      ],
      "metropolitan_statistical_area":{
        "title":"Atlanta-Sandy Springs-Alpharetta, GA",
        "code":12060
      },
      "designated_market_areas":[
        {
          "name":"Atlanta, GA",
          "id":524
        }
      ]
    }
  }
]
```
If no predictions found:
```json
[
  {
    "id":"bjea562179-169a-5dea-b87f-f08010b9cbfc",
    "geo":{}
  },
  {
    "id":"ajea562179-069a-4dea-a87f-e08010b9cbfc",
    "geo":{}
  }
]
```

## CallBack Error Response
If any error happens during the process, error response will be posted to the given `call_back_uri`.
Errors may originate from two services, autocomplete and details. 
In the case of an error from the autocomplete service, the response structure is:
Ex:
```json
[
  {
    "id":"bjea562179-169a-5dea-b87f-f08010b9cbfc",
    "error":"An error occurred in the autocomplete service",
    "geo":{}
  }
]
```
In the case of an error from the details service, the response structure is:

```json
[
  {
    "id":"bjea562179-169a-5dea-b87f-f08010b9cbfc",
    "error":"An error occurred in the details service",
    "geo":{}
  }
]
```
For errors originating from the autocompletedetails service, the response structure is:
```json
{"error": "bad gateway"}
```

## Batch Versioning

The current version of the service is 1.0.

Version must be specified in the Accept header. E.g. `application/json;version=1.0`.

Our general versioning strategy is available [here](/Versioning.md).