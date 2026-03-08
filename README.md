# finance-track
Finance Tracker

📊 Personal Financial Dashboard System

A complete, self-archiving financial dashboard built on Google Sheets and a custom HTML/Tailwind CSS Single Page Application.

Architecture Overview

Database: Google Sheets (Flat ledgers for robust data querying).

Backend: Google Apps Script (Serves the UI, handles API calls, automates reports).

Frontend: index.html (A responsive, glassmorphism UI built with Tailwind CSS and Chart.js).

🛠️ Step 1: Database (Google Sheets) Setup

Go to Google Sheets and create a new blank spreadsheet. Name it "Financial Database".

Create four tabs exactly named as follows:

Transactions: Your ongoing ledger.

Headers (A1:F1): ID, Date, Type (Inflow/Adhoc/Budgeted), Category, Name, Amount

Current_State: Your live assets and recurring budgets.

Headers (A1:D1): Type (Asset/Budget), Category, Name, Amount

Historical_Archive: Where the script locks your monthly data.

Headers (A1:E1): Archive_Period, Type, Category, Name, Amount

Report_Dashboard: A hidden tab where you can use Google Sheets' native Pivot Tables and Insert > Charts to build a visual summary of your data.

⚙️ Step 2: Backend (Apps Script) Setup

In your Google Sheet, click Extensions > Apps Script.

Rename the project to "Financial Dashboard Backend".

Code.gs: Delete the default code in Code.gs and paste the contents of the provided Code.gs file.

index.html: Click the + icon next to "Files", select HTML, and name it exactly index. Delete the default code and paste the entirety of your frontend index.html code into this file.

🚀 Step 3: Web App Deployment

To view your dashboard in the browser:

In the Apps Script editor, click Deploy > New deployment in the top right corner.

Click the gear icon next to "Select type" and choose Web app.

Fill out the details:

Description: Initial Release

Execute as: Me (your email)

Who has access: Only myself

Click Deploy. (You may need to authorize the application permissions).

Copy the Web app URL. This is the link to your personal financial dashboard!

⏳ Step 4: Automations & Triggers

To make the app self-archiving and generate your monthly emails:

In the Apps Script editor, click the Clock Icon (Triggers) on the left-hand sidebar.

Click + Add Trigger in the bottom right.

Setup the Archiver:

Function: archiveMonthlyData

Event source: Time-driven

Type of time based trigger: Month timer

Time of month: 1st

Time of day: Midnight to 1am

Save.

Setup the Email Report:

Click + Add Trigger again.

Function: sendMonthlyReport

Event source: Time-driven

Type of time based trigger: Month timer

Time of month: 1st

Time of day: 8am to 9am

Save.

📈 Step 5: The PDF Report Tab

To make your PDF email look good, go to the Report_Dashboard tab in your Google Sheet:

Highlight columns A to Z and set the background color to white (to hide gridlines).

Go to Insert > Pivot Table and select your Transactions sheet as the source.

Set the Rows to Category and Values to Amount.

Highlight your Pivot table, go to Insert > Chart, and make a pie chart or bar chart.

Resize everything to fit neatly into the top-left corner of the screen. When the sendMonthlyReport script runs, it takes a "screenshot" of this specific tab and emails it to you!
