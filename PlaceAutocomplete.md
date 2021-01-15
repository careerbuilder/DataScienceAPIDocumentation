Place Autocomplete
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Session Tokens and Usage Example](#session-tokens-and-usage-example)
- [Request Structure](#request-structure)
- [Response Structure](#response-structure)
- [Versioning](#versioning)

## Summary

Place Autocomplete (`/core/geography/place/autocomplete`) is a wrapper service for Google's Place 
Autocomplete API, which returns place predictions based upon a user input string. It can be used in conjunction with [Google Place Details]
to validate user input and retrieve geographic information. Please refer to 
[Google's documentation](https://developers.google.com/places/web-service/autocomplete) for information about this service. 

## Session Tokens and Usage Example

All requests require a UUID session token. Session tokens are used by Google to group multiple 
requests into one billing session. 

As an example of the intended usage of sessions, suppose a consumer of the Place Autocomplete and [Place Details](https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/PlaceDetails.md) apis has a 
user searching for a place:

1. A session token UUID `A` is generated for the session.
2. The user begins typing a place name.
3. On e.g. the third keystroke `"Har"` a request is sent to `/core/geography/place/autocomplete` with 
`input`: `Har` and `session_token`: `A`.
4. A list of predictions and their `place_id`s are generated for `"Har"` and returned in the response.
5. The user does not select any of the predictions and instead inputs another keystroke `"Harb"`.
6. `input`: `"Harb"` and `session_token`: `A` is sent in a new request.
7. A new list of predictions and their `place_id`s are generated and returned
8. The user selects one of the predictions.
9. The `place_id` for the selected prediction is sent to the Place Details service along with 
`session_token`: `A`.
10. Place Details returns geographic details (locality name, lat/lng, etc.) for the selected place.
11. A new session token is generated for any subsequent searches done by that user or any other
users (i.e. DO NOT reuse session tokens).

Using session tokens in this way greatly minimizes billing costs incurred. Please see [Google's session token
documentation](https://developers.google.com/places/web-service/session-tokens) for more information.
 
## Request Structure

Request structure for this service mirrors [Google's request structure](https://developers.google.com/places/web-service/autocomplete#place_autocomplete_requests).

#### Request

|Field|Type|Required|Example|Note|
|----|----|----|----|----|
|`input`|`string`|True|`"Harbi"`||
|`session_token`|`string`|True|-|Must be UUID.|
|`offset`|`number`|False|`2`|Must be greater than 0 and less than input lenght|
|`location`|`object`|False|-|See below for structure|
|`origin`|`object`|False|-|See below for structure|
|`radius`|`number`|False|`5000`|Meters|
|`strict_bounds`|`boolean`|False|`true`||
|`language`|`string`|False|`"et"`|See [supported languages](https://developers.google.com/maps/faq#languagesupport)|
|`components`|`array[string]`|False|`["US", "CA", "CN"]`|No more than 5 countries. Must be ISO 3166-1 Alpha-2|
|`types`|`string`|False|`"(region)"`|See [place types](https://developers.google.com/places/web-service/autocomplete#place_types)|

#### `location` and `origin`
|Field|Type|Required|Example|Note|
|----|----|----|----|----|
|`latitude`|`number`|True|45.7576||
|`longitude`|`number`|True|126.6409||

#### Examples

Only required fields:
````json
{
    "input": "Harbi",
    "session_token": "<UUID_TOKEN>"
}
````

All fields:
````json
{
    "input": "Harbi",
    "session_token": "<UUID_TOKEN>",
    "offset": 4.0,
    "location": {
        "latitude": 45.7576,
        "longitude": 126.6409
    },
    "origin": {
        "latitude": 45.818107,
        "longitude": 126.599279
    },
    "radius": 50000,
    "strict_bounds": true,
    "language": "zh-cn",
    "components": ["CN", "US", "CA", "ET", "FR"],
    "types": "(regions)"
}
````

## Response Structure

The response contains a list of up to 5 predictions. 

Each prediction contains:
- `description` string
- `place_id`
- List of `terms` and their `offset`
- Location of the `matched_substring` in the prediction result
- `structured_formatting`, pre-formatted text that can be used when displaying predictions
- `distance_meters` from `origin` (if `origin` was specified in the request)

Please see [Google's documentation](https://developers.google.com/places/web-service/autocomplete#place_autocomplete_results) 
for additional information on these fields. 

#### Example
```json
{
  "data": {
    "predictions": [
      {
        "description": "Harbin, 黑龙江省中国",
        "place_id": "ChIJYRRkpvhkQ14R1SygWnOSfF4",
        "types": [
          "locality",
          "political",
          "geocode"
        ],
        "terms": [
          {
            "offset": 0,
            "value": "Harbin"
          },
          {
            "offset": 8,
            "value": "黑龙江省"
          },
          {
            "offset": 12,
            "value": "中国"
          }
        ],
        "structured_formatting": {
          "main_text": "Harbin",
          "secondary_text": "黑龙江省中国",
          "main_text_matched_substrings": [
            {
              "length": 4,
              "offset": 0
            }
          ]
        },
        "matched_substring": [
          {
            "length": 4,
            "offset": 0
          }
        ],
        "distance_meters": 5233
      },
      {
        "description": "Nangang, 哈尔滨市黑龙江省中国",
        "place_id": "ChIJIcgm1bB5RF4RZA29COpGXqg",
        "types": [
          "sublocality_level_1",
          "sublocality",
          "political",
          "geocode"
        ],
        "terms": [
          {
            "offset": 0,
            "value": "Nangang"
          },
          {
            "offset": 9,
            "value": "哈尔滨市"
          },
          {
            "offset": 13,
            "value": "黑龙江省"
          },
          {
            "offset": 17,
            "value": "中国"
          }
        ],
        "structured_formatting": {
          "main_text": "Nangang",
          "secondary_text": "哈尔滨市黑龙江省中国",
          "main_text_matched_substrings": [
            {
              "length": 7,
              "offset": 0
            }
          ]
        },
        "matched_substring": [
          {
            "length": 7,
            "offset": 0
          }
        ],
        "distance_meters": 8416
      }
    ]
  }
}
```

In the above example, the `place_id` value in the response could then be sent to [Place Details](https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/PlaceDetails.md) to retrieve relevant geo information about this locality. Please see the [request](https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/PlaceDetails.md#example-request) and [response](https://github.com/careerbuilder/DataScienceAPIDocumentation/blob/master/PlaceDetails.md#examples] sections of Place Details documentation, which continue the above example and show this locations place information.

## Versioning

The current version of the service is 1.0. 

Version must be specified in the Accept header. E.g. `application/json;version=1.0`. 

Our general versioning strategy is available [here](/Versioning.md).
