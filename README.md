Recruiter's Onboarding Checklist
1. Introduction
This is a web-based, interactive checklist designed to streamline and monitor the onboarding process for new candidates. The application provides a clear, task-by-task view for individual candidates and a high-level dashboard for recruiters to track the progress of all candidates simultaneously.

It uses a simple frontend built with HTML, Tailwind CSS, and Vanilla JavaScript, and leverages Google Sheets as a free and easy-to-manage database through a Google Apps Script backend.

2. Key Features
Dynamic Candidate Checklists: Select a candidate from a dropdown to view and update their specific onboarding progress.

Real-Time Progress Tracking: A visual progress bar provides immediate feedback on a candidate's completion percentage.

Add New Candidates: Easily create a new checklist for a new candidate directly from the user interface.

Interactive Dashboard: Switch to a dashboard view to see the progress of all candidates at a glance.

Top Performers Ranking: The dashboard automatically highlights the top 3 candidates based on their progress, making it easy to identify those who are excelling.

Filter Inactive Candidates: Option to hide candidates who have not yet started the onboarding process to focus on active ones.

Responsive Design: The interface is optimized for both desktop and mobile devices.

Serverless Backend: No need for a traditional server; the application runs using Google Sheets and Google Apps Script.

3. How It Works
The application consists of two main parts:

Frontend (index.html): A single HTML file that renders the user interface. It fetches data from and sends updates to a Google Apps Script URL.

Backend (Google Apps Script): A simple script connected to a Google Sheet acts as a serverless API. It handles requests to:

read: Fetch all candidate data.

create: Add a new candidate (a new row in the sheet).

update: Modify the status of a specific task for a candidate.

4. Setup Instructions
To get the application running, you need to set up the Google Sheet, deploy the Google Apps Script, and configure the HTML file.

Step 1: Create the Google Sheet
Create a new Google Sheet in your Google Drive.

The first row of your sheet must be a header row.

The first column header must be Candidate Name.

Every subsequent column header must exactly match the id attribute of a checkbox input tag in the index.html file.

Example Sheet Structure:

Candidate Name

task1-1

task1-2

task1-3

...

final-5

John Doe

TRUE

FALSE

TRUE

...

FALSE

Jane Smith

TRUE

TRUE

TRUE

...

TRUE

Step 2: Create and Deploy the Google Apps Script
Open your Google Sheet and go to Extensions > Apps Script.

Delete any placeholder code in the Code.gs file and paste the following script:

// Google Apps Script for the Checklist Backend

const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();

function doGet(e) {
  const action = e.parameter.action;

  if (action == 'read') {
    return readData(e);
  }

  return ContentService.createTextOutput("Invalid action").setMimeType(ContentService.MimeType.TEXT);
}

function doPost(e) {
  const action = e.parameter.action;

  if (action == 'create') {
    return createData(e);
  }

  if (action == 'update') {
    return updateData(e);
  }
  
  return ContentService.createTextOutput("Invalid action").setMimeType(ContentService.MimeType.TEXT);
}

function readData(e) {
  const headers = sheet.getRange(1, 1, 1, sheet.getLastColumn()).getValues()[0];
  const data = sheet.getRange(2, 1, sheet.getLastRow() - 1, sheet.getLastColumn()).getValues();

  let formattedData = {};

  data.forEach(row => {
    let candidateName = row[0];
    if (candidateName) {
      formattedData[candidateName] = {};
      headers.forEach((header, index) => {
        if (index > 0) { // Skip candidate name
          formattedData[candidateName][header] = row[index];
        }
      });
    }
  });

  return ContentService.createTextOutput(JSON.stringify(formattedData)).setMimeType(ContentService.MimeType.JSON);
}

function createData(e) {
  const candidateName = e.parameter.candidateName;
  if (!candidateName) {
    return ContentService.createTextOutput("Error: Candidate name is required.").setMimeType(ContentService.MimeType.TEXT);
  }
  
  sheet.appendRow([candidateName]);
  return ContentService.createTextOutput("Success").setMimeType(ContentService.MimeType.TEXT);
}

function updateData(e) {
  const candidateName = e.parameter.candidateName;
  const taskId = e.parameter.taskId;
  const status = e.parameter.status === 'true'; // Convert string 'true'/'false' to boolean

  const headers = sheet.getRange(1, 1, 1, sheet.getLastColumn()).getValues()[0];
  const candidateNames = sheet.getRange(2, 1, sheet.getLastRow() - 1, 1).getValues();

  const rowIndex = candidateNames.findIndex(row => row[0] == candidateName) + 2; // +2 for 1-based index and header
  const colIndex = headers.indexOf(taskId) + 1; // +1 for 1-based index

  if (rowIndex > 1 && colIndex > 0) {
    sheet.getRange(rowIndex, colIndex).setValue(status);
    return ContentService.createTextOutput("Success").setMimeType(ContentService.MimeType.TEXT);
  }

  return ContentService.createTextOutput("Error: Could not find candidate or task.").setMimeType(ContentService.MimeType.TEXT);
}

// NOTE: The doGet and doPost functions have been simplified for this context.
// In a real application, you should use doPost for actions that modify data (create, update)
// and pass parameters in the request body for better security and handling of larger data.
// For this project, using URL parameters keeps the client-side JavaScript simple.

Save the script.

Click the Deploy button in the top-right corner and select New deployment.

Click the gear icon next to "Select type" and choose Web app.

In the configuration settings:

Description: (Optional) Add a description like "Recruiter Checklist API".

Execute as: Me.

Who has access: Anyone. (Important: This makes your sheet data publicly accessible via the URL. For internal use, you might restrict it to Anyone within [Your Organization]).

Click Deploy.

Authorize the script to access your Google Sheets data when prompted.

After deployment, copy the Web app URL. You will need it in the next step.

Step 3: Configure the HTML File
Open the index.html file.

Find the JavaScript constant named GOOGLE_SHEETS_API_URL.

Paste the Web app URL you copied from the Google Apps Script deployment as the value for this constant.

// ... inside the <script> tag in index.html

// PASTE YOUR GOOGLE APPS SCRIPT URL HERE
const GOOGLE_SHEETS_API_URL = 'YOUR_DEPLOYED_WEB_APP_URL_HERE';

// ... rest of the script

Save the file.

5. Usage
After completing the setup, simply open the index.html file in any modern web browser.

Use the "Create a New Candidate" section to add a new person to the tracker.

Select a candidate from the dropdown to view their checklist.

Check or uncheck tasks to update their progress. All changes are saved automatically.

Click the "Dashboard" button to see an overview of everyone's progress.

6. Customization
To add, remove, or change onboarding tasks:

Edit index.html: Modify the <section> blocks inside the <main id="checklist-container">. Ensure each checkbox input has a unique id.

Update Google Sheet: Add, remove, or rename the column headers in your Google Sheet to exactly match the new id attributes from the HTML. The order of columns (after Candidate Name) does not matter.
