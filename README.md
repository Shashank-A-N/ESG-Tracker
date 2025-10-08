# ESG & Timesheet Dashboard

A comprehensive, single-page web application designed to help businesses and individuals track Environmental, Social, and Governance (ESG) metrics alongside efficient employee time management. The application features a real-time dashboard with AI-powered insights, data entry forms, goal tracking, and robust user settings, all powered by a Firebase backend.

---

## Key Features

### 1. Authentication & User Management
- **Full Auth Suite:** Secure user authentication with email/password, including sign-up, sign-in, and password reset functionality.
- **Anonymous Access:** Users can start using the application immediately with an anonymous account.
- **Account Linking:** Seamlessly converts an anonymous session into a permanent account, preserving all entered data.

### 2. ESG Command Center (Dashboard)
- **At-a-Glance KPIs:** Key Performance Indicator (KPI) cards for Overall ESG Score, Energy Consumption, Waste Recycled, and Social Compliance, complete with trend analysis against the previous quarter.
- **AI-Powered Insights:** A "Get AI Insights" feature that uses the Google Gemini API to analyze the latest data and provide actionable recommendations.
- **Interactive Charts:**
    - **Performance Trends:** A line chart visualizing Environmental, Social, and Governance metrics over time.
    - **Metric Breakdown:** A bar chart showing the score for each individual ESG metric for the latest month.
    - **Industry Benchmark:** A radar chart comparing the user's ESG scores against industry averages.
- **Goal Progress:** A dedicated module on the dashboard to track progress towards the top three most important ESG goals.
- **Dynamic Content:** Widgets for Recent Activity and the latest ESG News & Regulatory Updates.

### 3. Data Management & Tracking
- **ESG Data Entry:** A clean form to log monthly data across Environmental (Energy, Waste), Social (Diversity, Safety), and Governance (Compliance) categories.
- **Goal Setting:** A dedicated section to create, view, and delete specific, measurable ESG goals with target values and deadlines.
- **Document Hub:** Functionality to upload and manage important documents like annual reports, certifications, or policies.

### 4. Timesheet & Reporting
- **Real-Time Clock:** A digital clock displaying the current time and date.
- **Clock In / Clock Out:** Simple one-click functionality to start and end work sessions.
- **Interactive Calendar:** A full monthly calendar view that highlights days with logged work and allows users to select a date to view its summary.
- **Daily Summary:** A detailed breakdown of all work sessions for a selected date, including total duration.
- **Timesheet Report:** A paginated table of all historical time logs, with the ability to add notes to entries.
- **CSV Export:** Download the complete timesheet report as a CSV file for offline analysis or payroll processing.

### 5. Advanced Settings & Personalization
- **Modular Settings Page:** A responsive settings section with a sidebar that collapses to icons on smaller screens.
- **Profile Management:** Users can update their public profile information (name, bio).
- **Security:**
    - **Password Management:** A secure form to change the account password, complete with a real-time password strength meter and validation criteria.
    - **Danger Zone:** A protected area allowing users to permanently clear their Timesheet data, ESG data, or all application data.
- **Team Management:** Invite new members to the workspace via email and assign them roles (e.g., Contributor, Viewer).
- **Notification Preferences:** Toggles to manage subscriptions to email notifications like a "Weekly ESG Digest" and "Regulatory Updates."
- **Appearance:** A theme selector to instantly switch between a **Light** and **Dark** mode interface.

---

## Tech Stack

- **Frontend:**
    - **HTML5**
    - **Tailwind CSS:** For utility-first styling and responsive design.
    - **JavaScript (ES6 Modules):** For all application logic.
    - **Chart.js:** For creating interactive and beautiful charts.
    - **Font Awesome:** For iconography.

- **Backend & Database:**
    - **Firebase:**
        - **Authentication:** Manages user sign-up, login, and sessions.
        - **Firestore:** A NoSQL database used to persistently store all user-specific data, including time logs, ESG records, goals, settings, and documents.

- **Services:**
    - **Google Gemini API:** Powers the AI-driven insights feature on the dashboard.

---

## Setup & Installation

This application is a self-contained `index.html` file that connects to a Firebase backend. To run it yourself, follow these steps:

1.  **Create a Firebase Project:** Go to the [Firebase Console](https://console.firebase.google.com/) and create a new project.
2.  **Enable Services:**
    - In your new project, enable **Firestore Database**.
    - Go to the **Authentication** section and enable the **Email/Password** and **Anonymous** sign-in methods.
3.  **Get Firebase Config:** In your project settings, find and copy your web app's Firebase configuration object.
4.  **Update `index.html`:**
    - Open the `index.html` file and locate the `firebaseConfig` object within the `<script>` tag.
    - Replace the placeholder configuration with your own project's configuration keys.
5.  **Get Gemini API Key:**
    - Go to the [Google AI Studio](https://aistudio.google.com/app/apikey) to get an API key for the Gemini API.
    - In `index.html`, find the `getAiInsights` function and replace the placeholder API key with your own.
6.  **Run the Application:** Simply open the `index.html` file in any modern web browser.

## Technical Architecture & Data Model

### Architecture
The application is a pure **Single-Page Application (SPA)** contained within a single `index.html` file.

1.  **Client-Side Logic:** All application logic, including UI rendering, state management, and business rules, is handled by vanilla JavaScript (ES6 Modules) running directly in the browser.
2.  **Backend-as-a-Service (BaaS):** Google Firebase serves as the complete backend.
    -   The client communicates directly and securely with Firebase services.
    -   **Firebase Authentication** handles all user identity operations.
    -   **Firestore** acts as the real-time NoSQL database, with data automatically syncing to the client whenever it changes.
3.  **Third-Party API:** The AI Insights feature makes a direct, client-side REST API call to the **Google Gemini API**.

### Firestore Data Model
All user data is stored within a main `users` collection, nested under the user's unique UID provided by Firebase Auth.

```json
/artifacts/{appId}/users/{userId}/
  |
  |-- timeLogs/
  |   |-- {timeLogId}:
  |       |-- clockIn: <Timestamp>
  |       |-- clockOut: <Timestamp> (or null)
  |       |-- note: <String>
  |
  |-- esgData/
  |   |-- {esgDataId}:
  |       |-- month: "YYYY-MM"
  |       |-- energy: <Number>
  |       |-- waste: <Number>
  |       |-- diversity: <Number>
  |       |-- safety: <Number>
  |       |-- compliance: <Number>
  |       |-- submittedAt: <ServerTimestamp>
  |
  |-- esgGoals/
  |   |-- {goalId}:
  |       |-- metric: <String> ("energy", "waste", etc.)
  |       |-- target: <Number>
  |       |-- deadline: "YYYY-MM"
  |
  |-- documents/
  |   |-- {docId}:
  |       |-- name: <String>
  |       |-- category: <String>
  |       |-- uploadedAt: <ServerTimestamp>
  |
  |-- teamMembers/
  |   |-- {memberId}:
  |       |-- email: <String>
  |       |-- role: <String> ("Contributor", "Viewer")
  |
  |-- settings/
      |-- user_settings:
          |-- theme: "light" or "dark"
          |-- notifications: { weeklyDigest: <Boolean>, regulatoryUpdates: <Boolean> }
          |-- profile: { firstName: <String>, lastName: <String>, bio: <String> }
