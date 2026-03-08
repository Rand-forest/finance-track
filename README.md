📊 Personal Financial Dashboard System (Decoupled Architecture)

A complete, self-archiving financial dashboard built on a decoupled architecture:

Frontend: Hosted on GitHub Pages (HTML/Tailwind CSS Single Page Application)

Backend API: Google Apps Script (Handles REST API requests, data archiving, and automated PDF reporting)

Database: Google Sheets (Flat ledgers for robust data querying)

🛠️ Step 1: Database (Google Sheets) Setup

Go to Google Sheets and create a new blank spreadsheet. Name it "Financial Database".

Create four tabs named exactly as follows:

Transactions: Your ongoing ledger.

Headers (A1:F1): ID, Date, Type (Inflow/Adhoc/Budgeted), Category, Name, Amount

Current_State: Your live assets and recurring budgets.

Headers (A1:D1): Type (Asset/Budget), Category, Name, Amount

Historical_Archive: Where the script locks your monthly data.

Headers (A1:E1): Archive_Period, Type, Category, Name, Amount

Report_Dashboard: A hidden tab where you can use Google Sheets' native Pivot Tables and Insert > Charts to build a visual summary of your data for the PDF email.

⚙️ Step 2: Backend API (Google Apps Script) Setup

In your Google Sheet, click Extensions > Apps Script.

Rename the project to "Financial Dashboard API".

Delete the default code in Code.gs and paste the contents of your updated Code.gs file.

Deploy the API:

Click Deploy > New deployment in the top right corner.

Click the gear icon next to "Select type" and choose Web app.

Fill out the details:

Description: Production API

Execute as: Me (your email)

Who has access: Anyone (Crucial: This must be "Anyone" so GitHub Pages can communicate with it)

Click Deploy. (You will be prompted to authorize permissions).

Copy the "Web app URL". You will need this for your frontend!

🚀 Step 3: Frontend Deployment (GitHub Pages)

Create a new repository on your GitHub account (e.g., personal-finance-dashboard).

Upload your final index.html file to the root of this repository.

Go to the repository Settings > Pages.

Under "Build and deployment", select Deploy from a branch.

Select the main (or master) branch and the /root folder, then click Save.

Wait a minute or two, and GitHub will provide you with a live URL to your hosted dashboard!

🔗 Step 4: Connecting Frontend to Backend

To make your GitHub Pages frontend talk to your Google Sheets database, you will use standard JavaScript fetch() requests.

When you write the final data-saving functions in your index.html, replace the mock logic with a call to the Web app URL you copied in Step 2.

Example wiring for your JavaScript:

const API_URL = "YOUR_GOOGLE_WEB_APP_URL_HERE";

// Example of saving a new transaction
async function saveTransactionToBackend(transactionData) {
    try {
        const response = await fetch(API_URL, {
            method: "POST",
            headers: {
                "Content-Type": "text/plain;charset=utf-8",
            },
            body: JSON.stringify({
                action: "saveTransaction",
                data: transactionData
            })
        });
        const result = await response.json();
        console.log("Save result:", result);
    } catch (error) {
        console.error("Error saving data:", error);
    }
}


(Note: Google Apps Script requires "Content-Type": "text/plain;charset=utf-8" for CORS POST requests to bypass preflight issues).

⏳ Step 5: Automations & Triggers (Archiver & Email)

To make the app self-archiving and generate your monthly emails:

Go back to your Google Apps Script editor.

Click the Clock Icon (Triggers) on the left-hand sidebar.

Click + Add Trigger in the bottom right.

Setup the Archiver:

Choose which function to run: archiveMonthlyData

Select event source: Time-driven

Select type of time based trigger: Month timer

Select time of month: 1st

Select time of day: Midnight to 1am

Save.

Setup the Email Report:

Click + Add Trigger again.

Choose which function to run: sendMonthlyReport

Select event source: Time-driven

Select type of time based trigger: Month timer

Select time of month: 1st

Select time of day: 8am to 9am

Save.

📈 Step 6: The PDF Report Formatting

To make your PDF email look professional, format the Report_Dashboard tab in your Google Sheet:

Highlight columns A to Z and set the background color to white (this hides the default gridlines).

Go to Insert > Pivot Table and select your Transactions sheet as the source.

Set the Rows to Category and Values to Amount.

Highlight your Pivot table, go to Insert > Chart, and make a pie chart or bar chart.

Resize everything to fit neatly into the top-left corner of the screen. When the sendMonthlyReport script runs, it takes a "screenshot" of this specific layout and emails it to you as a PDF!
