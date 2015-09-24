SchoolNormalizationService
=============

Table of Contents
_________
- [Request Information](#request-information)
- [Sample Response](#sample-response)
- [Versioning](#versioning)
- [Supported Countries](#supported-countries)



#Request Information


HTTP method: GET or POST
Parameters (query/form):
-        query (required) : The educational institution information to be queried.
-        country (optional) : 2 digit ISO country code of educational institution. Accepted country codes are all supported CB countries. The full list can be found [here](#supported-countries)
 
Example: https://api.careerbuilder.com/core/normalizedschools?query=georgia%20tech&country=us

#Sample Response


```
{
    "data": {
        "normalized_schools": [
            {
                "normalized_school_name": "Georgia Institute of Technology",
                "id": "53bff579e4b04710d09fa98d",
                "country": "US",
                "confidence": 2.0
            }
        ]
    }
}
```


#Response Information

The response returns a single data node which contains a list of normalized schools. These normalized schools are ordered by the confidence score. Each normalized shool has a normalized school name (string), a unique ID (string), a country (string), and a confidence (double). Confidence scores range from 0.0 to 2.0.


#Versioning
-----------
The response from the School Normalization Service is versioned with the current version being 1.0. The data set used to perform the normalization is also versioned (currently 1.0.0). This data set is upgraded at will in order to improve accuracy of the classifier. Requests made to this service will always use the latest version of the data set.

Our general versioning strategy is available [here](/Versioning.md).

#Supported Countries
--------------------
Supported ISO Country Codes (with full country name)

| Country Code | Full Country Name                            |
|--------------|----------------------------------------------|
| AD           | Andorra                                      |
| AE           | United Arab Emirates                         |
| AF           | Afghanistan                                  |
| AG           | Antigua and Barbuda                          |
| AI           | Anguilla                                     |
| AL           | Albania                                      |
| AM           | Armenia                                      |
| AO           | Angola                                       |
| AQ           | Antarctica                                   |
| AR           | Argentina                                    |
| AS           | American Samoa                               |
| AT           | Austria                                      |
| AU           | Australia                                    |
| AW           | Aruba                                        |
| AX           | Aland Islands                                |
| AZ           | Azerbaijan                                   |
| BA           | Bosnia and Herzegovina                       |
| BB           | Barbados                                     |
| BD           | Bangladesh                                   |
| BE           | Belgium                                      |
| BF           | Burkina Faso                                 |
| BG           | Bulgaria                                     |
| BH           | Bahrain                                      |
| BI           | Burundi                                      |
| BJ           | Benin                                        |
| BL           | St. Barthélemy                               |
| BM           | Bermuda                                      |
| BN           | Brunei Darussalam                            |
| BO           | Bolivia                                      |
| BQ           | Bonaire, Sint Eustatius and Saba             |
| BR           | Brazil                                       |
| BS           | Bahamas                                      |
| BT           | Bhutan                                       |
| BV           | Bouvet Island                                |
| BW           | Botswana                                     |
| BY           | Belarus                                      |
| BZ           | Belize                                       |
| CA           | Canada                                       |
| CC           | Cocos (Keeling) Islands                      |
| CD           | Congo, Democratic Republic of                |
| CF           | Central African Republic                     |
| CG           | Congo                                        |
| CH           | Switzerland                                  |
| CI           | Ivory Coast                                  |
| CK           | Cook Islands                                 |
| CL           | Chile                                        |
| CM           | Cameroon                                     |
| CN           | China                                        |
| CO           | Colombia                                     |
| CR           | Costa Rica                                   |
| CU           | Cuba                                         |
| CV           | Cape Verde                                   |
| CW           | Curaçao                                      |
| CX           | Christmas Island                             |
| CY           | Cyprus                                       |
| CZ           | Czech Republic                               |
| DE           | Germany                                      |
| DJ           | Djibouti                                     |
| DK           | Denmark                                      |
| DM           | Dominica                                     |
| DO           | Dominican Republic                           |
| DZ           | Algeria                                      |
| EC           | Ecuador                                      |
| EE           | Estonia                                      |
| EG           | Egypt                                        |
| EH           | Western Sahara                               |
| ER           | Eritrea                                      |
| ES           | Spain                                        |
| ET           | Ethiopia                                     |
| FI           | Finland                                      |
| FJ           | Fiji                                         |
| FK           | Falkland Islands (Malvinas)                  |
| FM           | Micronesia (the Federated States of)         |
| FO           | Faroe Islands                                |
| FR           | France                                       |
| GA           | Gabon                                        |
| GB           | Great Britain                                |
| GD           | Grenada                                      |
| GE           | Georgia                                      |
| GF           | French Guiana                                |
| GG           | Guernsey                                     |
| GH           | Ghana                                        |
| GI           | Gibraltar                                    |
| GL           | Greenland                                    |
| GM           | Gambia                                       |
| GN           | Guinea                                       |
| GP           | Guadeloupe                                   |
| GQ           | Equatorial Guinea                            |
| GR           | Greece                                       |
| GS           | South Georgia and the South Sandwich Islands |
| GT           | Guatemala                                    |
| GU           | Guam                                         |
| GW           | Guinea-Bissau                                |
| GY           | Guyana                                       |
| HK           | Hong Kong                                    |
| HM           | Heard Island & McDonald Islands              |
| HN           | Honduras                                     |
| HR           | Croatia                                      |
| HT           | Haiti                                        |
| HU           | Hungary                                      |
| ID           | Indonesia                                    |
| IE           | Ireland                                      |
| IL           | Israel                                       |
| IM           | Isle of Man                                  |
| IN           | India                                        |
| IO           | British Indian Ocean Territory               |
| IQ           | Iraq                                         |
| IR           | Iran                                         |
| IS           | Iceland                                      |
| IT           | Italy                                        |
| JE           | Jersey                                       |
| JM           | Jamaica                                      |
| JO           | Jordan                                       |
| JP           | Japan                                        |
| KE           | Kenya                                        |
| KG           | Kyrgyzstan                                   |
| KH           | Cambodia                                     |
| KI           | Kiribati                                     |
| KM           | Comoros                                      |
| KN           | St. Kitts & Nevis                            |
| KP           | Korea (North)                                |
| KR           | Korea (South)                                |
| KW           | Kuwait                                       |
| KY           | Cayman Islands                               |
| KZ           | Kazakhstan                                   |
| LA           | Laos                                         |
| LB           | Lebanon                                      |
| LC           | St. Lucia                                    |
| LI           | Liechtenstein                                |
| LK           | Sri Lanka                                    |
| LR           | Liberia                                      |
| LS           | Lesotho                                      |
| LT           | Lithuania                                    |
| LU           | Luxembourg                                   |
| LV           | Latvia                                       |
| LY           | Libya                                        |
| MA           | Morocco                                      |
| MC           | Monaco                                       |
| MD           | Moldova                                      |
| ME           | Montenegro                                   |
| MF           | St. Martin                                   |
| MG           | Madagascar                                   |
| MH           | Marshall Islands (the)                       |
| MK           | Macedonia, the former Yugoslav Republic of   |
| ML           | Mali                                         |
| MM           | Myanmar                                      |
| MN           | Mongolia                                     |
| MO           | Macau                                        |
| MP           | Northern Mariana Islands                     |
| MQ           | Martinique                                   |
| MR           | Mauritania                                   |
| MS           | Montserrat                                   |
| MT           | Malta                                        |
| MU           | Mauritius                                    |
| MV           | Maldives                                     |
| MW           | Malawi                                       |
| MX           | Mexico                                       |
| MY           | Malaysia                                     |
| MZ           | Mozambique                                   |
| NA           | Namibia                                      |
| NC           | New Caledonia                                |
| NE           | Niger                                        |
| NF           | Norfolk Island                               |
| NG           | Nigeria                                      |
| NI           | Nicaragua                                    |
| NL           | Netherlands                                  |
| NO           | Norway                                       |
| NP           | Nepal                                        |
| NR           | Nauru                                        |
| NU           | Niue                                         |
| NZ           | New Zealand                                  |
| OM           | Oman                                         |
| PA           | Panama                                       |
| PE           | Peru                                         |
| PF           | French Polynesia                             |
| PG           | Papua New Guinea                             |
| PH           | Philippines                                  |
| PK           | Pakistan                                     |
| PL           | Poland                                       |
| PM           | St. Pierre & Miquelon                        |
| PN           | Pitcairn                                     |
| PR           | Puerto Rico                                  |
| PS           | Palestinian Territory                        |
| PT           | Portugal                                     |
| PW           | Palau                                        |
| PY           | Paraguay                                     |
| QA           | Qatar                                        |
| RE           | Reunion                                      |
| RO           | Romania                                      |
| RS           | Serbia                                       |
| RU           | Russian Federation                           |
| RW           | Rwanda                                       |
| SA           | Saudi Arabia                                 |
| SB           | Solomon Islands                              |
| SC           | Seychelles                                   |
| SD           | Sudan                                        |
| SE           | Sweden                                       |
| SG           | Singapore                                    |
| SH           | Saint Helena, Ascension and Tristan da Cunha |
| SI           | Slovenia                                     | 

