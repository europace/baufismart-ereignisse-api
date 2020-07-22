# Baufismart Ereignisse-API
Die Ereignisse-API liefert die Ereignisse eines Vorgangs bzw. eines Antrags inkl. Zeitpunkt, Typ, Ersteller, Text und ggf. verlinkter Dokumente zurück.

*Aktuelle Version: 2.0.0*
* [Release Notes](https://github.com/hypoport/ep-ereignisse-api/releases)

> :warning: Diese Schnittstelle wird kontinuierlich weiterentwickelt. Daher erwarten wir 
> von allen Nutzern dieser Schnittstelle, dass sie das "[Tolerant Reader Pattern](https://martinfowler.com/bliki/TolerantReader.html)" nutzen, d.h. 
> tolerant gegenüber kompatiblen API-Änderungen beim Lesen und Prozessieren der Daten sind:
>
> 1. unbekannte Felder dürfen keine Fehler verursachen
>
> 2. Strings mit eingeschränktem Wertebereich (Enums) müssen mit neuen, unbekannten Werten umgehen können
>
> 3. sinnvoller Umgang mit HTTP-Statuscodes, die nicht explizit dokumentiert sind  
> 
 
<!-- https://opensource.zalando.com/restful-api-guidelines/#108 -->

# Inhaltsverzeichnis

* [Allgemeines](#allgemeines)
* [Authentifizierung](#authentifizierung)
* [TraceId zur Nachverfolgbarkeit von Requests](#traceid-zur-nachverfolgbarkeit-von-requests)
* [Content-Type](#content-type)
* [Fehlercodes](#fehlercodes)
   * [HTTP-Status Errors](#http-status-errors)
   * [weitere Fehler](#weitere-fehler)
* [API-Spezifikation](#api-spezifikation)
* [API-Referenz](#api-referenz)
* [Beispiel](#beispiel)
* [FAQs](#faqs)
* [Tools](#tools)
* [Kontakt](#kontakt)
* [Nutzungsbedingungen](#nutzungsbedingungen)

# Dokumentation

## Allgemeines

Für einen Schnelleinstieg, siehe [Tools](#tools)

## Authentifizierung

Für jeden Request ist eine Authentifizierung erforderlich. Die Authentifizierung erfolgt über den OAuth 2.0 Client-Credentials Flow. 

| Request Header Name | Beschreibung           |
|---------------------|------------------------|
| Authorization       | OAuth 2.0 Bearer Token |


Das Bearer Token kann über die [Authorization-API](https://github.com/europace/authorization-api) angefordert werden. 
Dazu wird ein Client benötigt der vorher von einer berechtigten Person über das Partnermanagement angelegt wurde, 
eine Anleitung dafür befindet sich im [Help Center](https://europace2.zendesk.com/hc/de/articles/360012514780).

Damit der Client für diese API genutzt werden kann, müssen im Partnermanagement die folgenden Berechtigungen aktiviert werden

| Name                                        | Hintergrund                                         |
| ------------------------------------------- | --------------------------------------------------- |
| **Baufinanzierungsereignisse lesen**        | Grundsätzlich zum Auslesen von Ereignissen benötigt |
| **Baufinanzierung-Echtgeschäft bearbeiten** | sonst sind nur Ereignisse zu Testvorgängen lesbar   |
| **Darf Partner-Daten lesen**                | sonst sind werden nur `PartnerIds` ausgegeben       |
| **Dokumente lesen**                         | sonst werden keine Dokumente ausgegeben             |
 
Schlägt die Authentifizierung fehl, erhält der Aufrufer eine HTTP Response mit Statuscode **401 UNAUTHORIZED**.

Hat der Client nicht die benötigte Berechtigung um die Resource abzurufen, erhält der Aufrufer eine HTTP Response mit Statuscode **403 FORBIDDEN**.

## TraceId zur Nachverfolgbarkeit von Requests

Für jeden Request soll eine eindeutige ID generiert werden, die den Request im EUROPACE 2 System nachverfolgbar macht und so bei etwaigen Problemen oder Fehlern die systemübergreifende Analyse erleichtert.  
Die Übermittlung der X-TraceId erfolgt über einen HTTP-Header. Dieser Header ist optional,
wenn er nicht gesetzt ist, wird eine ID vom System generiert.

| Request Header Name | Beschreibung                    | Beispiel    |
|---------------------|---------------------------------|-------------|
| X-TraceId           | eindeutige Id für jeden Request | sys12345678 |

## Content-Type

Die Schnittstelle akzeptiert Daten mit Content-Type `application/json`.  
Entsprechend muss im Request der Content-Type Header gesetzt werden. Zusätzlich das Encoding, wenn es nicht UTF-8 ist.

| Request Header Name |   Header Value   |
|---------------------|------------------|
| Content-Type        | application/json |

## Fehlercodes

### HTTP-Status Errors

| Fehlercode | Nachricht       | weitere Attribute          | Erklärung                            |
|------------|-----------------|----------------------------|--------------------------------------|
| 401        | Unauthorized    | -                          | Authentifizierung ist fehlgeschlagen |

### Weitere Fehler

| Fehlercode | Nachricht                  | Erklärung                                                                                       |
|------------|----------------------------|-------------------------------------------------------------------------------------------------|
| 403        | Insufficient access rights | Es wird versucht auf eine Ressource zuzugreifen, die die Vertriebsorganisation nicht lesen darf |

## API Spezifikation
Die API ist vollständig in Swagger definiert und steht im YAML-Format zur Verfügung. Für die Generierung eines Clients empfehlen wir Swagger Codegen.

* [Swagger.yaml](https://github.com/hypoport/ep-ereignisse-api/blob/master/swagger.yaml)

## API Referenz

Siehe Swagger

## Beispiel

Ereignisse zu einem Vorgang auslesen. Beispiel:

```
curl -X GET \
  'https://baufismart.api.europace.de/v2/ereignisse/DM2902' \
  -H 'Authorization: Bearer eyJj...GVkA' \
  -H 'X-TraceID: myTest123' \
  -H 'cache-control: no-cache'
```

Ereignisse zu einem Ereignis auslesen. Beispiel:

```
curl -X GET \
  'https://baufismart.api.europace.de/v2/ereignisse/DM2902/1/1' \
  -H 'Authorization: Bearer eyJj...GVkA' \
  -H 'X-TraceID: myTest123' \
  -H 'cache-control: no-cache'
```

## FAQs
[https://developer.europace.de/faq/](https://developer.europace.de/faq/)

## Tools

Für [Postman](https://www.getpostman.com/) stellen wir im [Schnellstarter-Projekt](https://github.com/europace/api-schnellstart/) 
auch eine Collection mit einem Beispiel für die Ereignisse-API zur Verfügung.

## Kontakt
Kontakt für Support: devsupport@europace2.de

## Requirements
Nur Bei Bedarf

## Nutzungsbedingungen
Die APIs werden unter folgenden [Nutzungsbedingungen](https://developer.europace.de/terms/) zur Verfügung gestellt.