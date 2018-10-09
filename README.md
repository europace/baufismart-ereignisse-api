# ep-ereignisse-api
Öffentliche API um Ereignisse von Vorgängen und Anträgen zu lesen

##### Aktuelle Version: 0.4.0

* Initiale Definition

### Swagger Spezifikation

Die API ist vollständig in Swagger definiert. Die Swagger Definition wird im YAML-Format zur Verfügung gestellt.: [swagger.yaml](swagger.yaml)

Diese Spezifikation kann auch zur Generierung von Clients für diese API verwendet
werden. Dazu empfehlen wir das Tool [Swagger Codegen](https://github.com/swagger-api/swagger-codegen)

# Dokumentation

 - [RELEASE NOTES](https://github.com/hypoport/ep-ereignisse-api/releases)
 
### Generierung des Clients
##### JAVA mit Retrofit

1. Die aktuelle Swagger-Codegen Version, mindestens 2.2.2, downloaden
2. Client mit folgendem Kommando generieren:

Example:

```
java -jar swagger-codegen-cli-2.2.2.jar generate -i swagger.yaml -l java -c codegen-config-file.json -o europace-api-client
```

Example **codegen-config-file.json**:

```
{
  "artifactId": "europace-api-client",
  "groupId": "de.europace.api",
  "library": "retrofit2",
  "artifactVersion": "0.1",
  "dateLibrary": "java8"
}

```

### Authentifizierung

Die Authentifizierung läuft über den [OAuth2](https://oauth.net/2/) Flow vom Typ *ressource owner password credentials flow*.
https://tools.ietf.org/html/rfc6749#section-1.3.3

##### Credentials
Um die Credentials zu erhalten, erfagen Sie beim Helpdesk der Plattform die Zugangsdaten zur Auslesen API, bzw. bitten Ihren Auftraggeber dies zu tun.

##### Schritte
1. Absenden eines POST Requests auf https://api.europace.de/login mit Username und Password. Der Username entspricht der PartnerId und das Password ist der API-Key. Auf dem Testsystem können diese Werte frei gewählt werden. Alternativ kann ein Login auch über einen GET Aufruf mit HTTP Basic Auth auf den Login-Endpunkt erfolgen.
2. Aus der JSON-Antwort das JWToken (access_token) entnehmen
3. Bei weiteren Requests muss dieses JWToken als Authorization Header mitgeschickt werden.

Ein Beispiel mit curl sähe wie folgt aus:

```
curl -X POST \
  https://api.europace.de/login \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/x-www-form-urlencoded' \
  -d 'username={{BaufiSmart Nutzername (Mail-Adresse)}}&password={{BaufiSmart Passwort}}'
```


Der in der Antwort enthaltene Access-Token muss bei allen Folge-Requests im Header `Authorization` im Bearer-Format mit gesendet
werden. Beispiel:

```
curl -X GET \
  http://localhost:8080/v1/ereignisse?vorgangsNummer={vorgangsNummer} \
  -H 'authorization: Bearer {{access_token}}' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' 
```

# Ereignisse abrufen

## Ereignisse mit VorgangsNummer

Die Ereignisse werden mit einer VorgangsNummer abgerufen. Ein möglicher Beispiel-Request wäre:

```
curl -X GET \
  http://localhost:8080/v1/ereignisse?vorgangsNummer=123456 \
  -H 'authorization: Bearer {{access_token}}' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' 
```

## Fragen und Anregungen
Bei Fragen und Anregungen entweder ein Issue in GitHub anlegen oder an [helpdesk@europace2.de](mailto:helpdesk@europace2.de) schreiben.

