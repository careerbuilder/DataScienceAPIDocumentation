#VirusScanner API

&nbsp;

#Contents

- [Description](#description)
- [Request](#request)
- [Request Structure](#request-structure)
- [Sample Request](#sample-request)
- [Response Structure](#response-structure)
- [Versioning](#versioning)

----------------------------
#Description

This service runs advanced antivirus scans on any file type. We poll Avira's update server every five minutes to ensure that the service is as up to date as possible.

The libraries that these virus scans are based on are regularly updated by Avira - sometimes as many as 20 or 30 times a day. We recommend regular reflows of data to ensure that your data security is as up to date as possible. All files that return **is_safe = false** should be immediately quarantined and the source of the data should be investigated.

----------------------------
#Request

A Virus Scanner request is simply the base64 string representation of a file to be scanned. The VS API accepts GET and POST requests, however we recommend using POSTs to avoid the request size limitations inherent to GET requests.

#Request Structure

| Parameter  | Description |
|------------|-------------|
| document     | Required. The base64 string representation of a file to be scanned. |

&nbsp;

#Sample Request

A sample request on the API tester page can be found [here](https://apimanagement.cbplatform.link/#routes/tester?preURL=https%3A%2F%2Fwwwtest.api.careerbuilder.com%2F&postURL=core%2Fvirusscanner&method=post&contentType=application%2Fx-www-form-urlencoded&acceptType=application%2Fjson&version=default&region=staging&flow=client_credentials&userDid=&accountDid=&headers=&body=document%3DSGVsbG8gV29ybGQ%3D)

&nbsp;

----------------------
#Response Structure

All responses with an HTTP status of 200 will consist of a JSON object with a top-level "data" node containing the following elements:

| Field    | Description |
|----------|-------------|
| is_safe  | A **boolean** value to quickly tell you whether or not the document is believed to contain a virus. |
| virus_definition | A **string** value that will always be returned. If no virus is found, this will say "200 OK" - if a virus is found, this field will contain more specific information about the nature of the virus. |


&nbsp;

-----------
#Versioning
The current API version is 1.0. The data returned from the service is unversioned.  




Our general versioning strategy is available [here](/Versioning.md).
