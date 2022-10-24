# cewe_api_demo

## Setup

1. Zuerst muss Node.js installiert werden, für Windows kann ein Installer genutzt werden: https://nodejs.org/en/download/
2. Jetzt können die Dependencies über die Konsole installiert werden mit: ```npm install```


## Compile und Start
```npm run serve```<br>
<br>
Nach dem starten kann man auf die Seite über localhost zugreifen.<br>
INFO: Der Browser wird die Seite als unsicher anzeigen, da die generierten SSL Zertifikate 'self-signed' sind und nicht überprüft werden können.

<br>

## Zu der App
<p>Diese Repo dient als ein Beispiel für die Nutzung einer Vue basierten App als Frontend für die Interaktion mit der CEWE API, sowie einem Backend.</p>
<p>Um die App zu Nutzen ist ein CEWE myPhotos Konto notwendig (https://www.cewe-myphotos.com/en-gb/). In den Feldern 'Username' und 'Password' der App müssen der Nutzername sowie das Passwort des CEWE Kontos eingetragen werden, danach können die Fotos von dem Konto mithilfe von 'Load Images' in die App geladen werden.</p>
<p>Der "Apply Blur" Button sendet eine Anfrage, die ein ausgewähltes Bild beinthaltet, an das lokale Backend (dieses befindet sich in dieser Repo: https://github.com/OFFIS-Deep-Learning-For-Media/cewe_api_demo_backend) und wartet auf eine Antwort.<br>
<strong>Wichtig</strong>: Vor dem schließen des Servers sollte ausgeloggt werden, ansonsten bleibt der Client in der CEWE API eingeloggt, ohne der benötigten clId um sich auszuloggen.
Dies passiert dann automatich nach einer Stunde, aber bis dann kann man sich nicht nochmal einloggen.</p>

<br>

## CEWE API
<p>
Über https://tcmp.photoprintit.com/apidocs/#/ könnt ihr auf die Dokumentation der CEWE API zugreifen, dort sind alle verfügbaren Endpoints der API aufgelistet,
zudem ist ihre Nutzung beschrieben. (Nutzername: CEWE, Pass: Freude)<br>
</p>
