#Geocoding API

&nbsp;

#Contents

- [Description](#description)
- [Geocoding Request](#goecoding-request)
- [Geocoding Request Structure](#request-structure)
- [Place ID Lookup Request](#place-id-lookup-request)
- [Place ID Lookup Request Structure](#place-id-lookup-request-structure)
- [Response Structure](#response-structure)
- [Versioning](#versioning)

----------------------------
#Description

Accepts structured or unstructured location information and attempts to resolve the input to a geographic coordinate. If successful, the response will contain one (or more, if requested) locations with latitude and longitude information as well as any available administrative location information, such as country and state. This service relies on Google's Geocoding API as its data vendor; Google's service is documented [here](https://developers.google.com/maps/documentation/geocoding/intro). This service supports the HTTP/GET method only. Lastly, this service makes use of two different requests available via Google: Geocoding Requests and Google Place ID Lookup.

----------------------------
#Geocoding Request
A Geocoding request contains at least one of (query, address, locality, postal_code, admin_area, country) that must be provided in the query string.

#Geocoding Request Structure

Example URL: https://api.careerbuilder.com/core/geography/geocoding?address=5550%20Peachtree%20Pkwy&locality=Norcross&admin_area=GA&postal_code=30092&country=US


| Parameter  | Description |
|------------|-------------|
| address      | Optional. A string containing address information, e.g. "5550 Peachtree Pkwy."" Other forms of location data may be passed in this field (such as postal codes or country names), but we recommend using the type-specific parameters when the location type of the data is known. This parameter is handled in the same way as the **query** parameter and the two cannot be used simultaneously. |
| query      | Optional. A string containing location information in any format, e.g. "5550 Peachtree Pkwy, Norcross, GA 30092."" We recommend using the type-specific parameters when the location type of the data is known. This parameter is handled in the same way as the **address** parameter and the two cannot be used simultaneously. |
| locality   | Optional. The locality, such as the city or neighborhood, that corresponds to an address. |
| admin_area  | Optional. The subdivision name in the country or region for an address. This element is typically treated as the first order administrative subdivision, but in some cases it is the second, third, or fourth order subdivision in a country, dependency, or region. |
| country    | Optional. A country name or two-letter ISO-3166 country code. Used to limit the geographic scope of results to a particular country. Multiple pipe-delimited values may be supplied to limit results to a set of specified countries, e.g. ?query=Rome&country=IT&#124;US. |
| postal_code | Optional. The post code, postal code, or ZIP Code of an address. |
| culture    | Optional. The preferred ISO 639-1 language code for the response. If this parameter is omitted, the response will be returned in English. Note that the level of localization will vary for each culture. For example, the name "United States" may not have a localized name for every culture code. The geocoding service currently only supports a subset of the ISO-639-1 standard. Click [here](Data/GeocodingServiceSupportedLanguages.md) to view a full list of supported culture codes. |
| territories_as_states | Optional. When enabled, dependent territories may be requested as administrative areas of their parent nation, and will also be returned as such. This parameter is necessary to validate, for instance, **locality=San Juan&admin_area=PR&country=US**. This functionality will only recognize territories and parent countries by their [two-digit ISO-3166-1 country codes](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2), so requests such as **admin_area=Puerto Rico&country=USA** would not be remapped according to this parameter. Default value is false. |
| try_locality_as_admin_area | Optional. When enabled, a request that includes the **locality** parameter and fails to retrieve locality-specific geography data will be reattempted with the locality value sent as an admin_area value instead. (If an admin_area value already exists, the locality value will be prepended and comma-separated, e.g. "locality=Harris%20Township&admin_area=OH" would be resent as "admin_area=Harris Township, OH" if try_locality_as_admin_area is set to true.) Default value is false. |

----------------------

#Place ID Lookup Request

A Google Place ID is a unique identifier used by Google to identify location/places in the Google Places Databases and on Google Maps. The service accepts a valid place ID as a means to retrieve goeographic information back from google via reverse geocoding. Requesting a location using a Google place ID requires that the request contains a place ID and a culture param if a language specification is desired. Additional location parameters will result in a 400 response in this request.

#Place ID Lookup Request Structure

Example URL: https://api.careerbuilder.com/core/geography/geocoding?culture=en&place_id=ChIJd8BlQ2BZwokRAFUEcm_qrcA

| Parameter  | Description |
|------------|-------------|
| place_id | Required. Id provided by google that uniquely identifies locations based on the Google Places Database and in Google Maps.
| culture    | Optional. The preferred ISO 639-1 language code for the response. If this parameter is omitted, the response will be returned in English. Note that the level of localization will vary for each culture. For example, the name "United States" may not have a localized name for every culture code. The geocoding service currently only supports a subset of the ISO-639-1 standard. Click [here](Data/GeocodingServiceSupportedLanguages.md) to view a full list of supported culture codes. |

&nbsp;

----------------------
#Response Structure

All responses with an HTTP status of 200 will consist of a JSON object with a top-level "data" node containing the following elements:

| Field    | Description |
|----------|-------------|
| results  | A JSON array of one or more objects containing geography data. If the request returns no results, the array will be empty. |
| expiration_date | A date string in YYYY-MM-DD format representing the last day that this data may be used. Per our contract with Google, any data cached by users of the geocoding service must be discarded by this date.

&nbsp;

Each element of the returned **results** array will be formatted as follows:

|   |   |
|---|---|
| admin_areas | A JSON array of up to 5 admin_area objects. See below for details on the elements of this array. **This property is always present, but may be empty** if no admin areas were found. |
| latitude | A double value specifying the location's latitude (in degrees) within the range [-90, +90]. **This property is always present.** |
| longitude | A double value specifying the location's longitude (in degrees) within the range [-180, +180]. **This property is always present.** |
| country | The country or region name of an address. **This property may be absent** if the location was not associated with a country. Some locations for which this occurs are "Europe", "Pacific Ocean", and "Gaza Strip" (a contested territory). |
| locality | The populated place for the address. This typically refers to a city, but may refer to a suburb or a neighborhood in certain countries. **This property may be absent**. |
| postal_code | The post code, postal code, or ZIP code of an address. **This property may be absent**. |
| country_code | The two-letter ISO country code. **This property may be absent**. See "Country" for examples of when this might occur. |
| formatted_address| A string containing the human-readable address of this location. Often this address is equivalent to the "postal address," which sometimes differs from country to country. (Note that some countries, such as the United Kingdom, do not allow distribution of true postal addresses due to licensing restrictions.) **This property may be absent**. |
| location_type | The classification of the geographic entity returned. Possible values (in descending order of specificity) are STREET_ADDRESS, SUBLOCALITY, POSTAL_CODE, LOCALITY, ADMIN_AREA_2, ADMIN_AREA_1, COORDINATE, STREET, COUNTRY, and UNKNOWN. COORDINATE is used when the location type returned by Google is "natural_feature", "park", "airport", or "point_of_interest". Otherwise, the returned location type will reflect the most specific field present, based on the previously described order. If none of these fields are present (such as for the examples for which the Country field is absent), then the returned location type will be Unknown. **This property is always present and non-empty.** |
| postcode_localities | A string array denoting all the localities contained in a postal code. **This property is only present when the result is a postal code that contains multiple localities,** and will never be empty (nor will it ever contain only one element). |
| street_address | The official street line of an address relative to the area, as specified by the Locality, or PostalCode, properties. **This property may be absent**. |
| sublocality | The neighborhood, borough, township, etc. for the address. Sublocalities are typically more specific than cities and may be returned even if the locality field is empty. **This property may be absent**. |
| landmark | The full name of the landmark (such as a military base or island) returned by the service. **This property may be absent**. |
| place_id | Place Id relating to the geolocation requested. 
| viewport | A bounding box describing a rectangle that encloses the location. The viewport field contains two coordinate objects -- **northeast** and **southwest** -- each with **lat** and **lng** decimal values. It also contains a **suggested_radius** decimal field that contains the distance in miles from the center of the viewport to a corner. **This property and its elements are always present and non-empty.** |
| metropolitan_statistical_area | The metropolitan statistical area containing the location. An object containing two fields, the title of the area and an integer code for the area. Metropolitan statistical areas are specific to the US. **This property may be absent**. |
| designated_market_areas | A string array of the designated market areas containing the location. Designated market areas may overlap and thus a location may be in more than one designated market area. Designated market areas are specific to the continental US. **This property may be absent**. |
| partial_match | A Boolean value that, when true, indicates that the geocoder did not return an exact match for the original request, though it was able to match part of the requested address. You may wish to examine the original request for misspellings and/or an incomplete address.<br>Partial matches most often occur for street addresses that do not exist within the locality you pass in the request. Partial matches may also be returned when a request matches two or more locations in the same locality. For example, "21 Henr St, Bristol, UK" will return a partial match for both Henry Street and Henrietta Street. Note that if a request includes a misspelled address component, the geocoding service may suggest an alternative address. Suggestions triggered in this way will also be marked as a partial match. **This property is always present.** |

&nbsp;

Each element of the returned **admin_areas** array will be formatted as follows:

|   |   |
|---|---|
| short_name | The abbreviated form of the administrative area's name. This will vary by country and administrative division level. In the US, states (admin_area level 1) will use the state abbreviation (e.g. "GA"), and counties will use the full county name (e.g. "Gwinnett County"). **This property is always present and non-empty.** |
| long_name | The unabbreviated form of the administrative area's name. **This property is always present and non-empty.** |
| level | An integer between 1 and 5 inclusive specifying the hierarchal level at which this administrative area resides. In the US, states will be returned with a level of 1 and counties with a level of 2. **This property is always present and non-empty.** |

&nbsp;

-----------
#Versioning
The current API version is 1.0. The data returned from the service is unversioned.  

Geography is constantly changing as postal codes, cities, states, and countries are created and destroyed.  Google, our vendor, doesn't publish an update policy and changes things as they see fit.

Our general versioning strategy is available [here](/Versioning.md).
