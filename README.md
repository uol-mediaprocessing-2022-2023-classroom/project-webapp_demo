# cewe_api_demo

## Setup

1. Zuerst muss Node.js installiert werden, für Windows kann ein Installer genutzt werden: https://nodejs.org/en/download/
2. Jetzt können die Dependencies über die Konsole installiert werden mit: ```npm install```


## Compile and Start
```npm run serve```<br>
<br>
Nach dem starten kann man auf die Seite über localhost zugreifen.<br>
INFO: Der Browser wird die Seite als unsicher anzeigen, da die SSL Zertifikate 'self-signed' sind und nicht überprüft werden können.

<br>

## Zu der App
Diese Repo dient als ein Beispiel für die Nutzung einer Vue basierten App als Frontend für die Interaktion mit der CEWE API, sowie einem Backend.<br>
Um die App zu Nutzen ist ein CEWE myPhotos Konto notwendig (https://www.cewe-myphotos.com/en-gb/). In den Feldern 'Username' und 'Password' der App müssen der Nutzername sowie das Passwort des CEWE Kontos eingetragen werden, danach können die Fotos von dem Konto mithilfe von 'Load Images' in die App geladen werden.<br>
Der "Apply Blur" Button sendet eine Anfrage, die ein ausgewähltes Bild beinthaltet, an das lokale Backend (dieses befindet sich in dieser Repo: https://github.com/OFFIS-Deep-Learning-For-Media/cewe_api_demo_backend) und wartet auf eine Antwort.
