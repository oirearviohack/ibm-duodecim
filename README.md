# OIREAPULAINEN
#### IBM-Duodecim tiimi, 2017-05-06
Pekka Leppänen, Sandra Calvo, Ville Lindholm, Jukka Ruponen


## ESITYS (PDF):
https://github.com/oirearviohack/ibm-duodecim/blob/master/IBM-Duodecim-ODA-esitys_v2.pdf

![slide1](https://github.com/oirearviohack/ibm-duodecim/blob/master/IBM-Duodecim-ODA-esitys_v2_kuva1.png?raw=true)
![slide1](https://github.com/oirearviohack/ibm-duodecim/blob/master/IBM-Duodecim-ODA-esitys_v2_kuva2.png?raw=true)


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
5. Kopioi [oda-node-flow.txt](https://raw.githubusercontent.com/oirearviohack/ibm-duodecim/master/oda-node-flow.txt) tiedoston JSON-sisältö leikepöydälle
6. Avaa Node-RED UI
7. Node-RED UI:ssa, valitse Menu > Import > Clipboard, liitä flow kuvaus ja paina "Import"
8. Paina [Deploy]
9. HUOM! Joudut asentamaan "node-red-dashboard" komponentit, jotta sentiment/pi mittaristot toimivat (http://your-node-red-url.mybluemix.net/ui):
Valitse Node-RED:ssä Menu > Manage Palette > Install ja asenna "node-red-dashboard".
![Sentiment dashboard esimerkki](https://github.com/oirearviohack/ibm-duodecim/blob/master/oda-sentiment-pi-ui-sample.png?raw=true)

Conversation service:
1. Avaa Conversation palvelun käyttöliittymä
2. "Workspace" ruudulla, valitse "Import workspace" ja valitse [oda-conversation-workspace.json](https://raw.githubusercontent.com/oirearviohack/ibm-duodecim/master/oda-conversation-workspace.json) tiedosto

Translation service:
1. Pura [oda-translate.zip](https://github.com/oirearviohack/ibm-duodecim/raw/master/oda-translate.zip) levyllesi
2. Avaa app.js ja laita oma Google API avaimesi (http://developers.google.com)
3. Avaa package.json ja vaihda "name" avaimen arvo yksilölliseksi, esim: oda-translate-abc
3. Asenna CF CLI, mikäli sinulla ei sitä jo ole (https://console.ng.bluemix.net/docs/cli/index.html#cli)
4. Kirjaudu Bluemixiin CF:llä (cf login)
5. Siirry oda-translate hakemistoon ja vie sovellus Bluemixiin (cf push oda-translate-abc)
6. Käy Node-RED:ssä päivittämässä "translate" ja "http" nodet, joissa viitataan oda-translate URL:ään (muuta linkki vastaamaan yksilöllistä osoitettasi)


### B. Node-Red chat integraatio web-sivulle

![Node-RED web flow esimerkki](https://github.com/oirearviohack/ibm-duodecim/blob/master/oda-web-node-flow-sample.png?raw=true)

Tämä Node-RED flow upottaa chatbot-liittymän (http://your-node-red-url.mybluemix.net/bot) halutulle verkkosivulle, jolloin sivulla voi keskustella botin kanssa suoraan ja suomenkielellä. Lisäksi flow implementoi keskustelun interaktiot Conversation palvelun kanssa. Tämä esimerkki hyödyntää IBM Virtual Agent palvelua, joka auttaa keskustelun luomisessa suoraan selaimesta ja ilman mitään koodausta. Chatti näkyy ODA demo sivuun vieressä.

![Embed esimerkki](https://github.com/oirearviohack/ibm-duodecim/blob/master/oda-embedded-chatbot-sample.png?raw=true)

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

![IBM Virtual Agent esimerkki](https://github.com/oirearviohack/ibm-duodecim/blob/master/IBM_Virtual_Agent_sample.png?raw=true)






