#Geography Normalization API

&nbsp;

#Contents

- [Description](#description)
- [Request Structure](#request-structure)
- [Response Structure](#response-structure)
- [Nearby Locations API](#nearby-locations-api)
- [Nearby Locations Response](#nearby-locations-api-response)
- [Versioning](#versioning)

----------------------------
#Description

Accepts structured or unstructured location information and attempts to resolve the input to a geographic coordinate. If successful, the response will contain one (or more, if requested) locations with latitude and longitude information as well as any available administrative location information, such as country and state. Supports the HTTP/GET method only.

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

| Field    | Description |
|----------|-------------|
| errorMsg | Contains information about any error(s) incurred during the request. If the request successfully returns results, this field will be empty. |
| geoData  | A JSON array of one or more objects containing geography data. If the request returns no results, the array will be empty. |

&nbsp;

Each element of the returned **geoData** array will be formatted as follows:

|   |   |
|---|---|
| AdminAreas | A JSON array of up to 2 AdminArea objects. Name: The name of the administrative district or subdivision. Level: An integer (1 or 2) indicating the hierarchical level of the administrative area. |
| Latitude | A double value specifying the location's latitude (in degrees) within the range [-90, +90]. |
| Longitude | A double value specifying the location's longitude (in degrees) within the range [-180, +180]. |
| Country | The country or region name of an address. |
| City | The populated place for the address. This typically refers to a city, but may refer to a suburb or a neighborhood in certain countries. |
| PostalCode | The post code, postal code, or ZIP code of an address. |
| CountryCode | The two-letter ISO country code. |
| LocationType | The classification of the geographic entity returned. Possible values are City, PostalCode, AdminArea1, AdminArea2, Country, and Unknown. |
| StreetAddress | The official street line of an address relative to the area, as specified by the Locality, or PostalCode, properties. |
| Landmark | The full name of the landmark (such as a military base or island) returned by the service. If the returned entity is not a landmark, this field will be empty. |

&nbsp;

Each AdminArea object will be formatted as follows:

|   |   |
|---|---|
| ShortName | The abbreviated form of the administrative area's name. This will vary by country and administrative division level. In the US, states (AdminArea level 1) will use the state abbreviation (e.g. "GA"), and counties (AdminArea level 2) will use the full county name. |
| LongName | The unabbreviated form of the administrative area's name. |
| Level | An integer specifying the hierarchal level at which this administrative area resides. In the US, states will be returned with a level of 1 and counties with a level of 2. |
| Name | Deprecated. The administrative area's name, with abbreviation determined at a per-country level. For new code, please use the ShortName or LongName field instead. |

&nbsp;

--------------------
#Nearby Locations API

***THIS API HAS BEEN DECOMMISSIONED AS OF SEPTEMBER 22, 2015. Please contact DataScienceApplicationDevelopment@careerbuilder.com if you are interested in consuming this API.***

~~Accepts latitude and longitude values representing a geographic point and attempts to return a list of populated places near the point, sorted in descending order of population.~~

~~Example URL: https://api.careerbuilder.com/core/geography/nearbylocations?lat=33.75&lon=-84.39~~

| Parameter | Description |
|-----------|-------------|
| latitude | Required. The latitude part of the coordinate query. May be shortened to lat. |
| longitude | Required. The longitude part of the coordinate query. May be shortened to lon. |
| language | Optional. The preferred ISO 639-1 language code for the response. May be shortened to lang. If this parameter is not specified, the default value of en (English) will be used. |
| radius | Optional. An integer value (in km) indicating the maximum distance of a returned location from the provided point (up to 300 km). May be shortened to rad. If this parameter is not specified, the default value of 30 km will be used. |
| maxResults | Optional. Used to specify the maximum number of desired results (up to 100). If this parameter is not specified, the default value of 10 will be used. |
| sortBy | Optional. Used to specify the value by which to sort results. Accepted values are "population" (default) and "distance". Note that in cases where not all results are returned, the set of results returned for one sorting parameter will often contain different results than those returned for the other sorting parameter. |

&nbsp;

-----------------------------
#Nearby Locations API Response
|   |   |
|---|---|
| errorMsg | Contains information about any error(s) incurred during the request. If the request successfully returns results, this field will be empty. |
| geoData | A JSON array of one or more objects containing geography data. If the request returns no results, the array will be empty. |  

For successful requests, each element of the returned array will be formatted as follows:

|   |   |
|---|---|
| countryCode | The two-letter ISO-3166 country code representing the country of the named populated place. |
| countryName | The country of the named populated place. |
| latitude | The latitude part of the named populated place's geographic location. |
| longitude | The longitude part of the named populated place's geographic location. |
| distance | The distance, in km, of the named populated place from the geographic coordinate supplied in the request. |
| population |The population of the named populated place. |
| name | The name of the populated place. |
| adminArea | The top-level administrative division of the named populated place (typically a state or province). |

&nbsp;

-----------
#Versioning
The current API version is 1.0. The data returned from both services is unversioned.  

Geography is constantly changing as postal codes, cities, states, and countries are created and destroyed.  Google, our vendor, doesn't publish an update policy and changes things as they see fit.

Our general versioning strategy is available [here](/Versioning.md).
