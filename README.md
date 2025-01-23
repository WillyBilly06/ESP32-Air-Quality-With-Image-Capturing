# ESP32 Air Quality Wiht Image Capturing

This project aims to collect environmental data, including humidity, temperature, and UV levels, while capturing images of the surroundings to monitor and analyze changes in air quality and ecological conditions throughout the day.

# Backend for Google Sheet (Using Google AppScript Extension)

This is where all of the data recording is processed, captured, and backup. There are total of 2 scripts for the project to work:

- CSV File Conversion
  ```cpp
    // Maximum rows allowed per sheet before exporting
    const MAX_ROWS = 50;

    function checkAndExportSheets() {
      const spreadsheet = SpreadsheetApp.getActiveSpreadsheet();
      const sheets = spreadsheet.getSheets();
      const folder = getOrCreateFolder("Exported CSV Files"); // Folder to save CSV files

    sheets.forEach(sheet => {
    const lastRow = sheet.getLastRow();

    if (lastRow >= MAX_ROWS) {
      Logger.log(`Sheet "${sheet.getName()}" has reached the limit. Exporting to CSV...`);

      // Export sheet data to CSV
      const csvFile = exportSheetToCSV(sheet, folder);

      // Reformat the sheet for reuse
      formatSheet(sheet);

      Logger.log(`Sheet "${sheet.getName()}" exported as "${csvFile.getName()}" and reformatted.`);
    }
  });
  }

  // Helper function: Export a sheet as CSV
  function exportSheetToCSV(sheet, folder) {
    const data = sheet.getDataRange().getValues();
    const csvContent = data.map(row => row.join(",")).join("\n");

   const timestamp = new Date().toISOString().replace(/[:.]/g, "-");
    const fileName = `${sheet.getName()}_${timestamp}.csv`;


    const blob = Utilities.newBlob(csvContent, "text/csv", fileName);
    return folder.createFile(blob); // Save the file to the folder

  }

  // Helper function: Format the sheet for reuse
  function formatSheet(sheet) {
  sheet.clear(); // Clear all data
  sheet.clearFormats(); // Clear formatting
  sheet.setRowHeight(1, 30); // Example: Set header row height
  sheet.getRange(1, 1).setValue(""); // Example: Add default headers
  sheet.getRange(1, 2).setValue("");
  sheet.getRange(1, 3).setValue("");
  sheet.getRange(1, 4).setValue("");
  sheet.getRange(1, 5).setValue("");
  sheet.getRange(1, 6).setValue("");
  }

  // Helper function: Get or create a folder in Google Drive
  function getOrCreateFolder(folderName) {
    const folders = DriveApp.getFoldersByName(folderName);
    return folders.hasNext() ? folders.next() : DriveApp.createFolder(folderName);
  }
```
