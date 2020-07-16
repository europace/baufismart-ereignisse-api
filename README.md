# Baufismart Ereignisse-API

Die Ereignisse-API liefert die Ereignisse eines Vorgangs bzw. eines Antrags inkl. Zeitpunkt, Typ, Ersteller, Text und ggf. verlinkter Dokumente zurück.

# Dokumentation
*Aktuelle Version: 1.2.0*

Die API ist vollständig in Swagger definiert und steht im YAML-Format zur Verfügung. Für die Generierung eines Clients empfehlen wir Swagger Codegen.

* [Swagger.yaml](https://github.com/hypoport/ep-ereignisse-api/blob/master/swagger.yaml)
* [Release Notes](https://github.com/hypoport/ep-ereignisse-api/releases)

### API Docs

[API Docs](https://ereignisse-api-12.api-docs.io/1.2.0/ereignisse/)

## Wichtigste Features

Ereignisse zu einem Vorgang auslesen. Beispiel:

```
curl -X GET \
  'https://api.europace.de/v1/ereignisse/DM2902' \
  -H 'Authorization: Bearer eyJj...GVkA' \
  -H 'X-TraceID: myTest123' \
  -H 'cache-control: no-cache'
```

Ereignisse zu einem Ereignis auslesen. Beispiel:

```
curl -X GET \
  'https://api.europace.de/v1/ereignisse/DM2902/1/1' \
  -H 'Authorization: Bearer eyJj...GVkA' \
  -H 'X-TraceID: myTest123' \
  -H 'cache-control: no-cache'
```

### Authentifizierung

Die Authentifizierung läuft über den [OAuth2](https://oauth.net/2/) Flow vom Typ *ressource owner password credentials flow*.
https://tools.ietf.org/html/rfc6749#section-1.3.3

##### Credentials
Um die Credentials zu erhalten, erfragen Sie beim Helpdesk der Plattform die Zugangsdaten zur Auslesen API, bzw. bitten Ihren Auftraggeber dies zu tun.

##### Schritte
1. Absenden eines POST Requests auf den [Login-Endpunkt](https://htmlpreview.github.io/?https://raw.githubusercontent.com/hypoport/antraege-auslesen-api/master/Dokumentation/index.html#_oauth2) mit Username und Password. Der Username entspricht der PartnerId und das Password ist der API-Key. Alternativ kann ein Login auch über einen GET Aufruf mit HTTP Basic Auth auf den Login-Endpunkt erfolgen.
2. Aus der JSON-Antwort das JWToken (access_token) entnehmen
3. Bei weiteren Requests muss dieses JWToken als Authorization Header mitgeschickt werden.

##### Beispiel Implementierung für die Authentifizierung mit einem Java Client und Retrofit

Den API client nicht mit dem Default Konstruktor, sondern dem credentials Kontruktor erzeugen. Z.B:

```
 api = new ApiClient("oauth2","partnerID", "api-key").createService(AntraegeApi.class);
```

## Fragen und Anregungen
Bei Fragen und Anregungen entweder ein Issue in GitHub anlegen oder an [helpdesk@europace2.de](mailto:helpdesk@europace2.de) schreiben.

## Nutzungsbedingungen
Die APIs werden unter folgenden [Nutzungsbedingungen](https://developer.europace.de/terms/) zur Verfügung gestellt

