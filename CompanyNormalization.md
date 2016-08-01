CompanyNormalizationService
=============

Table of Contents
_________
- [Request Information](#request-information)
- [Sample Response](#sample-response)
- [Versioning](#versioning)



#Request Information


HTTP method: GET
Parameters (query):
-        company_name (required) : The company name to be normalized.
-        service (required) : The normalizer to be used. Accepted values are "datadotcom" and "companydepot".
-        country (optional) : The country component of the provided company's location. Two-digit ISO country codes are supported. *note this is required for DataDotCom Service*
-        state (optional) : The state component of the provided company's location.
-        city (optional) : The city component of the provided company's location.
-        max_results (optional) : The maximum number of results to be returned. The default value is 3 and max_results must be between 1 and 10, inclusive.
 
Example: https://api.careerbuilder.com/core/normalizedcompanies?company_name=careerbuilder&service=companydepot&max_results=1

#Sample Response


```
{
  "data": {
    "normalized_companies": [
      {
        "confidence": 1,
        "id": "NC842f69a0-b621-48f6-b53c-9b3301e9aab7",
        "normalized_name": "Careerbuilder, LLC",
        "postal_code": "606011014",
        "city": "Chicago",
        "state": "IL",
        "naics_code": "561311",
        "country": "US",
        "address": "200 N La Salle St # 1100",
        "naics_description": "Employment Placement Agencies",
        "duns_number": "095301110",
        "company_size": "42868",
        "website": "www.careerbuilder.com"
      }
    ],
    "master_company": {
      "confidence": 1,
      "id": "NC842f69a0-b621-48f6-b53c-9b3301e9aab7",
      "normalized_name": "Careerbuilder, LLC",
      "postal_code": "606011014",
      "city": "Chicago",
      "state": "IL",
      "naics_code": "561311",
      "country": "US",
      "address": "200 N La Salle St # 1100",
      "naics_description": "Employment Placement Agencies",
      "duns_number": "095301110",
      "company_size": "42868",
      "website": "www.careerbuilder.com"
    },
    "data_version": "1.0.4"
  }
}
```


#Response Information

The response returns a single data node which contains a list of normalized companies. These normalized companies are ordered by the confidence score. Each normalized company has a normalized company name (string), a [NAICS](http://www.census.gov/eos/www/naics/) ID (string), a NAICS description (string), a DUNS number(string), a country (string), a state (string), a city (string), a postal code (string), a website (string), a company size (int), an ID (string), and a confidence (double). Confidence scores range from 0.0 to 1.0. Additionally, when the service requested is CompanyDepot, a single master company will be returned. 

The master company is the highest division of the requested company. For example a request with the company name "coca-cola bottling" returns the master company of "The Coca-Cola Company" instead of "Coca Cola Bottling Co". While the "Coca Cola Bottling Co" will be returned in the normalized companies list it is not the master company.



Additionally within the data Company Normaliztion will also return the version of the data used to perform the normalization. 
Current Data Version for Company Depot: "1.0.4"
Current Data Version for DataDotCom: "35.0"


#Versioning
-----------
The response from the Company Normalization Service is versioned with the current version being 1.0. The data set used to perform the normalization is also versioned but we will always use the latest.

Our general versioning strategy is available [here](/Versioning.md).
