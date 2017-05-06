# OIREAPULAINEN
#### IBM-Duodecim tiimi, 2017-05-06
Pekka Leppänen, Sandra Calvo, Ville Lindholm, Jukka Ruponen


## ESITYS
(https://github.com/oirearviohack/ibm-duodecim/blob/master/ODA-esitys%20v2.pdf)


## ASENNUSOHJEET


### A. Node-RED voicechat flow

![Node-RED flow](https://raw.githubusercontent.com/oirearviohack/ibm-duodecim/master/oda-node-flow-sample.png)

Node-RED flow implementoi chat-liittymän (http://your-node-red-url.mybluemix.net/chat), puheentunnistusta tukevan chat-liittymän (http://your-node-red-url.mybluemix.net/voicechat) ja websocketin, jonka kautta kaikki keskustelu kulkee. Lisäksi flow implementoi keskustelun interaktiot Conversation, Tone Analyzer ja Sentiment palvelujen kanssa.
Flow:ssa on myös valmiina "http post" input node johon käyttäjän input työntää jostakin muualta, esim. iOS sovelluksesta (vastausten kuuntelu tapahtuu em. websocketin kautta).

Huom! Voicechat puheliittymä käyttää nyt selaimen puheentunnistusta (webkitSpeechRecognition kirjasto), joka toimii ainakin Chromella, ehkä myös Firefoxilla. Lisäksi flow:ssa käytetään hyväksi Google Translate -palvelua (oda-translate), jolla käyttäjän syöttämät viestit käännetään englanniksi Tone Analyzer ja Sentiment analyysiä varten.
![Voicechat esimerkki](https://raw.githubusercontent.com/oirearviohack/ibm-duodecim/master/oda-voicechat-ui-sample.png)


#### Asennus ja konfigurointi
1. Kirjaudu Bluemixiin (http://bluemix.net)
2. Mene "Bluemix Catalog" ja luo nämä:
- Boilerplates / "Node-RED starter"
- Watson / Conversation
- Watson / Tone Analyzer
3. Bindaa Conversation ja Tone Analyzer
4. Kun Node-RED palvelu on käynnistynyt, bindaa Conversation ja Tone Analyzer palvelut siihen (Connect)
5. Kopioi oda-node-flow.txt tiedoston sisältö leikepöydälle
6. Avaa Node-RED UI
7. Node-RED UI:ssa, valitse Menu > Import > Clipboard, liitä flow kuvaus ja paina "Import"
8. Paina [Deploy]
9. HUOM! Joudut asentamaan "node-red-dashboard" komponentit, jotta sentiment/pi mittaristot toimivat (http://your-node-red-url.mybluemix.net/ui):
Valitse Node-RED:ssä Menu > Manage Palette > Install ja asenna "node-red-dashboard".
![Sentiment dashboard esimerkki](https://github.com/oirearviohack/ibm-duodecim/blob/master/oda-sentiment-pi-ui-sample.png?raw=true)

Conversation service:
1. Avaa Conversation palvelun käyttöliittymä
2. "Workspace" ruudulla, valitse "Import workspace" ja valitse oda-conversation-workspace.json tiedosto

Translation service:
1. Pura oda-translate.zip levyllesi
2. Avaa app.js ja laita oma Google API avaimesi (http://developers.google.com)
2. Asenna CF CLI, mikäli sinulla ei sitä jo ole (https://console.ng.bluemix.net/docs/cli/index.html#cli)
3. Kirjaudu Bluemixiin CF:llä
$ cf login
3. Siirry oda-translate hakemistoon ja vie sovellus Bluemixiin
$ cf push oda-translate
4. Käy Node-RED:ssä konfiguroimassa "translate" ja "http" nodet jotta ne viittaavat sinun oda-translate URL:ään.


### B. Node-Red chat integraatio web-sivulle

Tämä Node-RED flow implementoi chatbot-liittymän (http://your-node-red-url.mybluemix.net/bot) jossa voi keskustella kirjoittamalla suoraan suomenkielellä. Lisäksi flow implementoi keskustelun interaktiot Conversation palvelun kanssa. Tämä esimerkki hyödyntää IBM Virtual Agent palvelu, joka auttaa keskustelun luomisessa suoraan selaimesta ja ilman mitään koodausta. Chatti näkyy ODA demo sivuun vieressä.

#### Asennus ja konfigurointi

Node-RED flow: (voit käyttää luomasi sovellus kohdassa 'A')
1. Kirjaudu Bluemixiin (http://bluemix.net)
2. Mene "Bluemix Catalog" ja luo nämä:
- Boilerplates / "Node-RED starter"
- Watson / Conversation
3. Bindaa Conversation
4. Kun Node-RED palvelu on käynnistynyt, bindaa Conversation palvelu siihen (Connect)
5. Kopioi oda-node-red-chatbot tiedoston sisältö leikepöydälle
6. Avaa Node-RED UI
7. Node-RED UI:ssa, valitse Menu > Import > Clipboard, liitä flow kuvaus ja paina "Import"
8. Paina [Deploy]

IBM Virtual Agent
1. Avaa (https://www.ibm.com/us-en/marketplace/cognitive-customer-engagement) ja ota Virtual Agent käyttöön. 
2. Voit vapaasti muokata keskustelun suoraan selaimesta. Likkaa sinun oma Watson Conversation palvelu: 'HOME'-'Linked workspaces' HUOM! Virtual Agent toimii vain englannin kielellä.
3. Tarvitset Virtual Agent credentiaalit saadakseen Node-Red flown toimimaan. Saat niitä Virtual Agent APIn kautta. 
Lisätietoja tässä: https://www.ibm.com/us-en/marketplace/cognitive-customer-engagement







