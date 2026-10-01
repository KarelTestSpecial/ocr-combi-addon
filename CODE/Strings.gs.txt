// Strings.gs

const STRINGS = {
  // English strings
  en: {
    addonName: "OCR Combi",
    cardTitle: "Image to Text",
    step1Header: "Step 1: Convert Images",
    step1Instruction: "Select 1 to {max_files} image files (JPG, PNG). The add-on processes them in batches to prevent timeouts.",
    step1ButtonText: "Step 1: Convert {count} image(s)",
    step2Header: "Step 2: Combine Documents",
    step2Instruction: "Select all your generated <code>[OCR]</code> documents, choose the sort order, and combine them into one file.",
    step2ButtonText: "Step 2: Combine {count} document(s)",
    sortLabel: "Sort in reverse order (Z-A)",
    warningLimit: "<b>Warning:</b> Please select a maximum of {max_files} files at a time.",
    errorNoFiles: "Error: No files selected.",
    errorNoImages: "No supported image files selected.",
    errorFileLimit: "Error: Do not select more than {max_files} files.",
    errorNoOcrDocs: "No documents with the '[OCR]' prefix selected.",
    convertSuccess: "{count} image(s) successfully converted. You can now check the [OCR] documents.",
    convertFail: " {error_count} conversions failed.",
    combineSuccess: "{count} document(s) successfully combined. The intermediate [OCR] files have NOT been deleted.",
    cardCompleteTitle: "Combination Complete",
    openFinalDoc: "Open Final Document",
    genericError: "An error occurred: {error_message}"
  },
  // Dutch strings
  nl: {
    addonName: "OCR Combi",
    cardTitle: "Afbeelding naar Tekst",
    step1Header: "Stap 1: Afbeeldingen Omzetten",
    step1Instruction: "Selecteer 1 tot {max_files} afbeeldingsbestanden (JPG, PNG). De add-on verwerkt ze in batches om timeouts te voorkomen.",
    step1ButtonText: "Stap 1: Zet {count} afbeelding(en) om",
    step2Header: "Stap 2: Documenten Combineren",
    step2Instruction: "Selecteer alle gegenereerde <code>[OCR]</code>-documenten, kies de sorteervolgorde en voeg ze samen tot één bestand.",
    step2ButtonText: "Stap 2: Combineer {count} document(en)",
    sortLabel: "Sorteer in omgekeerde volgorde (Z-A)",
    warningLimit: "<b>Waarschuwing:</b> Selecteer maximaal {max_files} bestanden tegelijk.",
    errorNoFiles: "Fout: Geen bestanden geselecteerd.",
    errorNoImages: "Geen ondersteunde afbeeldingsbestanden geselecteerd.",
    errorFileLimit: "Fout: Selecteer niet meer dan {max_files} bestanden.",
    errorNoOcrDocs: "Geen documenten met de prefix '[OCR]' geselecteerd.",
    convertSuccess: "{count} afbeelding(en) succesvol omgezet. U kunt de [OCR] documenten nu controleren.",
    convertFail: " {error_count} omzettingen zijn mislukt.",
    combineSuccess: "{count} document(en) succesvol gecombineerd. De tussenliggende [OCR] bestanden zijn NIET verwijderd.",
    cardCompleteTitle: "Combinatie Voltooid",
    openFinalDoc: "Open Einddocument",
    genericError: "Er is een fout opgetreden: {error_message}"
  }
};
