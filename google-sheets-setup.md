# Google Sheets Form Integration Setup

## Step 1: Set up Google Apps Script

1. Open your Google Sheet: https://docs.google.com/spreadsheets/d/1lMBzRfnw-I-To_AMSXv3WjE069DjJDHD3I36BxEZCUY/edit?usp=sharing

2. Go to Extensions → Apps Script

3. Delete any existing code and paste this code:

```javascript
function doPost(e) {
  try {
    // Open the spreadsheet
    var sheet = SpreadsheetApp.getActiveSheet();
    
    // Get the data from the form
    var data = JSON.parse(e.postData.contents);
    
    // Add timestamp
    var timestamp = new Date();
    
    // Append the data to the sheet
    sheet.appendRow([
      timestamp,
      data.name,
      data.email,
      data.phone,
      data.service,
      data.message,
      data.source || 'Website'
    ]);
    
    // Return success response
    return ContentService
      .createTextOutput(JSON.stringify({
        'result': 'success',
        'message': 'Data added successfully'
      }))
      .setMimeType(ContentService.MimeType.JSON);
      
  } catch(error) {
    // Return error response
    return ContentService
      .createTextOutput(JSON.stringify({
        'result': 'error',
        'message': error.toString()
      }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

function doGet(e) {
  return ContentService.createTextOutput("This script only accepts POST requests");
}
```

4. Save the script (File → Save)

5. Deploy the script:
   - Click "Deploy" → "New deployment"
   - Choose type: "Web app"
   - Description: "SunStation Contact Form"
   - Execute as: "Me"
   - Who has access: "Anyone"
   - Click "Deploy"

6. Copy the Web App URL (it will look like: https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec)

7. IMPORTANT: Save this URL - you'll need it for the website

## Step 2: Set up your Google Sheet

Make sure your Google Sheet has these column headers in the first row:
- A1: Timestamp
- B1: Name
- C1: Email
- D1: Phone
- E1: Service
- F1: Message
- G1: Source

## Step 3: Update the website

Replace YOUR_GOOGLE_SCRIPT_URL in the script.js file with your actual Web App URL from step 6.

## Troubleshooting

If submissions aren't working:
1. Make sure the Web App is deployed with "Anyone" access
2. Check that column headers match exactly
3. Test the URL directly in browser - should show "This script only accepts POST requests"
4. Check browser console for errors