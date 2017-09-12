Company Normalization Service
=============

The Company Normalization Service accepts HTTP GET or POST requests that specify information about a company, and returns normalized company data including but not limited to: name, location, website, NAICS data, and Dun & Bradstreet (DUNS) number. It is backed by the CompanyDepot classifier developed by the Information Extraction and Retrieval team.
_________
- [Request Information](#request-information)
- [Sample Response](#sample-response)
- [Versioning](#versioning)


## Request Information

The following parameters may be used in constructing a request to the service:

 Field                      | Required | Description  
 -------------------------- |----------| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 ```company_name```         | true     | The company name to be normalized. 
 ```website```              | true     | The website of the company to be normalized. *(Note: either* ```company_name``` *or* ```website``` *must be provided. It is not necessary to provide both.)* 
 ```country```              | false    | The country component of the provided company's location. Two-digit ISO country codes are supported. *(Note: Required for requests to the DataDotCom service.)* 
 ```state```                | false    | The state component of the provided company's location.
 ```city```                 | false    | The city component of the provided company's location.
 ```max_results```          | false    | The maximum number of results to be returned, between 1 and 10 (inclusive). Defaults to 3.
 ```use_query_classifier``` | false    | Specifies whether to detect specific queries (irrelevant queries or agency queries) and return empty results for such queries. This value is defaulted to true, in order to turn it off users must provide the param with a value of false.
 ```filter_by_country```    | false    | With this set to false, the country parameter only biases results and this is traditionally how the service has worked. Setting this value to true will filter results based on country. This param is defaulted to false.

\* *Note that company_name and website are each constrained to a maximum length of 400 characters. Requests that contain a company_name or website value exceeding this limit will fail with an HTTP 400 Bad Request status.*
 
Example: https://api.careerbuilder.com/core/normalizedcompanies?company_name=careerbuilder&max_results=1

## Sample Response

```json
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

## Response Information

The response returns a single data node which contains a list of normalized companies. These normalized companies are ordered by the confidence score. Each normalized company has a normalized company name (string), a [NAICS](http://www.census.gov/eos/www/naics/) ID (string), a NAICS description (string), a DUNS number(string), a country (string), a state (string), a city (string), a postal code (string), a website (string), a company size (int), an ID (string), and a confidence (double). Confidence scores range from 0.0 to 1.0. A single master company will be returned.

The master company is the highest division of the requested company. For example a request with the company name "amazon web services" returns "Amazon Web Services LLC" in its normalized_companies list, with a master company of "Amazon.com, Inc."

The top-level data node will also include a data_version field that specifies the current classifier (CompanyDepot or DataDotCom) version as a string.

## Versioning
The response from the Company Normalization Service is versioned with the current version being 1.0. The CompanyDepot data is also versioned, and we will always use the latest.

Our general versioning strategy is available [here](/Versioning.md).
