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
-        service (required) : The normalizer to be used. Accepted values are DataDotCom, and CompanyDepot
-        country (optional) : 2 digit ISO country code of the company name provided. *note this is required for DataDotCom Service*
-        state (optional) : State of the company name provided.
-        city (optional) : City of the company name provided.
-        max_results (optional) : The maximum results returned by the API. The default value is 3 and max_results must be between 0 and 11.
 
Example: https://api.careerbuilder.com/core/normalizedcompanies?company_name=careerbuilder&service=companydepot&max_results=1

#Sample Response


```
{
    "data": {
        "normalized_companies": [
            {
                "confidence": 1,
                "normalized_name": "Careerbuilder, LLC",
                "postal_code": "",
                "city": "Chicago",
                "state": "IL",
                "naics_code": "561311",
                "country": "US",
                "address": "200 N La Salle St # 1100",
                "naics_description": ""
            }
        ],
        "data_version": "0.0.2"
    }
}
```


#Response Information

The response returns a single data node which contains a list of normalized companies. These normalized companies are ordered by the confidence score. Each normalized company has a normalized company name (string), a naics ID (string), a naics description (string), a country (string), a state(string), a city(string), a postal code(string), and a confidence (double). Confidence scores range from 0.0 to 1.0.

Additionally within the data Company Normaliztion will also return the version of the data used to perform the normalization. 
Current Data Version for Company Depot: "0.0.2"
Current Data Version for DataDotCom: "35.0"


#Versioning
-----------
The response from the Company Normalization Service is versioned with the current version being 1.0. The data set used to perform the normalization is also versioned but we will always use the latest.

Our general versioning strategy is available [here](/Versioning.md).
