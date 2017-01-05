#GeoEntity API

&nbsp;

#Contents

- [Description](#description)
- [All Countries Request](#all-countries-request)
- [All Administrative Area Ones Request](#all-administrative-area-ones-request)
- [All Administrative Area Twos Request](#all-administrative-area-twos-request)
- [All Localities Request](#all-localities-request)
- [All Postal Codes Request](#all-postal-codes-request)
- [All Metropolitan Statistical Areas Request](#all-metropolitan-statistical-areas-request)
- [Versioning](#versioning)

&nbsp;
----------------------------
#Description

This service provides aggregated geography entities. Currently the following requests could be made.
- Request all countries in the world.
- Request all administrative area ones in a specific country.
- In a specific country ( and a specific first order of administrative area as optional), 
	- Request all administrative area twos.
	- Request all localities.
	- Request all postal codes.
	- Request all metropolitan statistical areas.
	
This service supports both HTTP/GET and POST methods.
All responses with an HTTP status of 200 will consist of a JSON object with a top-level "data" node containing a node "entities", which is an list of entities as requested.

&nbsp;
----------------------------
#All Countries Request
- Request Structure:
	- Target: http://localhost:8080/admin_area_1s (After adding to Routing layer, we update this)
	- URL parameter: none
	- Example URL:http://localhost:8080/countries
	
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
        "location_type": "APPROXIMATE",
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
     }
  ]
 }
}
```
&nbsp;
----------------------------
#All Administrative Area Ones Request
- Request Structure:
	- Target: http://localhost:8080/admin_area_1s (After adding to Routing layer, we update this)
	- URL parameter: 
  
    | Parameter  | Required | Description |
    |----------------|----------------|-----------------|
    | country        | Yes | A country name or two-letter ISO-3166 country code, not casing sensitive            | 
    
	- Example URL: http://localhost:8080/admin_area_1s?country=us
	
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
        "location_type": "APPROXIMATE",
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
      }
    ]
 }
}
```
&nbsp;
----------------------------
#All Administrative Area Twos Request
- Request Structure:
	- Target: http://localhost:8080/admin_area_2s (After adding to Routing layer, we update this)
	- URL parameter: 
  
    | Parameter  | Required | Description |
    |----------------|----------------|-----------------|
    | country        | Yes | A country name or two-letter ISO-3166 country code, not casing sensitive            | 
    | admin_area_one | Optional  | A first order administrative subdivision, both long name and short name are accepted|
    
	- Example URL: http://localhost:8080/admin_area_2s?country=us&admin_area_one=ga
	
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
        "location_type": "APPROXIMATE",
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
      }
  ]
 }
}
```
&nbsp;
----------------------------
#All Localities Request
- Request Structure:
	- Target: http://localhost:8080/localities (After adding to Routing layer, we update this)
	- URL parameter: 
  
    | Parameter  | Required | Description |
    |----------------|----------------|-----------------|
    | country        | Yes | A country name or two-letter ISO-3166 country code, not casing sensitive            | 
    | admin_area_one | Optional  | A first order administrative subdivision, both long name and short name are accepted|
    
	- Example URL: http://localhost:8080/localities?country=us&admin_area_one=ga
	
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
        "location_type": "APPROXIMATE",
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
      }
    ]
  }
}
```
&nbsp;
----------------------------
#All Postal Codes Request
- Request Structure:
	- Target: http://localhost:8080/postal_codes (After adding to Routing layer, we update this)
	- URL parameter: 
  
    | Parameter  | Required | Description |
    |----------------|----------------|-----------------|
    | country        | Yes | A country name or two-letter ISO-3166 country code, not casing sensitive            | 
    | admin_area_one | Optional  | A first order administrative subdivision, both long name and short name are accepted|
    
	- Example URL: http://localhost:8080/postal_codes?country=us&admin_area_one=ga
	
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
        "location_type": "APPROXIMATE",
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
      }
    ]
  }
}
```
&nbsp;
----------------------------
#All Metropolitan Statistical Areas Request
- Request Structure:
	- Target: http://localhost:8080/msas (After adding to Routing layer, we update this)
	- URL parameter: 
  
    | Parameter  | Required | Description |
    |----------------|----------------|-----------------|
    | country        | Yes | A country name or two-letter ISO-3166 country code, not casing sensitive            | 
    | admin_area_one | Optional  | A first order administrative subdivision, both long name and short name are accepted|
    
	- Example URL: http://localhost:8080/msas?country=us&admin_area_one=ga
	
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
      }
    ]
  }
}
```
&nbsp;
-----------
#Versioning
The current API version is 1.0. The data returned from the service is unversioned.  
Our general versioning strategy is available [here](/Versioning.md).
