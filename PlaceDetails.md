Place Details
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request Structure](#request-structure)
- [Response Structure](#response-structure)
- [Versioning](#versioning)

## Summary

Place Details (`/core/geography/place/details`) is a wrapper service for 
[Google's Place Details API.](https://developers.google.com/places/web-service/details), which 
returns geographic and place information for a given [place ID](https://developers.google.com/places/web-service/place-id). 

## Request Structure

Request structure for this service mirrors 
[Google's request structure](https://developers.google.com/places/web-service/details#PlaceDetailsRequests),
with a few notable exceptions:

1. `session_token` is required.
2. At least one `fields` value is required.
3. Not all `fields` are supported.

#### Supported `fields`

| `field` name | Supported? |
|------------|------------|
|`address_component`|`true`|
|`adr_address`|`true`|
|`business_status`|`true`|
|`formatted_address`|`true`|
|`geometry/location`|`true`|
|`geometry/viewport`|`true`|
|`icon`|`false`|
|`name`|`true`|
|`photo`|`false`|
|`place_id`|`true`|
|`plus_code`|`true`|
|`type`|`true`|
|`url`|`true`|
|`utc_offset`|`true`|
|`vicinity`|`true`
|`formatted_phone_number`<sup>†</sup>|`true`|
|`international_phone_number`<sup>†</sup>|`true`|
|`opening_hours`|`false`|
|`website`<sup>†</sup>|`true`
|`price_level`|`false`|
|`rating`|`false`|
|`review`|`false`|
|`user_ratings_total`|`false`|

<sup>†</sup> Note that `formatted_phone_number`, `international_phone_number` and `website` are __billed at a 
higher rate__ than other fields. Consider omitting these fields from your request if they are not 
needed.

If you would like to request support for any of the currently unsupported fields, contact DSAD at 
[DataScienceApplicationDevelopment@careerbuilder.com](DataScienceApplicationDevelopment@careerbuilder.com).

#### Request Fields

|Field|Type|Required|Example|Note|
|----|----|----|----|----|
|`place_id`|`string`|True|`"ChIJIcgm1bB5RF4RZA29COpGXqg"`||
|`session_token`|`string`|True|-|Must be UUID|
|`fields`|`array[string]`|True|`["name", "address_component"]`|At least one field is required. See [Google's documentation on `fields`](https://developers.google.com/places/web-service/place-data-fields) and [results](https://developers.google.com/places/web-service/details#PlaceDetailsResults) for more information.|
|`language`|`string`|False|`"et"`|See [supported languages](https://developers.google.com/maps/faq#languagesupport)|

#### Example Request

```json
{
    "place_id": "ChIJYRRkpvhkQ14R1SygWnOSfF4",
    "session_token": "<UUID_TOKEN>",
    "fields": [
        "address_component",
        "adr_address",
        "business_status",
        "formatted_address",
        "formatted_phone_number",
        "geometry/location",
        "geometry/viewport",
        "international_phone_number",
        "name",
        "place_id",
        "plus_code",
        "types",
        "url",
        "utc_offset",
        "vicinity",
        "website"
    ],
    "language": "et"
}
```

## Response Structure
 
Please refer to Google's Place Details [Response](https://developers.google.com/places/web-service/details#PlaceDetailsResponses) 
and [Results](https://developers.google.com/places/web-service/details#PlaceDetailsResults) 
for a description of the various response fields.

### Examples

#### Locality

```json
{
  "data": {
    "address_components": [
      {
        "long_name": "Harbin",
        "short_name": "Harbin",
        "types": [
          "LOCALITY",
          "POLITICAL"
        ]
      },
      {
        "long_name": "Heilongjiang",
        "short_name": "Heilongjiang",
        "types": [
          "ADMINISTRATIVE_AREA_LEVEL_1",
          "POLITICAL"
        ]
      },
      {
        "long_name": "Hiina",
        "short_name": "CN",
        "types": [
          "COUNTRY",
          "POLITICAL"
        ]
      }
    ],
    "adr_address": "<span class=\"locality\">Harbin</span>, <span class=\"region\">Heilongjiang</span>, <span class=\"country-name\">Hiina</span>",
    "formatted_address": "Harbin, Heilongjiang, Hiina",
    "geometry": {
      "location": {
        "latitude": 45.803775,
        "longitude": 126.534967
      },
      "viewport": {
        "northeast": {
          "latitude": 45.8838096,
          "longitude": 126.8799341
        },
        "southwest": {
          "latitude": 45.6306503,
          "longitude": 126.4241087
        }
      }
    },
    "name": "Harbin",
    "place_id": "ChIJYRRkpvhkQ14R1SygWnOSfF4",
    "types": [
      "LOCALITY",
      "POLITICAL"
    ],
    "url": "https://maps.google.com/?q=Harbin,+Heilongjiang,+Hiina&ftid=0x5e4364f8a6641461:0x5e7c92735aa02cd5",
    "utc_offset": 480,
    "vicinity": "Harbin",
    "website": "http://www.harbin.gov.cn/"
  }
}
```

#### Business

```json
{
  "data": {
    "address_components": [
      {
        "long_name": "5231",
        "short_name": "5231",
        "types": [
          "STREET_NUMBER"
        ]
      },
      {
        "long_name": "Buford Highway Northeast",
        "short_name": "Buford Hwy NE",
        "types": [
          "ROUTE"
        ]
      },
      {
        "long_name": "Doraville",
        "short_name": "Doraville",
        "types": [
          "LOCALITY",
          "POLITICAL"
        ]
      },
      {
        "long_name": "DeKalb County",
        "short_name": "Dekalb County",
        "types": [
          "ADMINISTRATIVE_AREA_LEVEL_2",
          "POLITICAL"
        ]
      },
      {
        "long_name": "Georgia",
        "short_name": "GA",
        "types": [
          "ADMINISTRATIVE_AREA_LEVEL_1",
          "POLITICAL"
        ]
      },
      {
        "long_name": "Ameerika Ühendriigid",
        "short_name": "US",
        "types": [
          "COUNTRY",
          "POLITICAL"
        ]
      },
      {
        "long_name": "30340",
        "short_name": "30340",
        "types": [
          "POSTAL_CODE"
        ]
      }
    ],
    "adr_address": "<span class=\"street-address\">5231 Buford Hwy NE</span>, <span class=\"locality\">Doraville</span>, <span class=\"region\">GA</span> <span class=\"postal-code\">30340</span>, <span class=\"country-name\">Ameerika Ühendriigid</span>",
    "business_status": "OPERATIONAL",
    "formatted_address": "5231 Buford Hwy NE, Doraville, GA 30340, Ameerika Ühendriigid",
    "formatted_phone_number": "(678) 691-2175",
    "geometry": {
      "location": {
        "latitude": 33.8948497,
        "longitude": -84.2819859
      },
      "viewport": {
        "northeast": {
          "latitude": 33.8962456802915,
          "longitude": -84.2807332197085
        },
        "southwest": {
          "latitude": 33.89354771970851,
          "longitude": -84.2834311802915
        }
      }
    },
    "international_phone_number": "+1 678-691-2175",
    "name": "LanZhou Ramen 兰州拉面",
    "place_id": "ChIJZ8obVM4J9YgR_JlLF3EF1EM",
    "plus_code": {
      "global_code": "865QVPV9+W6",
      "compound_code": "VPV9+W6 Doraville, GA, Ameerika Ühendriigid"
    },
    "types": [
      "RESTAURANT",
      "FOOD",
      "POINT_OF_INTEREST",
      "ESTABLISHMENT"
    ],
    "url": "https://maps.google.com/?cid=4887537478884104700",
    "utc_offset": -240,
    "vicinity": "5231 Buford Highway Northeast, Doraville",
    "website": "http://lanzhouramenatlanta.com/"
  }
}
```


## Versioning

The current version of the service is 1.0. 

Version must be specified in the Accept header. E.g. `application/json;version=1.0`. 

Our general versioning strategy is available [here](/Versioning.md).
