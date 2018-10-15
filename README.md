# Ereignisse-API
*Aktuelle Version: 1.0*

Die Ereignisse-API liefert die Ereignisse eines Vorgangs inkl. Zeitpunkt, Typ, Ersteller, Text und ggf. verlinkter Dokumente zurück.

## Dokumentation
Die API ist vollständig in Swagger definiert und steht im YAML-Format zur Verfügung. Für die Generierung eines Clients empfehlen wir Swagger Codegen.

* [Swagger.yaml](https://github.com/hypoport/ep-ereignisse-api/blob/master/swagger.yaml)
* [Release Notes](https://github.com/hypoport/ep-ereignisse-api/releases)

### API Docs

[API Docs](https://ereignisse-api-3.api-docs.io/1.0/)

## First Steps
Beispiel
```
curl -X GET \
  'https://api.europace.de/v1/ereignisse?vorgangsNummer=DM2902' \
  -H 'Authorization: Bearer eyJj...GVkA' \
  -H 'X-TraceID: myTest123' \
  -H 'cache-control: no-cache'  
```

### Authenifizierung
Für das Nutzen der API ist ein gültiger Europace JWT-Token erforderlich, der als Bearer-Token im HTTP-Header=Authorization mitgegeben wird (siehe Beispiel).
Wie man sich einen JWT-Token erstellt, erfährst du hier:
[europace2-api/PEX-SSO-API.md at master · hypoport/europace2-api · GitHub](https://github.com/hypoport/europace2-api/blob/master/Partnermanagement/PEX-SSO-API.md)

### Parameter
* vorgangsNummer
