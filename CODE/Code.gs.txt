// --- CONFIGURATIE ---
const OCR_PREFIX = "[OCR] ";
const MAX_FILES_AT_ONCE = 5; // Limiet om time-outs te voorkomen

/**
 * Helper-functie om de juiste vertaalde string op te halen.
 * @param {string} key De sleutel van de string (bv. 'cardTitle').
 * @param {string} locale De taalcode van de gebruiker (bv. 'nl' of 'en-US').
 * @returns {string} De vertaalde string.
 */
function getString(key, locale) {
  const lang = locale ? locale.split('-')[0] : 'en';
  const strings = STRINGS[lang] || STRINGS.en; // Val terug op Engels
  return strings[key] || STRINGS.en[key] || `??${key}??`; // Val terug op Engels als sleutel niet bestaat
}

/**
 * Creëert de "homepage" kaart die wordt getoond als er GEEN bestanden zijn geselecteerd.
 */
function maakHomepageKaart(e) {
  const userLocale = e.commonEventObject.userLocale;
  return createMainInterfaceCard({ commonEventObject: { userLocale: userLocale }, drive: { selectedItems: [] } });
}

/**
 * Hoofdfunctie die wordt aangeroepen als de gebruiker een of meer items selecteert.
 */
function onDriveItemsSelected(e) {
  return createMainInterfaceCard(e);
}

/**
* Bouwt de hoofdinterface op met twee aparte, duidelijke secties.
*/
function createMainInterfaceCard(e) {
  const userLocale = e.commonEventObject.userLocale;
  const cardBuilder = CardService.newCardBuilder()
    .setHeader(CardService.newCardHeader().setTitle(getString('cardTitle', userLocale)));

  const imageMimeTypes = [MimeType.JPEG, MimeType.PNG, 'image/gif', 'image/bmp'];
  let selectedImages = [];
  let selectedOcrDocs = [];

  if (e && e.drive && e.drive.selectedItems) {
    selectedImages = e.drive.selectedItems.filter(item => imageMimeTypes.includes(item.mimeType));
    selectedOcrDocs = e.drive.selectedItems.filter(item => item.mimeType === MimeType.GOOGLE_DOCS && item.title.startsWith(OCR_PREFIX));
  }

  const isOverLimit = selectedImages.length > MAX_FILES_AT_ONCE;

  const convertButton = CardService.newTextButton()
    .setText(getString('step1ButtonText', userLocale).replace('{count}', selectedImages.length))
    .setOnClickAction(CardService.newAction().setFunctionName('handleConvertClick'))
    .setDisabled(selectedImages.length === 0 || isOverLimit);

  const combineButton = CardService.newTextButton()
    .setText(getString('step2ButtonText', userLocale).replace('{count}', selectedOcrDocs.length))
    .setOnClickAction(CardService.newAction().setFunctionName('handleCombineClick'))
    .setDisabled(selectedOcrDocs.length === 0);

  const decoratedSwitch = CardService.newDecoratedText()
    .setText(getString('sortLabel', userLocale))
    .setSwitchControl(CardService.newSwitch().setFieldName("sort_descending").setValue("true"));
  
  const sectionStep1 = CardService.newCardSection()
    .setHeader(getString('step1Header', userLocale))
    .addWidget(CardService.newTextParagraph().setText(getString('step1Instruction', userLocale).replace('{max_files}', MAX_FILES_AT_ONCE)))
    .addWidget(convertButton);
  
  if (isOverLimit) {
    sectionStep1.addWidget(CardService.newTextParagraph().setText(
        getString('warningLimit', userLocale).replace('{max_files}', MAX_FILES_AT_ONCE)
    ));
  }

  const sectionStep2 = CardService.newCardSection()
    .setHeader(getString('step2Header', userLocale))
    .addWidget(CardService.newTextParagraph().setText(getString('step2Instruction', userLocale)))
    .addWidget(decoratedSwitch)
    .addWidget(combineButton);
    
  cardBuilder.addSection(sectionStep1);
  cardBuilder.addSection(sectionStep2);

  return cardBuilder.build();
}

/**
 * ACTIE VOOR STAP 1: Zet geselecteerde afbeeldingen om naar Google Docs.
 */
