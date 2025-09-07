# OCR-combi

Een Google Workspace Add-on voor Google Drive die het proces van tekstherkenning (OCR) en het samenvoegen van documenten stroomlijnt.

## Korte Omschrijving

Heeft u meerdere foto's van een document of verschillende PDF-bestanden waaruit u de tekst wilt extraheren en combineren? OCR-combi lost dit probleem op. Deze add-on gebruikt de ingebouwde OCR-functionaliteit van Google om tekst te herkennen in meerdere afbeeldings- of PDF-bestanden tegelijk. Vervolgens voegt het de herkende tekst van al deze bestanden samen in één overzichtelijk Google Document, direct in uw Google Drive.

## Waarom deze tool?

Het standaardproces voor OCR in Google Drive is omslachtig:
1.  Klik met de rechtermuisknop op één bestand.
2.  Kies "Openen met" -> "Google Documenten".
3.  Wacht tot de conversie is voltooid.
4.  Herhaal dit voor elk bestand.
5.  Open elk nieuw document, kopieer de tekst, en plak deze in een hoofddocument.

OCR-combi automatiseert deze stappen. U selecteert al uw bestanden, klikt op één knop voor de conversie, selecteert vervolgens de nieuwe documenten en klikt nogmaals op één knop om alles samen te voegen. Dit bespaart aanzienlijk veel tijd en muisklikken.

## Functionaliteiten

-   **Batch OCR:** Voer tekstherkenning uit op meerdere bestanden (`.jpg`, `.png`, `.gif`, `.pdf`) tegelijk.
-   **Automatisch Samenvoegen:** Combineer de tekst van meerdere Google Documenten in een nieuw, enkel document.
-   **Behoud van Locatie:** Het uiteindelijke gecombineerde document wordt in dezelfde map geplaatst als de bronbestanden.
-   **Sorteeropties:** Kies ervoor om de documenten alfabetisch (A-Z) of omgekeerd alfabetisch (Z-A) te sorteren voordat ze worden samengevoegd.
-   **Duidelijke Scheidingstekens:** Elk gecombineerd tekstblok wordt voorzien van een duidelijke header en separator, zodat u de herkomst van elke tekst kunt zien.
-   **Bescherming tegen Timeouts:** Een ingebouwde limiet voorkomt dat het script stopt bij het verwerken van te veel bestanden tegelijk.

## Belangrijke Opmerking over Installatie

Deze add-on wordt niet via de officiële Google Workspace Marketplace gedistribueerd. De verificatie-eisen van Google (zoals een geverifieerde domeinnaam en een dure beveiligingsaudit) zijn te zwaar voor een kleinschalig, open-source project.

Daarom volgt u een **handmatige installatieprocedure**. Tijdens dit proces zal Google u een waarschuwing tonen zoals **"Google heeft deze app niet geverifieerd"**. Dit is **normaal en verwacht** voor scripts die u zelf installeert. U geeft enkel uzelf toestemming om het script in uw eigen account uit te voeren. De code is volledig openbaar in deze repository, dus u kunt zelf controleren dat er geen misbruik wordt gemaakt van uw data.

## Installatiegids (Stap-voor-stap)

Volg deze stappen nauwkeurig om de add-on te installeren.

### Stap 1: Maak een nieuw Apps Script-project

