#Geovalidation API

&nbsp;

#Contents

- [Description](#description)
- [Request Structure](#request-structure)
- [Response Structure](#response-structure)
- [Versioning](#versioning)

----------------------------
#Description

Accepts structured location data and attempts to geocode the data using the validation strategies specified in the request. Each successful geocoding call is verified against the list of acceptable location types specified in the request. The outcome of each strategy is described in the response, along with the outputs of the geocoding calls. This service supports the HTTP/GET method only.

----------------------------
#Request Structure

Example URL: https://api.careerbuilder.com/core/geography/validation?locality=atlanta&country=US&validation_strategies=FREETEXT&accepted_location_types=locality

At least one of (query, locality, postal_code, admin_area, country) must be provided in the query string, as well as a "validation_strategies" parameter and an "accepted_location_types" parameter, each a comma-separated list of one or more values.


| Parameter  | Description |
|------------|-------------|
| address      | Optional. A string containing address information, e.g. "5550 Peachtree Pkwy."" Other forms of location data may be passed in this field (such as postal codes or country names), but we recommend using the type-specific parameters when the location type of the data is known. |
| locality   | Optional. The locality, such as the city or neighborhood, that corresponds to an address. |
| admin_area  | Optional. The subdivision name in the country or region for an address. This element is typically treated as the first order administrative subdivision, but in some cases it is the second, third, or fourth order subdivision in a country, dependency, or region. |
| country    | Optional. A country name or two-letter ISO-3166 country code. Used to limit the geographic scope of results to a particular country. Multiple pipe-delimited values may be supplied to limit results to a set of specified countries, e.g. ?query=Rome&country=IT&#124;US. |
| postal_code | Optional. The post code, postal code, or ZIP Code of an address. |
| culture    | Optional. The preferred ISO 639-1 language code for the response. If this parameter is omitted, the response will be returned in English. Note that the level of localization will vary for each culture. For example, the name "United States" may not have a localized name for every culture code. The geocoding service currently only supports a subset of the ISO-639-1 standard. Click [here](Data/GeographyServiceSupportedLanguages.md) to view a full list of supported culture codes. |
| territories_as_states | Optional. When enabled, dependent territories may be requested as administrative areas of their parent nation, and will also be returned as such. This parameter is necessary to validate, for instance, **locality=San Juan&admin_area=PR&country=US**. This functionality will only recognize territories and parent countries by their [two-digit ISO-3166-1 country codes](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2), so requests such as **admin_area=Puerto Rico&country=USA** would not be remapped according to this parameter. Default value is false. |
| try_locality_as_admin_area | Optional. When enabled, a request that includes the **locality** parameter and fails to retrieve locality-specific geography data will be reattempted with the locality value sent as an admin_area value instead. (If an admin_area value already exists, the locality value will be prepended and comma-separated, e.g. "locality=Harris%20Township&admin_area=OH" would be resent as "admin_area=Harris Township, OH" if try_locality_as_admin_area is set to true.) Default value is false. |
| validation_strategies | Required. [Validation Strategies](#Validation Strategies) for making the geocoding request. |
| accepted_location_types | Required. [Location types](#Location types) to validate the geocoding response by. |

&nbsp;
#Validation Strategies

| Validation Strategy | Description|
|----------|-------------|
| FIELDED | Makes a Geocoder call with admin area, address, country, locality, and postal code |
| FIELDED_NO_ADDRESS | Makes a Geocoder call with the provided admin area, country, locality, and postal code |
| FREETEXT | Makes a Geocoder call with the query parameter equal to the formatted string "address, locality, admin area, country, postal code"  |
| POSTAL_CODE_ONLY |  Makes a Geocoder call with postal code and country parameters set to the provided postal code and country parameters |

*Additional strategies can be implemented as needed. Please contact DataScienceApplicationDevelopment if your use case requires a strategy that's not currently available. *
&nbsp;

&nbsp;
#Location Types

| Location Types |
|----------|
| ADMIN_AREA1 |
| ADMIN_AREA2 |
| COORDINATE |
| COUNTRY |
| LOCALITY |
| POSTAL_CODE |
| STREET |
| STREET_ADDRESS |
| SUBLOCALITY |
| UNKNOWN |

*For more information on these location types plese refer to the [Geocoding](Geocoding.md#response-structure) documentation.*

&nbsp;

&nbsp;

----------------------
#Response Structure

All responses with an HTTP status of 200 will consist of a JSON object with a top-level "data" node containing the following elements:

| Field    | Description |
|----------|-------------|
| strategy_results  | A JSON array of one or more objects containing the results of the requested strategies. The strategy_results will be ordered by their order in the validation_strategies comma separated string (ie for the string "FREETEXT,FIELDED" the result of FREETEXT will be the first result in the array) |

&nbsp;

Each element of the returned **strategy_results** array will be formatted as follows:

|   |   |
|---|---|
| strategy | The name of the strategy. |
| validation_data | A JSON object containing validation data. See below for properties of this object. **This object is always present regardless of the strategy result** |
| geography | A JSON object containing the result of the geocoding call that was validated. **This object is only present if that strategy's geocoder call succeeded ** |
| expiration_date | A date string in YYYY-MM-DD format representing the last day that this data may be used. Per our contract with Google, any data cached by users of the geocoding service must be discarded by this date. |

&nbsp;

Properties of the **validation_data** object will contain the following properties:

|   |   |
|---|---|
| status | The status of the valdation strategy can be (NOT_RUN, SUCCESS, FAILURE, NO_RESULTS) **This property is always present and non-empty.** |
| message | A short message describing a reason for the returned status. **This property is not always present.** |

&nbsp;

&nbsp;

Properties of the **geography** object will be formatted as follows:

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
| viewport | A bounding box describing a rectangle that encloses the location. The viewport field contains two coordinate objects -- **northeast** and **southwest** -- each with **lat** and **lng** decimal values. It also contains a **suggested_radius** decimal field that contains the distance in miles from the center of the viewport to a corner. **This property and its elements are always present and non-empty.** |
| partial_match | A Boolean value that, when true, indicates that the geocoder did not return an exact match for the original request, though it was able to match part of the requested address. You may wish to examine the original request for misspellings and/or an incomplete address.<br>Partial matches most often occur for street addresses that do not exist within the locality you pass in the request. Partial matches may also be returned when a request matches two or more locations in the same locality. For example, "21 Henr St, Bristol, UK" will return a partial match for both Henry Street and Henrietta Street. Note that if a request includes a misspelled address component, the geocoding service may suggest an alternative address. Suggestions triggered in this way will also be marked as a partial match. **This property is always present.** |

&nbsp;

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