function handleConvertClick(e) {
  const userLocale = e.commonEventObject.userLocale;
  if (!e.drive || !e.drive.selectedItems || e.drive.selectedItems.length === 0) {
    return CardService.newActionResponseBuilder().setNotification(CardService.newNotification().setText(getString('errorNoFiles', userLocale))).build();
  }

  const imageMimeTypes = [MimeType.JPEG, MimeType.PNG, 'image/gif', 'image/bmp'];
  const selectedImages = e.drive.selectedItems.filter(item => imageMimeTypes.includes(item.mimeType));
  
  if (selectedImages.length === 0) {
    return CardService.newActionResponseBuilder().setNotification(CardService.newNotification().setText(getString('errorNoImages', userLocale))).build();
  }
  
  if (selectedImages.length > MAX_FILES_AT_ONCE) {
    return CardService.newActionResponseBuilder().setNotification(CardService.newNotification().setText(getString('errorFileLimit', userLocale).replace('{max_files}', MAX_FILES_AT_ONCE))).build();
  }

  let convertedCount = 0;
  let errorCount = 0;

  selectedImages.forEach(image => {
    let success = false;
    for (let i = 0; i < 4; i++) { // Retry-mechanisme
      try {
        const imageFile = DriveApp.getFileById(image.id);
        const imageBlob = imageFile.getBlob();
        const newDocName = OCR_PREFIX + image.title;
        const parentFolder = imageFile.getParents().next();
        const resource = { title: newDocName, parents: [{ id: parentFolder.getId() }] };
        Drive.Files.insert(resource, imageBlob, { ocr: true, ocrLanguage: 'nl' });
        convertedCount++;
        success = true;
        break; 
      } catch (err) {
        if (err.message.includes('User rate limit exceeded')) {
          Logger.log(`Rate limit geraakt voor ${image.title}. Poging ${i + 1}. Wacht...`);
          Utilities.sleep(Math.pow(2, i + 1) * 1000 + Math.random() * 1000);
        } else {
          Logger.log(`Onherstelbare fout voor ${image.title}: ${err.toString()}`);
          success = true; 
          errorCount++;
          break;
        }
      }
    }
    if (!success) {
      Logger.log(`Alle pogingen voor ${image.title} zijn mislukt.`);
      errorCount++;
    }
  });

  let message = getString('convertSuccess', userLocale).replace('{count}', convertedCount);
  if (errorCount > 0) {
    message += getString('convertFail', userLocale).replace('{error_count}', errorCount);
  }
  
  return CardService.newActionResponseBuilder()
      .setNotification(CardService.newNotification().setText(message))
      .setNavigation(CardService.newNavigation().updateCard(createMainInterfaceCard(e)))
      .build();
}

/**
 * ACTIE VOOR STAP 2: Combineert de GESELECTEERDE [OCR] docs.
 */
function handleCombineClick(e) {
  const userLocale = e.commonEventObject.userLocale;
  if (!e.drive || !e.drive.selectedItems || e.drive.selectedItems.length === 0) {
    return CardService.newActionResponseBuilder().setNotification(CardService.newNotification().setText(getString('errorNoOcrDocs', userLocale))).build();
  }

  const ocrDocsToProcess = e.drive.selectedItems.filter(item => item.mimeType === MimeType.GOOGLE_DOCS && item.title.startsWith(OCR_PREFIX));

  if (ocrDocsToProcess.length === 0) {
    return CardService.newActionResponseBuilder().setNotification(CardService.newNotification().setText(getString('errorNoOcrDocs', userLocale))).build();
  }

  const sortDescending = e.formInput && e.formInput.sort_descending === "true";

  try {
    const firstDocFile = DriveApp.getFileById(ocrDocsToProcess[0].id);
    const folder = firstDocFile.getParents().next();
    let combinedText = '';

    if (sortDescending) {
      ocrDocsToProcess.sort((a, b) => b.title.localeCompare(a.title, undefined, { numeric: true }));
    } else {
      ocrDocsToProcess.sort((a, b) => a.title.localeCompare(a.title, undefined, { numeric: true }));
    }

    ocrDocsToProcess.forEach((docInfo, index) => {
      const originalName = docInfo.title.substring(OCR_PREFIX.length);
      if (index > 0) { combinedText += `\n\n════════════════════════════════════\n\n`; }
      combinedText += `═══════ [Start van ${originalName}] ═══════\n\n`;
      const doc = DocumentApp.openById(docInfo.id);
      combinedText += doc.getBody().getText();
      combinedText += `\n\n═══════ [Einde van ${originalName}] ═══════`;
    });

    const finalDocName = `Gecombineerde Tekst - ${folder.getName()}`;
    const finalDoc = DocumentApp.create(finalDocName);
    finalDoc.getBody().setText(combinedText);
    const finalFile = DriveApp.getFileById(finalDoc.getId());
    folder.addFile(finalFile);
    DriveApp.getRootFolder().removeFile(finalFile);

    // De individuele [OCR] bestanden worden niet verwijderd, zodat de gebruiker ze kan controleren.
    // tempDocIds.forEach(id => { DriveApp.getFileById(id).setTrashed(true); });

    const message = getString('combineSuccess', userLocale).replace('{count}', ocrDocsToProcess.length);
    const resultCard = CardService.newCardBuilder()
      .setHeader(CardService.newCardHeader().setTitle(getString('cardCompleteTitle', userLocale)))
      .addSection(CardService.newCardSection()
        .addWidget(CardService.newTextParagraph().setText(message))
        .addWidget(CardService.newButtonSet().addButton(
          CardService.newTextButton()
            .setText(getString('openFinalDoc', userLocale))
            .setOpenLink(CardService.newOpenLink().setUrl(finalDoc.getUrl()))
        ))
      ).build();

    return CardService.newActionResponseBuilder()
      .setNotification(CardService.newNotification().setText(message))
      .setNavigation(CardService.newNavigation().updateCard(resultCard))
      .build();

  } catch (err) {
    Logger.log(`Fout bij combineren: ${err.toString()}`);
    const errorMessage = getString('genericError', userLocale).replace('{error_message}', err.message);
    return CardService.newActionResponseBuilder().setNotification(CardService.newNotification().setText(errorMessage)).build();
  }
}