1.  Ga naar [script.google.com](https://script.google.com).
2.  Klik linksboven op **+ Nieuw project**.

### Stap 2: Voeg de projectbestanden toe

U vervangt de standaard bestanden door de code uit deze repository.

#### A. Code.gs
1.  In het Apps Script-project ziet u een bestand genaamd `Code.gs`.
2.  Verwijder alle bestaande code in dit bestand.
3.  Kopieer de volledige inhoud van het [**`Code.gs`**](Code.gs)-bestand uit deze repository.
4.  Plak de gekopieerde code in de lege `Code.gs` in uw Apps Script-project.

#### B. Strings.gs
1.  Klik in de editor op het **+** icoon naast "Bestanden".
2.  Kies **Script**.
3.  Geef het nieuwe bestand de naam `Strings` (de `.gs`-extensie wordt automatisch toegevoegd).
4.  Kopieer de volledige inhoud van het [**`Strings.gs`**](Strings.gs)-bestand uit deze repository.
5.  Plak de gekopieerde code in uw nieuwe `Strings.gs`-bestand.

#### C. Manifestbestand (appsscript.json)
1.  Klik links op het **Projectinstellingen**-icoon (tandwiel ⚙️).
2.  Vink het selectievakje **Manifestbestand 'appsscript.json' in editor tonen** aan.
3.  Ga terug naar de **Editor** (icoon `<>`). U ziet nu het bestand `appsscript.json`.
4.  Verwijder alle bestaande inhoud in dit bestand.
5.  Kopieer de volledige inhoud van het [**`appsscript.json`**](appsscript.json)-bestand uit deze repository.
6.  Plak de gekopieerde code in uw `appsscript.json`-bestand.

### Stap 3: Schakel de Google Drive API in

Het script heeft een geavanceerde service nodig om OCR uit te voeren.
1.  Klik in de editor op het **+** icoon naast "Services".
2.  Zoek in de lijst naar **Google Drive API** en selecteer deze.
3.  Klik op de knop **Toevoegen**. U zou "Drive" nu moeten zien onder de lijst met Services.

### Stap 4: Sla het project op

Klik bovenaan op het **Project opslaan**-icoon (diskette 💾).

### Stap 5: Implementeer de Add-on

1.  Klik rechtsboven op de blauwe knop **Implementeren** en kies **Nieuwe implementatie**.
2.  Klik naast "Selecteer type" op het **tandwiel-icoon** (Implementatietypen inschakelen).
3.  Selecteer **Add-on**.
4.  Geef de implementatie een beschrijving (bijv. "Versie 1").
5.  Klik op **Implementeren**.

### Stap 6: Installeer de Add-on lokaal

1.  Na de implementatie verschijnt een venster "Implementatie bijgewerkt". Kopieer de **Add-on-URL**. Deze ziet eruit als `https://script.google.com/macros/d/{ID}/addon?authuser=0`.
2.  Klik op **Gereed**.
3.  Plak de gekopieerde URL in een nieuw browsertabblad en druk op Enter.
4.  U krijgt een installatiescherm te zien voor "OCR Combi". Klik op **Installeren** en vervolgens op **Doorgaan**.

### Stap 7: Autoriseer het script

Dit is de stap waar de waarschuwing verschijnt.
1.  Kies het Google-account waarmee u de add-on wilt gebruiken.
2.  U ziet het scherm **"Google heeft deze app niet geverifieerd"**. Dit is de verwachte waarschuwing.
3.  Klik op **Geavanceerd**.
4.  Klik onderaan op de link **Doorgaan naar [Naam van uw project] (onveilig)**.
5.  Bekijk de gevraagde toestemmingen (voor Drive en Documents) en klik op **Toestaan**.

De add-on is nu geïnstalleerd en klaar voor gebruik!

## Gebruikershandleiding

1.  Ga naar uw **Google Drive**.
2.  Selecteer één of meerdere afbeeldingsbestanden (`.jpg`, `.png`, etc.) of PDF's.
3.  De **OCR-combi zijbalk** verschijnt automatisch aan de rechterkant. (Zo niet, vernieuw de pagina).
4.  **Stap 1: Omzetten.** Klik op de knop `Stap 1: Zet X afbeelding(en) om`. Het script converteert elk bestand naar een Google Doc met de prefix `[OCR]`.
5.  **Stap 2: Combineren.** Wacht tot de conversie klaar is. Selecteer nu alle nieuwe `[OCR]`-documenten in uw Drive.
6.  De zijbalk wordt bijgewerkt. Kies de gewenste sorteervolgorde en klik op `Stap 2: Combineer X document(en)`.
7.  Een nieuw document genaamd `Gecombineerde Tekst - ...` wordt aangemaakt met daarin alle tekst. U kunt dit direct openen via de link in de add-on.

**Let op:** De individuele `[OCR]`-documenten worden niet automatisch verwijderd, zodat u de conversie per bestand kunt controleren. U kunt ze handmatig verwijderen nadat u tevreden bent met het eindresultaat.

## Disclaimer

Deze software wordt "as is" aangeboden, zonder enige vorm van garantie, expliciet of impliciet. De auteur is niet aansprakelijk voor enig verlies van data of schade die voortvloeit uit het gebruik van deze software. Gebruik op eigen risico.

## Licentie

Dit project is gelicentieerd onder de MIT-licentie. Zie het [LICENSE](LICENSE)-bestand voor meer details.
