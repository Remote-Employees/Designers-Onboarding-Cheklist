Recruiter Onboarding Checklist
A beautiful, interactive, and self-contained web application for tracking onboarding tasks for new recruiters. This single-page application is designed to be simple, efficient, and visually pleasing, with no backend or database required.

✨ Key Features
Interactive Checklist: Easily check and uncheck tasks with satisfying custom animations.

Multi-Candidate Tracking: Select different candidates from a dropdown menu to view and manage their individual progress.

Automatic Progress Saving: All progress is automatically saved in the browser's localStorage. The checklist will remember the state for each candidate even after closing the browser tab.

Dynamic Progress Bar: A sleek progress bar provides an immediate visual representation of task completion for the selected candidate.

Modern & Beautiful UI: A clean, responsive interface built with Tailwind CSS, featuring a modern color palette, custom checkboxes, and smooth transitions.

Zero Dependencies: It's a single index.html file. There's no need for npm install, build steps, or a web server. It just works.

Easy to Deploy: Perfect for hosting on static site providers like GitHub Pages.

🚀 How to Use
Download: Get the index.html file.

Open: Open the file in any modern web browser (like Chrome, Firefox, or Edge).

Track: Select a candidate from the dropdown and start checking off their completed tasks. The progress will be saved automatically.

🔧 Tech Stack
HTML5: For the core structure and content.

Tailwind CSS: For all styling and creating the responsive, modern UI.

JavaScript (ES6): For all the application logic, including progress tracking and saving data to localStorage.

⚙️ How It Works
The application's state management is handled entirely on the client-side using JavaScript and localStorage.

When you check or uncheck a task, a JavaScript function is triggered.

This function calculates the current progress and updates the progress bar.

It then saves the state of all checkboxes for the currently selected candidate into a JSON object in localStorage.

Each candidate has a unique key in localStorage (e.g., recruiterChecklist_Sofiia Lukianchuk), ensuring that progress is saved independently.

When you switch between candidates, the application loads the saved state from localStorage and updates the checkboxes accordingly.

🌐 Deploying to GitHub Pages
You can easily host this checklist for free on GitHub Pages.

Create a new GitHub Repository.

Upload the index.html file to the repository.

In your repository, go to Settings > Pages.

Under the "Build and deployment" section, select Deploy from a branch.

Choose the main (or master) branch as your source and click Save.

Wait a few minutes for the site to be built. Your live checklist will be available at https://your-username.github.io/your-repository-name/.

✏️ Customization
Editing the checklist is straightforward. Open the index.html file in a text editor.

To change the list of candidates:
Find the candidates array in the <script> tag at the bottom of the file and edit the names:

const candidates = [
    "Kabanets Serafyma",
    "Sofiia Lukianchuk",
    "Polina Bohun",
    // Add new candidates here
];

To change the tasks:
The tasks are located in the <main> section of the HTML. You can edit the text directly. Each task follows this structure:

<div class="checklist-item">
    <input type="checkbox" id="task1-1">
    <label for="task1-1">
        <span class="checkbox-custom"></span>
        <span class="checkbox-label-text text-slate-700">Your task description goes here.</span>
    </label>
</div>

To add a new task, simply copy and paste this block, making sure to give the input and the label's for attribute a new, unique id.

📜 License
This project is open-source and available under the MIT License.
