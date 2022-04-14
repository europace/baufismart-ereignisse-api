# Ereignisse-API
You can get all events of a case or application including timestamp, actors, type, link to documents and many more.

![advisor](https://img.shields.io/badge/-advisor-lightblue)
![loanProvider](https://img.shields.io/badge/-loanProvider-lightblue)
![mortgageLoan](https://img.shields.io/badge/-mortgageLoan-lightblue)
![consumerLoan](https://img.shields.io/badge/-consumerLoan-lightblue)

[![Authentication](https://img.shields.io/badge/Auth-OAuth2-green)](https://github.com/europace/authorization-api)
[![GitHub release](https://img.shields.io/github/v/release/europace/baufismart-ereignisse-api)](https://github.com/europace/baufismart-ereignisse-api/releases)

[![Pattern](https://img.shields.io/badge/Pattern-Tolerant%20Reader-yellowgreen)](https://martinfowler.com/bliki/TolerantReader.html)

## Documentation
[![YAML](https://img.shields.io/badge/OAS-HTML_Doc-lightblue)](https://europace.github.io/baufismart-ereignisse-api/gh-pages/index.html)
[![YAML](https://img.shields.io/badge/OAS-YAML-lightgrey)](https://raw.githubusercontent.com/europace/baufismart-ereignisse-api/master/ereignisse-openapi.yaml)

## Usecases
- display events in a CRM system or app
- detailed analyses of time and content for processing the case

## Requirements
- authenticated as loan provider

## Quick Start
To test our APIs and your use cases as quickly as possible, we have created a [Postman Collection](https://github.com/europace/api-quickstart) for you.

### Authentication
Please use [![Authentication](https://img.shields.io/badge/Auth-OAuth2-green)](https://docs.api.europace.de/common/authentifizierung/authorization-api/) to get access to the APIs. The OAuth2 client requires the following scopes:

| Scope                                  | API Usecase                                   |
|----------------------------------------|-----------------------------------------------|
| `baufinanzierung:echtgeschaeft`        | to use api in production mode                 |
| `baufinanzierung:ereignis:lesen`       | to get event information                      |
| `partner:plakette:lesen`               | to get partner information                    |
| `unterlagen:dokument:lesen`            | to get documents                              |


## Get events of a case

As advisor you can get all events of case to get an overview for the last actions.

example-request:
``` http
GET /v2/ereignisse/CH6407 HTTP/1.1
Host: baufismart.api.europace.de
Content-Type: application/json
Authorization: Bearer {{access-token}}
```

example-response: \
[siehe API-Spezifikation](https://europace.github.io/baufismart-ereignisse-api/gh-pages/index.html#get-/ereignisse/-vorgangsNummer-)

``` json
{
        "vorgangsNummer": "CH6407",
        "meldung": "Herr Musterkunde meldet sich am Montag nochmal zum Finanzierungsvorschlag.",
        "typ": "KOMMENTAR",
        "zeitpunkt": "2018-03-02T17:09:34.952+01:00",
        "quelle": {
            "partnerId": "DIV95",
            "anrede": "HERR",
            "nachname": "Musterberater",
            "vorname": "Max",
            "externePartnerId": "meineId"
        }
    },
    {
        "vorgangsNummer": "CH6407",
        "meldung": "Finanzierungsvorschlag an theo.musterkunde@gmail.com versandt",
        "typ": "KOMMUNIKATION",
        "zeitpunkt": "2018-03-02T16:58:04.952+01:00",
        "quelle": {
          "partnerId": "DIV95",
          "anrede": "HERR",
          "nachname": "Musterberater",
          "vorname": "Max",
          "externePartnerId": "meineId"
        },
        "dokumente": [
            {
                "_links": {
                    "self": {
                        "href": "https://api.europace.de/v1/dokumente/5c4ac67de4b0829c9dc13a04"
                    },
                    "download": {
                        "href": "https://api.europace.de/v1/dokumente/5c4ac67de4b0829c9dc13a04/content"
                    },
                    "publicDownload": {
                        "href": "https://www.europace2.de/dokumentenverwaltung/download?id=354d44fbe827b624bc4b439ba0bdb4aa33b941e628ddd3e5ca0f9b47677ba9c419668c654887c2489f4535f17ec4a7f71657344c128d0d04acf5fbff0e5e92a0"
                    }
                },
                "name": "Ihre Finanzierungsanfrage (Musterkunde und Musterfrau) (25.01.2019)"
            }
        ]
    },
    {
     "vorgangsNummer": "CH6407",
     "meldung": "Datenschutzklausel vom Kunden unterschrieben und Vorvertragliche Informationspflichten nach §655a Abs. 2 Satz 1 BGB i.V.m. Art. 247 §13 EGBGB vom Vermittler bestätigt. Soweit Beratungsleistungen angeboten wurden, wurde zusätzlich bestätigt, dass den vorvertraglichen Informationspflichten nach Art. 247 § 13b Absatz 3 i.V.m. Art. 247 § 18 EGBGB nachgekommen wurde.",
     "typ": "HINWEIS",
     "zeitpunkt": "2018-03-02T16:45:00.952+01:00",
     "quelle": {
       "partnerId": "DIV95",
       "anrede": "HERR",
       "nachname": "Musterberater",
       "vorname": "Max",
       "externePartnerId": "meineId"
     }
    },
    {
        "vorgangsNummer": "CH6407",
        "meldung": "Vorgang angelegt",
        "typ": "STATUS_AENDERUNG",
        "zeitpunkt": "2018-03-02T16:29:53.952+01:00",
        "quelle": {
          "partnerId": "DIV95",
          "anrede": "HERR",
          "nachname": "Musterberater",
          "vorname": "Max",
          "externePartnerId": "meineId"
        }
    }
```

## Get events of an application

As loan officer you can get all events of application to get an overview for the last actions.

example-request:
``` cURL
curl -X GET \
  'https://baufismart.api.europace.de/v2/ereignisse/DM2902/1/1' \
  -H 'Authorization: Bearer eyJj...GVkA' \
  -H 'X-TraceID: myTest123' \
  -H 'cache-control: no-cache'
```

example-response: \
[siehe API-Spezifikation](https://europace.github.io/baufismart-ereignisse-api/gh-pages/index.html#get-/ereignisse/-vorgangsNummer-/-antragsNummer-/-teilAntragsNummer-)

## Terms of use
The APIs are provided under the following [Terms of Use](https://docs.api.europace.de/nutzungsbedingungen).

## Support
If you have any questions or problems, you can contact devsupport@europace2.de.
