# How to Save HTML Form Data to Google Sheets

Follow these steps to save data from an HTML form directly into a Google Sheet using Google Apps Script.

## Step 1: Create and Deploy Google Apps Script

1. **Create a New Google Sheet**
   - Create a new Google Sheet where the form submissions will be saved.

2. **Open Google Apps Script**
   - In the Google Sheet, click on `Extensions` > `Apps Script`.

3. **Add the Google Apps Script Code**
   - Replace the default code with the following script:

   ```javascript
   function doPost(e) {
     const sheet = SpreadsheetApp.openById("YOUR_SHEET_ID").getSheetByName("Sheet1");
     const data = e.parameter;
     sheet.appendRow([data.name, data.email, data.message]);
     return ContentService.createTextOutput("Success");
   }
   ```

   - Replace `"YOUR_SHEET_ID"` with the ID of your Google Sheet (you can find this in the URL of the Google Sheet).

4. **Deploy the Script as a Web App**
   - Click on the `Deploy` button.
   - Select `New Deployment`.
   - Under `Select type`, choose `Web app`.
   - Set access to `Anyone` (if you want the form to be public).
   - Click `Deploy` and copy the URL provided.

## Step 2: Create the HTML Form

Now that you have the deployment URL, create an HTML form that sends data to the script.

1. **Create an HTML File**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Google Sheets Form</title>
   </head>
   <body>
       <form id="google-sheet-form">
           <label for="name">Name:</label>
           <input type="text" id="name" name="name" required><br><br>

           <label for="email">Email:</label>
           <input type="email" id="email" name="email" required><br><br>

           <label for="message">Message:</label>
           <textarea id="message" name="message" required></textarea><br><br>

           <button type="submit">Submit</button>
       </form>

       <script>
           const scriptURL = 'YOUR_GOOGLE_APP_SCRIPT_DEPLOYMENT_URL';
           const form = document.getElementById('google-sheet-form');

           form.addEventListener('submit', (e) => {
               e.preventDefault();
               fetch(scriptURL, {
                   method: 'POST',
                   body: new FormData(form)
               })
               .then(response => alert('Success! Your submission has been saved.'))
               .catch(error => console.error('Error!', error.message));
           });
       </script>
   </body>
   </html>
   ```

   - Replace `'YOUR_GOOGLE_APP_SCRIPT_DEPLOYMENT_URL'` with the URL of the Google Apps Script that you deployed.

## How It Works

1. **Form Data Collection**: The form collects user inputs for `name`, `email`, and `message`.
2. **JavaScript for Submission**: The JavaScript code uses the `fetch()` API to send form data to the Apps Script URL.
   - **FormData**: This object serializes the form fields to send to the Apps Script.
3. **Google Apps Script**: The script receives the data, and `doPost(e)` processes it to append it to the Google Sheet.

## Important Notes

- Ensure the **Google Sheet ID** and **script URL** are correctly copied.
- Set the web app access permissions to `Anyone` if the form is publicly accessible.
- Use `method: 'POST'` to match the `doPost(e)` function in the script.

After following these steps, when users fill out the form and click "Submit", the data will be saved in your Google Sheet automatically!
