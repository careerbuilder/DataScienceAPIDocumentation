#GeoEntity API

&nbsp;

#Contents

- [Description](#description)
- [All Countries Request](#all-countries-request)
- [All First-level Administrative Areas Request](#all-first-level-administrative-areas-request)
- [All Second-level Administrative Areas Request](#all-second-level-administrative-areas-request)
- [All Localities Request](#all-localities-request)
- [All Postal Codes Request](#all-postal-codes-request)
- [All Metropolitan Statistical Areas Request](#all-metropolitan-statistical-areas-request)
- [Versioning](#versioning)

&nbsp;
----------------------------
#Description

This service provides lists of geographic entities of a particular type, such as postal codes or first-level administrative areas. This service uses the same underlying data as our geocoding service. The list of entities returned as queried should be comprehensive for the populous countries, but we could not guarantee 100% completeness. Currently, the following requests are supported:
- Request all countries in the world.
- Request all administrative area ones in a specific country.
- In a specific country (optionally limited to a single first-level administrative area):
	- Request all administrative area twos.
	- Request all localities.
	- Request all postal codes.
	- Request all metropolitan statistical areas.
	
If you require a list of geographic entities of a particular entity type, but the entity list is not complete, please contact <DataScienceApplicationDevelopment@careerbuilder.com>.

This service supports HTTP/GET method only.
All responses with an HTTP status of 200 will consist of a JSON object with a top-level "data" node containing a node "entities", which is a list of entities as requested.

&nbsp;
----------------------------
#All Countries Request
- Request Structure:
	- Target: https://api.careerbuilder.com/core/geography/entities/countries 
	- URL parameter: none
	- Example URL:https://api.careerbuilder.com/core/geography/entities/countries
	
- Response Sample:
```
{
  "data": {
    "entities": [
      {
        "name": "Afghanistan",
        "country_code": "AF",
        "place_id": "ChIJbQL_-LZu0TgReNqWvg1GtfM",
        "location": {
          "lat": 33.93911,
          "lng": 67.709953
        },
        "viewport": {
          "northeast": {
            "lat": 38.4908389,
            "lng": 74.8898619
          },
          "southwest": {
            "lat": 29.3772319,
            "lng": 60.5170005
          }
        },
        "formatted_address": "Afghanistan"
     },
     ...
  ]
 }
}
```
&nbsp;
----------------------------
#All First-level Administrative Areas Request
- Request Structure:
	- Target: https://api.careerbuilder.com/core/geography/entities/admin_area_1s
	- URL parameter: 
  
    | Parameter  | Required | Description |
    |----------------|----------------|-----------------|
    | country        | Yes | A country name or two-letter ISO-3166 country code, not casing sensitive            | 
    
	- Example URL: https://api.careerbuilder.com/core/geography/entities/admin_area_1s?country=us
	
- Response Sample:
```
{
  "data": {
    "entities": [
      {
        "long_name": "Alaska",
        "short_name": "AK",
        "level": 1,
        "place_id": "ChIJG8CuwJzfAFQRNduKqSde27w",
        "location": {
          "lat": 64.2008413,
          "lng": -149.4936733
        },        
        "viewport": {
          "northeast": {
            "lat": 71.347066,
            "lng": -129.9945563
          },
          "southwest": {
            "lat": 51.220011,
            "lng": 172.4573429
          }
        },
        "formatted_address": "Alaska, USA"
      },
      ...
    ]
 }
}
```
&nbsp;
----------------------------
#All Second-level Administrative Areas Request
- Request Structure:
	- Target: https://api.careerbuilder.com/core/geography/entities/admin_area_2s
	- URL parameter: 
  
    | Parameter  | Required | Description |
    |----------------|----------------|-----------------|
    | country        | Yes | A country name or two-letter ISO-3166 country code, not casing sensitive            | 
    | admin_area_one | Optional  | A first order administrative subdivision, both long name and short name are accepted. If present, limits the result set to the second-level administrative areas within the specified first-level administrative area. Otherwise, all second-level administrative areas within the specified country will be returned.|
    
	- Example URL: https://api.careerbuilder.com/core/geography/entities/admin_area_2s?country=us&admin_area_one=ga
	
- Response Sample:
```
{
  "data": {
    "entities": [
      {
        "long_name": "Camden County",
        "short_name": "Camden County",
        "level": 2,
        "place_id": "ChIJl6NPkwkb5YgRwGvHFXP6tYY",
        "location": {
          "lat": 30.898276,
          "lng": -81.6035062
        },
        "viewport": {
          "northeast": {
            "lat": 31.169595,
            "lng": -81.40354649999999
          },
          "southwest": {
            "lat": 30.7114626,
            "lng": -81.936988
          }
        },
        "formatted_address": "Camden County, GA, USA"
      },
      ...
  ]
 }
}
```
&nbsp;
----------------------------
#All Localities Request
- Request Structure:
	- Target: https://api.careerbuilder.com/core/geography/entities/localities
	- URL parameter: 
  
    | Parameter  | Required | Description |
    |----------------|----------------|-----------------|
    | country        | Yes | A country name or two-letter ISO-3166 country code, not casing sensitive            | 
    | admin_area_one | Optional  | A first order administrative subdivision, both long name and short name are accepted. If present, limits the result set to the localities within the specified first-level administrative area. Otherwise, all localities within the specified country will be returned.|
    
	- Example URL: https://api.careerbuilder.com/core/geography/entities/localities?country=us&admin_area_one=ga
	
- Response Sample:
```
{
  "data": {
    "entities": [
      {
        "locality": "Candler-McAfee",
        "place_id": "ChIJ9T6vBZ4A9YgRMKirj_zfz3I",
        "location": {
          "lat": 33.7254622,
          "lng": -84.2752831
        },
        "viewport": {
          "northeast": {
            "lat": 33.7399031,
            "lng": -84.230871
          },
          "southwest": {
            "lat": 33.7122249,
            "lng": -84.30969189999999
          }
        },
        "formatted_address": "Candler-McAfee, GA 30032, USA"
      },
      ...
    ]
  }
}
```
&nbsp;
----------------------------
#All Postal Codes Request
- Request Structure:
	- Target: https://api.careerbuilder.com/core/geography/entities/postal_codes
	- URL parameter: 
  
    | Parameter  | Required | Description |
    |----------------|----------------|-----------------|
    | country        | Yes | A country name or two-letter ISO-3166 country code, not casing sensitive            | 
    | admin_area_one | Optional  | A first order administrative subdivision, both long name and short name are accepted. If present, limits the result set to the postal codes ithin the specified first-level administrative area. Otherwise, all postal codes within the specified country will be returned.|
    
	- Example URL: https://api.careerbuilder.com/core/geography/entities/postal_codes?country=us&admin_area_one=ga
	
- Response Sample:
```
{
  "data": {
    "entities": [
      {
        "postal_code": "30004",
        "place_id": "ChIJEVI59EJ29YgR42rqudX4ipg",
        "location": {
          "lat": 34.17630630000001,
          "lng": -84.2910759
        },
        "viewport": {
          "northeast": {
            "lat": 34.228105,
            "lng": -84.2134471
          },
          "southwest": {
            "lat": 34.06322189999999,
            "lng": -84.3814419
          }
        },
        "formatted_address": "Alpharetta, GA 30004, USA"
      },
      ...
    ]
  }
}
```
&nbsp;
----------------------------
#All Metropolitan Statistical Areas Request
- Request Structure:
	- Target: https://api.careerbuilder.com/core/geography/entities/msas
	- URL parameter: 
  
    | Parameter  | Required | Description |
    |----------------|----------------|-----------------|
    | country        | Yes | A country name or two-letter ISO-3166 country code, not casing sensitive            | 
    | admin_area_one | Optional  | A first order administrative subdivision, both long name and short name are accepted. If present, limits the result set to the metropolitan statistical areas within the specified first-level administrative area. Otherwise, all metropolitan statistical areas within the specified country will be returned.|
    
	- Example URL: https://api.careerbuilder.com/core/geography/entities/msas?country=us&admin_area_one=ga
	
- Response Sample:
```
{
  "data": {
    "entities": [
      {
        "title": "Albany, GA",
        "code": 10500
      },
      {
        "title": "Athens-Clarke County, GA",
        "code": 12020
      },
      ...
    ]
  }
}
```
&nbsp;
-----------
#Versioning
The current API version is 1.0. The data returned from the service is unversioned.  
Our general versioning strategy is available [here](/Versioning.md).
