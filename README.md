# Ereignisse-API
Die API liefert die Ereignisse eines Vorgangs bzw. eines Antrags inkl. Zeitpunkt, Typ, Ersteller, Text und ggf. verlinkter Dokumente zurück.

![Vertrieb](https://img.shields.io/badge/-Vertrieb-lightblue)
![Produktanbieter](https://img.shields.io/badge/-Produktanbieter-lightblue)
![Baufinanzierung](https://img.shields.io/badge/-Baufinanzierung-lightblue)
![Privatkredit](https://img.shields.io/badge/-Privatkredit-lightblue)

[![Authentication](https://img.shields.io/badge/Auth-OAuth2-green)](https://github.com/europace/authorization-api)
[![GitHub release](https://img.shields.io/github/v/release/europace/baufismart-ereignisse-api)](https://github.com/europace/baufismart-ereignisse-api/releases)

[![Pattern](https://img.shields.io/badge/Pattern-Tolerant%20Reader-yellowgreen)](https://martinfowler.com/bliki/TolerantReader.html)

## Dokumentation
[![YAML](https://img.shields.io/badge/OAS-HTML_Doc-lightblue)](https://europace.github.io/baufismart-ereignisse-api/gh-pages/index.html)
[![YAML](https://img.shields.io/badge/OAS-YAML-lightgrey)](https://raw.githubusercontent.com/europace/baufismart-ereignisse-api/master/swagger.yaml)

## Anwendungsfälle der API
- Anzeige der Ereignisse in einem CRM-System oder einer App
- detaillierte zeitliche und inhaltliche Analysen zur Bearbeitung des Vorgangs

# Schnellstart
Damit du unsere APIs und deinen Anwendungsfall schnellstmöglich testen kannst, haben wir eine [Postman-Collection](https://docs.api.europace.de/baufinanzierung/schnellstart/) für dich zusammengestellt.

### Authentifizierung
Bitte benutze [![Authentication](https://img.shields.io/badge/Auth-OAuth2-green)](https://docs.api.europace.de/baufinanzierung/authentifizierung/), um Zugang zur API bekommen. Um die API verwenden zu können, benötigt der OAuth2-Client folgende Scopes:

| Scope                                  | API Usecase                                   |
|----------------------------------------|-----------------------------------------------|
| `baufinanzierung:ereignis:lesen`       | Grundsätzlich zum Auslesen von Ereignissen benötigt |
| `baufinanzierung:echtgeschaeft`        | sonst sind nur Ereignisse zu Testvorgängen lesbar |
| `partner:plakette:lesen`               | sonst werden nur `PartnerIds` ausgegeben      |
| `unterlagen:dokument:lesen`            | sonst werden keine Dokumente ausgegeben       |


## Beispiel: Ereignisse zu einem Vorgang auslesen

Request:
``` cURL
curl -X GET \
  'https://baufismart.api.europace.de/v2/ereignisse/DM2902' \
  -H 'Authorization: Bearer eyJj...GVkA' \
  -H 'X-TraceID: myTest123' \
  -H 'cache-control: no-cache'
```

Response: \
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

## Beispiel: Ereignisse zu einem Antrag auslesen

Request:
``` cURL
curl -X GET \
  'https://baufismart.api.europace.de/v2/ereignisse/DM2902/1/1' \
  -H 'Authorization: Bearer eyJj...GVkA' \
  -H 'X-TraceID: myTest123' \
  -H 'cache-control: no-cache'
```

Response: \
[siehe API-Spezifikation](https://europace.github.io/baufismart-ereignisse-api/gh-pages/index.html#get-/ereignisse/-vorgangsNummer-/-antragsNummer-/-teilAntragsNummer-)


## Support
Bei Fragen oder Problemen kannst du dich an devsupport@europace2.de wenden.

## Nutzungsbedingungen
Die APIs werden unter folgenden [Nutzungsbedingungen](https://docs.api.europace.de/nutzungsbedingungen/) zur Verfügung gestellt.
