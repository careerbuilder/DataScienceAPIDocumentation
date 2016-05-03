#Geography Normalization API

&nbsp;

#Contents

- [Description](#description)
- [Request Structure](#request-structure)
- [Response Structure](#response-structure)
- [Versioning](#versioning)

----------------------------
#Description

Accepts structured or unstructured location information and attempts to resolve the input to a geographic coordinate. If successful, the response will contain one (or more, if requested) locations with latitude and longitude information as well as any available administrative location information, such as country and state. This service relies on Google's Geocoding API as its data vendor; Google's service is documented [here](https://developers.google.com/maps/documentation/geocoding/intro). This service supports the HTTP/GET method only.

----------------------------
#Request Structure

Example URL: https://api.careerbuilder.com/core/geography/validate?address=5550%20Peachtree%20Pkwy&locality=Norcross&adminArea=GA&postalCode=30092&country=US

At least one of (query, address, locality, postalCode, adminArea, country) must be provided in the query string.


| Parameter  | Description |
|------------|-------------|
| address      | Optional. A string containing address information, e.g. "5550 Peachtree Pkwy."" Other forms of location data may be passed in this field (such as postal codes or country names), but we recommend using the type-specific parameters when the location type of the data is known. This parameter is handled in the same way as the **query** parameter and the two cannot be used simultaneously. |
| query      | Optional. A string containing location information in any format, e.g. "5550 Peachtree Pkwy, Norcross, GA 30092."" We recommend using the type-specific parameters when the location type of the data is known. This parameter is handled in the same way as the **address** parameter and the two cannot be used simultaneously. |
| locality   | Optional. The locality, such as the city or neighborhood, that corresponds to an address. |
| adminArea  | Optional. The subdivision name in the country or region for an address. This element is typically treated as the first order administrative subdivision, but in some cases it is the second, third, or fourth order subdivision in a country, dependency, or region. |
| country    | Optional. A country name or two-letter ISO-3166 country code. Used to limit the geographic scope of results to a particular country. Multiple pipe-delimited values may be supplied to limit results to a set of specified countries, e.g. ?query=Rome&country=IT&#124;US. |
| postalCode | Optional. The post code, postal code, or ZIP Code of an address. |
| culture    | Optional. The preferred ISO 639-1 language code for the response. If this parameter is omitted, the response will be returned in English. Note that the level of localization will vary for each culture. For example, the name "United States" may not have a localized name for every culture code. The geography normalization service currently only supports a subset of the ISO-639-1 standard. Click [here](Data/GeocodingServiceSupportedLanguages.md) to view a full list of supported culture codes. |
| maxResults |Optional. Used to specify the maximum number of desired results. If this parameter is not specified, the default value of 1 will be used. |
| territoriesAsStates | Optional. When enabled, dependent territories may be requested as administrative areas of their parent nation, and will also be returned as such. This parameter is necessary to validate, for instance, **locality=San Juan&adminArea=PR&country=US**. This functionality will only recognize territories and parent countries by their [two-digit ISO-3166-1 country codes](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2), so requests such as **adminArea=Puerto Rico&country=USA** would not be remapped according to this parameter. Default value is false. |
| tryLocalityAsAdminArea | Optional. When enabled, a request that includes the **locality** parameter and fails to retrieve locality-specific geography data will be reattempted with the locality value sent as an adminArea value instead. (If an adminArea value already exists, the locality value will be prepended and comma-separated, e.g. "locality=Harris%20Township&adminArea=OH" would be resent as "adminArea=Harris Township, OH" if tryLocalityAsAdminArea is set to true.) Default value is false. |

&nbsp;

----------------------
#Response Structure

The following response structure will be returned for all responses with an HTTP status of 200. Non-200 responses will return a JSON object containing a single "Error" string.

| Field    | Description |
|----------|-------------|
| errorMsg | *Deprecated*. This field always contains an empty string and will be removed in a future version of the API. |
| geoData  | A JSON array of one or more objects containing geography data. If the request returns no results, the array will be empty. |
| expirationDate | A date string in YYYY-MM-DD format representing the date by which this response must be discarded for clients who implement request-response caching. Per our contract with Google, this is the date when our service received the response from Google plus six months.

&nbsp;

Each element of the returned **geoData** array will be formatted as follows:

|   |   |
|---|---|
| AdminAreas | A JSON array of up to 2 AdminArea objects. Name: The name of the administrative district or subdivision. Level: An integer (1 or 2) indicating the hierarchical level of the administrative area. **This property is always present,** but may be empty if no admin areas were found. |
| Latitude | A double value specifying the location's latitude (in degrees) within the range [-90, +90]. **This property is always present.** |
| Longitude | A double value specifying the location's longitude (in degrees) within the range [-180, +180]. **This property is always present.** |
| Country | The country or region name of an address. **This property is always present,** but may be empty if the location was not associated with a country. Some locations for which this occurs are "Europe", "Pacific Ocean", and "Gaza Strip" (a contested territory). |
| City | The populated place for the address. This typically refers to a city, but may refer to a suburb or a neighborhood in certain countries. **This property is always present,** but may be empty. |
| PostalCode | The post code, postal code, or ZIP code of an address. **This property is always present,** but may be empty. |
| CountryCode | The two-letter ISO country code. **This property is always present,** but may be empty. See "Country" for examples of when this might occur. |
| FormattedAddress| A string containing the human-readable address of this location. Often this address is equivalent to the "postal address," which sometimes differs from country to country. (Note that some countries, such as the United Kingdom, do not allow distribution of true postal addresses due to licensing restrictions.) **This property is always present,** but may be empty. |
| LocationType | The classification of the geographic entity returned. Possible values (in descending order of specificity) are StreetAddress, Sublocality, City, PostalCode, AdminArea2, AdminArea1, Coordinate, Country, and Unknown. Coordinate is used when the location type returned by Google is either "natural_feature", "park", "airport", or "point_of_interest". Otherwise, the returned location type will reflect the most specific field present in the response. If none of these fields are present (such as for the examples for which the Country field is absent), then the returned location type will be Unknown. **This property is always present and non-empty.** |
| PostcodeLocalities | A string array denoting all the localities contained in a postal code. **This property is only present when the result is a postal code that contains multiple localities,** and will never be empty (nor will it ever contain only one element). |
| StreetAddress | The official street line of an address relative to the area, as specified by the Locality, or PostalCode, properties. **This property is always present,** but may be empty. |
| Sublocality | The neighborhood, borough, township, etc. for the address. Sublocalities are typically more specific than cities and may be returned even if the City field is empty. **This property is always present,** but may be empty. |
| Landmark | The full name of the landmark (such as a military base or island) returned by the service. **This property is always present,** but may be empty. |
| Viewport | A bounding box describing a rectangle that encloses the location. The viewport field contains two coordinate objects -- Northeast and Southwest -- each with lat and lng decimal values, and a SuggestedRadius decimal field that contains the distance in miles from the center of the viewport to a corner. **This property and its elements are always present and non-empty.** |
| PartialMatch | A boolean value that, when true, indicates that the geocoder did not return an exact match for the original request, though it was able to match part of the requested address. You may wish to examine the original request for misspellings and/or an incomplete address.<br>Partial matches most often occur for street addresses that do not exist within the locality you pass in the request. Partial matches may also be returned when a request matches two or more locations in the same locality. For example, "21 Henr St, Bristol, UK" will return a partial match for both Henry Street and Henrietta Street. Note that if a request includes a misspelled address component, the geocoding service may suggest an alternative address. Suggestions triggered in this way will also be marked as a partial match. **This property is always present.** |

&nbsp;

Each element of the returned **AdminAreas** array will be formatted as follows:

|   |   |
|---|---|
| ShortName | The abbreviated form of the administrative area's name. This will vary by country and administrative division level. In the US, states (AdminArea level 1) will use the state abbreviation (e.g. "GA"), and counties (AdminArea level 2) will use the full county name. **This property is always present and non-empty.** |
| LongName | The unabbreviated form of the administrative area's name. **This property is always present and non-empty.** |
| Level | An integer specifying the hierarchal level at which this administrative area resides. In the US, states will be returned with a level of 1 and counties with a level of 2. **This property is always present and non-empty.** |
| Name | *Deprecated*. The administrative area's name, with abbreviation determined at a per-country level. For new code, please use the ShortName or LongName field instead. **This property is always present and non-empty.** |

&nbsp;

-----------
#Versioning
The current API version is 1.0. The data returned from both services is unversioned.  

Geography is constantly changing as postal codes, cities, states, and countries are created and destroyed.  Google, our vendor, doesn't publish an update policy and changes things as they see fit.

Our general versioning strategy is available [here](/Versioning.md).
