# OCR-combi

A Google Workspace Add-on for Google Drive that streamlines the process of text recognition (OCR) and document merging.

## Short Description

Do you have multiple photos of a document or several PDF files from which you want to extract and combine the text? OCR-combi solves this problem. This add-on uses Google's built-in OCR functionality to recognize text in multiple image or PDF files at once. It then merges the recognized text from all these files into a single, well-organized Google Document, directly in your Google Drive.

## Why this tool?

The standard OCR process in Google Drive is cumbersome:
1.  Right-click on a single file.
2.  Choose "Open with" -> "Google Docs".
3.  Wait for the conversion to complete.
4.  Repeat this for every file.
5.  Open each new document, copy the text, and paste it into a master document.

OCR-combi automates these steps. You select all your files, click one button for the conversion, then select the new documents and click another button to merge everything. This saves a significant amount of time and mouse clicks.

## Features

-   **Batch OCR:** Perform text recognition on multiple files (`.jpg`, `.png`, `.gif`, `.pdf`) at once.
-   **Automatic Merging:** Combine the text from multiple Google Docs into a new, single document.
-   **Location Preservation:** The final combined document is placed in the same folder as the source files.
-   **Sorting Options:** Choose to sort the documents alphabetically (A-Z) or reverse alphabetically (Z-A) before merging.
-   **Clear Separators:** Each combined text block is provided with a clear header and separator, so you can see the origin of each text.
-   **Timeout Protection:** A built-in limit prevents the script from stopping when processing too many files at once.

## Important Note on Installation

This add-on is not distributed through the official Google Workspace Marketplace. Google's verification requirements (such as a verified domain name and an expensive security audit) are too burdensome for a small-scale, open-source project.

Therefore, you will follow a **manual installation procedure**. During this process, Google will show you a warning like **"Google hasn't verified this app"**. This is **normal and expected** for scripts you install yourself. You are only giving yourself permission to run the script in your own account. The code is fully public in this repository, so you can verify for yourself that your data is not being misused.

## Installation Guide (Step-by-step)

Follow these steps carefully to install the add-on.

### Step 1: Create a new Apps Script project

1.  Go to [script.google.com](https://script.google.com).
2.  Click on **+ New project** in the top left.

### Step 2: Add the project files

You will replace the default files with the code from this repository.

#### A. Code.gs
1.  In the Apps Script project, you will see a file named `Code.gs`.
2.  Delete all existing code in this file.
3.  Copy the entire content of the [**`Code.gs`**](CODE/Code.gs) file from this repository.
4.  Paste the copied code into the empty `Code.gs` in your Apps Script project.

#### B. Strings.gs
1.  In the editor, click the **+** icon next to "Files".
2.  Choose **Script**.
3.  Name the new file `Strings` (the `.gs` extension will be added automatically).
4.  Copy the entire content of the [**`Strings.gs`**](CODE/Strings.gs) file from this repository.
5.  Paste the copied code into your new `Strings.gs` file.

#### C. Manifest file (appsscript.json)
1.  On the left, click the **Project Settings** icon (gear ⚙️).
2.  Check the box **Show "appsscript.json" manifest file in editor**.
3.  Go back to the **Editor** (icon `<>`). You will now see the `appsscript.json` file.
4.  Delete all existing content in this file.
5.  Copy the entire content of the [**`appsscript.json`**](CODE/appsscript.json) file from this repository.
6.  Paste the copied code into your `appsscript.json` file.

### Step 3: Enable the Google Drive API

The script needs an advanced service to perform OCR.
1.  In the editor, click the **+** icon next to "Services".
2.  Find **Google Drive API** in the list and select it.
3.  Click the **Add** button. You should now see "Drive" under the list of Services.

### Step 4: Save the project

Click the **Save project** icon (diskette 💾) at the top.

### Step 5: Deploy the Add-on

1.  In the top right, click the blue **Deploy** button and choose **New deployment**.
2.  Next to "Select type", click the **gear icon** (Enable deployment types).
3.  Select **Add-on**.
4.  Give the deployment a description (e.g., "Version 1").
5.  Click **Deploy**.

### Step 6: Install the Add-on locally

1.  After deployment, a window "Deployment updated" will appear. Copy the **Add-on URL**. It looks like `https://script.google.com/macros/d/{ID}/addon?authuser=0`.
2.  Click **Done**.
3.  Paste the copied URL into a new browser tab and press Enter.
4.  You will see an installation screen for "OCR Combi". Click **Install** and then **Continue**.

### Step 7: Authorize the script

This is the step where the warning appears.
1.  Choose the Google account you want to use the add-on with.
2.  You will see the screen **"Google hasn't verified this app"**. This is the expected warning.
3.  Click **Advanced**.
4.  At the bottom, click the link **Go to [Name of your project] (unsafe)**.
5.  Review the requested permissions (for Drive and Documents) and click **Allow**.

The add-on is now installed and ready to use!

## User Guide

1.  Go to your **Google Drive**.
2.  Select one or more image files (`.jpg`, `.png`, etc.) or PDFs.
3.  The **OCR-combi sidebar** will automatically appear on the right. (If not, refresh the page).
4.  **Step 1: Convert.** Click the button `Step 1: Convert X image(s)`. The script will convert each file to a Google Doc with the prefix `[OCR]`.
5.  **Step 2: Combine.** Wait for the conversion to finish. Now select all the new `[OCR]` documents in your Drive.
6.  The sidebar will update. Choose the desired sort order and click `Step 2: Combine X document(s)`.
7.  A new document named `Combined Text - ...` will be created containing all the text. You can open it directly via the link in the add-on.

**Note:** The individual `[OCR]` documents are not automatically deleted, so you can check the conversion for each file. You can delete them manually after you are satisfied with the final result.

## For Developers

Please feel free to set up the project as a Google Chrome Web Store extension.

The original author does not have a personal web domain, which is a new requirement by Google for oAuth-verification for extensions that use Google/YouTube APIs via the Google Cloud Platform (since Manifest v3).

It is a good program and the author uses it locally (which is allowed by Google). It would be beneficial to the community if someone made a publicly available Chrome extension out of it.

## Disclaimer

This software is provided "as is", without warranty of any kind, express or implied. The author is not liable for any loss of data or damage arising from the use of this software. Use at your own risk.

## License

This project is licensed under the MIT License. There is no `LICENSE` file in this repository, but the standard MIT license terms apply.

