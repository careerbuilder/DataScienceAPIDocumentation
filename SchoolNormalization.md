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
-        country (optional) : 2 digit ISO country code of educational institution. Accepted country codes are [here](#supported-countries)
 
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
                "confidence": 2
            }
        ]
    }
}
```

#Versioning
-----------
The response from the School Normalization Service is versioned with the current version being 1.0. The data set used to perform the normalization is also versioned (currently 1.0.0). This data set is upgraded at will in order to improve accuracy of the classifier. Requests made to this service will always use the latest version of the data set.

Our general versioning strategy is available [here](/Versioning.md).

#Supported Countries
--------------------
Supported ISO Country Codes (with full country name)

| Country Code | Full Country Name | 
|----|----------------------------------| 
| AF | Afghanistan                      | 
| AL | Albania                          | 
| DZ | Algeria                          | 
| AS | American Samoa                   | 
| AG | Antigua and Barbuda              | 
| AR | Argentina                        | 
| AM | Armenia                          | 
| AW | Aruba                            | 
| AU | Australia                        | 
| AT | Austria                          | 
| AZ | Azerbaijan                       | 
| BS | Bahamas                          | 
| BH | Bahrain                          | 
| BD | Bangladesh                       | 
| BB | Barbados                         | 
| BY | Belarus                          | 
| BE | Belgium                          | 
| BZ | Belize                           | 
| BJ | Benin                            | 
| BM | Bermuda                          | 
| BT | Bhutan                           | 
| BO | Bolivia                          | 
| BA | Bosnia and Herzegovina           | 
| BW | Botswana                         | 
| BR | Brazil                           | 
| VG | British Virgin Islands           | 
| BN | Brunei                           | 
| BG | Bulgaria                         | 
| KH | Cambodia                         | 
| CM | Cameroon                         | 
| CA | Canada                           | 
| CV | Cape Verde                       | 
| KY | Cayman islands                   | 
| CF | Central African Republic         | 
| TD | Chad                             | 
| CL | Chile                            | 
| CN | China                            | 
| CO | Colombia                         | 
| CR | Costa rica                       | 
| HR | Croatia                          | 
| CU | Cuba                             | 
| CW | Curaçao                          | 
| CY | Cyprus                           | 
| CZ | Czech Republic                   | 
| CD | Democratic Republic of the Congo | 
| DK | Denmark                          | 
| DJ | Djibouti                         | 
| DO | Dominican republic               | 
| EC | Ecuador                          | 
| EG | Egypt                            | 
| SV | El Salvador                      | 
| GB | England                          | 
| GQ | Equatorial Guinea                | 
| ER | Eritrea                          | 
| EE | Estonia                          | 
| ET | Ethiopia                         | 
| FJ | Fiji                             | 
| FI | Finland                          | 
| FR | France                           | 
| PF | French Polynesia                 | 
| GM | Gambia                           | 
| GE | Georgia                          | 
| DE | Germany                          | 
| GH | Ghana                            | 
| GR | Greece                           | 
| GD | Grenada                          | 
| GU | Guam                             | 
| GT | Guatemala                        | 
| GW | Guinea                           | 
| GY | Guyana                           | 
| HT | Haiti                            | 
| HN | Honduras                         | 
| HK | Hong kong                        | 
| HU | Hungary                          | 
| IS | Iceland                          | 
| IN | India                            | 
| ID | Indonesia                        | 
| IR | Iran                             | 
| IQ | Iraq                             | 
| IE | Ireland                          | 
| IM | Isle of Man                      | 
| IL | Israel                           | 
| IT | Italy                            | 
| JM | Jamaica                          | 
| JP | Japan                            | 
| KE | Jenya                            | 
| JE | Jersey                           | 
| JO | Jordan                           | 
| KG | Jyrgyzstan                       | 
| KZ | Kazakhstan                       | 
| KR | Korea                            | 
| XK | Kosovo                           | 
| KW | Kuwait                           | 
| LA | Lao Pdr                          | 
| LV | Latvia                           | 
| LB | Lebanon                          | 
| LS | Lesotho                          | 
| LR | Liberia                          | 
| LY | Libya                            | 
| LI | Liechtenstein                    | 
| LT | Lithuania                        | 
| LU | Luxembourg                       | 
| MO | Macau                            | 
| MW | Malawi                           | 
| MY | Malaysia                         | 
| MV | Maldives                         | 
| MH | Marshall islands                 | 
| MR | Mauritania                       | 
| MX | Mexico                           | 
| MD | Moldova                          | 
| MN | Mongolia                         | 
| ME | Montenegro                       | 
| MA | Morocco                          | 
| MM | Myanmar                          | 
| NA | Namibia                          | 
| NP | Nepal                            | 
| NL | Netherlands                      | 
| NZ | New Zealand                      | 
| NI | Nicaragua                        | 
| NG | Nigeria                          | 
| MP | Northern mariana islands         | 
| NO | Norway                           | 
| OM | Oman                             | 
| PK | Pakistan                         | 
| PS | Palestine                        | 
| PA | Panama                           | 
| PG | Papua New Guinea                 | 
| PY | Paraguay                         | 
| PE | Peru                             | 
| PH | Philippines                      | 
| PL | Poland                           | 
| PT | Portugal                         | 
| PR | Puerto Rico                      | 
| QA | Qatar                            | 
| MK | Republic of macedonia            | 
| RO | Romania                          | 
| RU | Russia                           | 
| RW | Rwanda                           | 
| LC | Saint Lucia                      | 
| VC | Saint Vincent and the Grenadines | 
| KN | Saint kitts                      | 
| WS | Samoa                            | 
| SA | Saudi Arabia                     | 
| SN | Senegal                          | 
| RS | Serbia                           | 
| SC | Seychelles                       | 
| SL | Sierra Leone                     | 
| SG | Singapore                        | 
| SK | Slovak Republic                  | 
| SI | Slovenia                         | 
| SB | Solomon Islands                  | 
| SO | Somalia                          | 
| ZA | South Africa                     | 
| ES | Spain                            | 
| LK | Sri Lanka                        | 
| WL | St Lucia                         | 
| SD | Sudan                            | 
| SR | Suriname                         | 
| SZ | Swaziland                        | 
| SE | Sweden                           | 
| CH | Switzerland                      | 
| SY | Syria                            | 
| TW | Taiwan                           | 
| TZ | Tanzania                         | 
| TH | Thailand                         | 
| TG | Togo                             | 
| TO | Ton                              | 
| TT | Trinidad and Tobago              | 
| TN | Tunisia                          | 
| TR | Turkey                           | 
| TM | Turkmenistan                     | 
| US | US                               | 
| UG | Uganda                           | 
| UA | Ukraine                          | 
| AE | United Arab Emirates             | 
| UZ | Uzbekistan                       | 
| VE | Venezuela                        | 
| VN | Vietnam                          | 
| YE | Yemen                            | 
| ZM | Zambia                           | 
| ZW | Zimbabwe                         | 

