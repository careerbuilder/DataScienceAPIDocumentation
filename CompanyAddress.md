Company Address Service
==================

#### Table of Contents
_______

- [Summary](#summary)
- [Request structure](#request-structure)
- [Response structure](#response-structure)
- [Sample response](#sample-response)
- [Versioning](#versioning)

## Summary

The Company Address Service takes a request containing a company name, latitude and longitude and returns a response describing a normalized geographic location.

## Request Structure
```json
{
	"latitude": 59.439675,
	"longitude": 24.746831,
	"company_name": "Texas Honky Tonk & Cantina"
}
```

### Response Structure

All responses with an HTTP status of 200 will consist of a JSON object with a top-level "data" node containing the following elements:

| Field    | Description |
|----------|-------------|
| results  | A JSON array of one or more objects containing geography data. If the request returns no results, the array will be empty. |
| expiration_date | A date string in YYYY-MM-DD format representing the last day that this data may be used. Per our contract with Google, any data cached by users of the this service must be discarded by this date.

&nbsp;

Each element of the returned **results** array will be formatted as follows:

|   |   |
|---|---|
| admin_areas | A JSON array of up to 5 admin_area objects. See below for details on the structure of these objects. **This property is always present, but may be empty** if no admin areas were found. |
| latitude | A double value specifying the location's latitude (in degrees) within the range [-90, +90]. **This property is always present.** |
| longitude | A double value specifying the location's longitude (in degrees) within the range [-180, +180]. **This property is always present.** |
| country | The country or region name of an address. **This property may be absent** |
| locality | The populated place for the address. This typically refers to a city, but may refer to a suburb or a neighborhood in certain countries. **This property may be absent**. |
| postal_code | The post code, postal code, or ZIP code of an address. **This property may be absent**. |
| country_code | The two-letter ISO country code. **This property may be absent**. |
| formatted_address| A string containing the human-readable address of this location. Often this address is equivalent to the "postal address," which sometimes differs from country to country. (Note that some countries, such as the United Kingdom, do not allow distribution of true postal addresses due to licensing restrictions.) **This property may be absent**. |
| location_type | The classification of the geographic entity returned. For Company Address searches this will always be STREET_ADDRESS. |
| postcode_localities | A string array denoting all the localities contained in a postal code. **This property is only present when the result is a postal code that contains multiple localities,** and will never be empty (nor will it ever contain only one element). |
| street_address | The official street line of an address relative to the area, as specified by the `locality` or `postal_code` properties. **This property may be absent**. |
| sublocality | The neighborhood, borough, township, etc. for the address. Sublocalities are typically more specific than cities and may be returned even if the locality field is empty. **This property may be absent**. |
| landmark | The full name of the landmark (such as a military base or island) returned by the service. **This property may be absent**. |
| place_id | Google's unique identifier string corresponding to the returned location. |
| viewport | A bounding box describing a rectangle that encloses the location. The viewport field contains two coordinate objects -- **northeast** and **southwest** -- each with **lat** and **lng** decimal values. It also contains a **suggested_radius** decimal field that contains the distance in miles from the center of the viewport to a corner. **This property and its elements are always present and non-empty.** |
| metropolitan_statistical_area | The metropolitan statistical area containing the location. An object containing two fields, the title of the area and an integer code for the area. Metropolitan statistical areas are specific to the US. **This property may be absent**. |
| designated_market_areas | An object array of the designated market areas containing the location. Designated market areas may overlap and thus a location may be in more than one designated market area. Designated market areas are specific to the continental US. See below for details on the structure of these objects. **This property may be absent and the array may be empty**. |
| partial_match | A boolean value indicating that the geocoder did not return an exact match. **This property will always be false for Company Address searches**.|

&nbsp;

Each element of the returned **admin_areas** array will be formatted as follows:

|   |   |
|---|---|
| short_name | The abbreviated form of the administrative area's name. This will vary by country and administrative division level. In the US, states (admin_area level 1) will use the state abbreviation (e.g. "GA"), and counties will use the full county name (e.g. "Gwinnett County"). **This property is always present and non-empty.** |
| long_name | The unabbreviated form of the administrative area's name. **This property is always present and non-empty.** |
| level | An integer between 1 and 5 inclusive specifying the hierarchal level at which this administrative area resides. In the US, states will be returned with a level of 1 and counties with a level of 2. **This property is always present and non-empty.** |

&nbsp;

Each element of the returned **designated_market_areas** array will be formatted as follows:

|   |   |
|---|---|
| name | The name of the DMA, e.g. "Atlanta, GA." **This property is always present and non-empty.** |
| id | The integer ID of the DMA. E.g. for the Atlanta, GA DMA, the ID is 524. **This property is always present and non-empty.** |

&nbsp;

## Sample Response

```json
{
    "data": {
        "results": [
            {
                "admin_areas": [
                    {
                        "long_name": "Harju maakond",
                        "short_name": "Harju maakond",
                        "level": 1
                    }
                ],
                "country": "Estonia",
                "country_code": "EE",
                "formatted_address": "Pikk 43, 10133 Tallinn, Estonia",
                "latitude": 59.4396784,
                "longitude": 24.7469663,
                "locality": "Tallinn",
                "location_type": "STREET_ADDRESS",
                "postal_code": "10133",
                "street_address": "43 Pikk",
                "sublocality": "Kesklinn",
                "place_id": "ChIJVZfruGOTkkYR4Ci9TcrEXFQ",
                "viewport": {
                    "northeast": {
                        "lat": 59.4410273802915,
                        "lng": 24.7483152802915
                    },
                    "southwest": {
                        "lat": 59.43832941970849,
                        "lng": 24.7456173197085
                    },
                    "suggested_radius_miles": 0.10459088520608802
                },
                "partial_match": false
            }
        ],
        "expiration_date": "2018-04-22"
    }
}
```

## Versioning
The current version is 1.0. 

Version must be specified in the Accept header. E.g. ```application/json;version=1.0```. 

Our general versioning strategy is available [here](/Versioning.md).
