# ClimbLog 🧗‍♂️

**Online Climbing Coach – By a climber, for a climber.**

ClimbLog is a free, open-source Single Page Application (SPA) for planning and analyzing climbing workouts. Built for simplicity, it requires no complex installation; the entire code resides in a single file, making modification and hosting effortless.

## 🌟 Key Features

* **Workout Planning:** Schedule sessions by date and location.
* **Interactive Timer:** Built-in interval timer handling work/rest phases with audio cues.
* **Exercise Database:** Editable library with customizable defaults (sets, reps, load).
* **Progress Tracking:** Workout history and exercise-specific analytics.
* **Coach Notes:** Dedicated space for goals and feedback.
* **Cloud Sync:** Firebase integration for secure, cross-device data access.

## 🛠 Tech Stack

Uses a modern "No-Build" approach (no Node.js required):

* **Frontend:** HTML5 + React 18 (via CDN).
* **Styling:** Tailwind CSS (via CDN).
* **Compilation:** Babel Standalone for in-browser JSX.
* **Backend:** Google Firebase (Firestore + Authentication).
* **Audio:** Web Audio API (no external audio files).

## 🚀 How to Run

Since the app is a single `index.html` file, installation is trivial.

### Option 1: Local Run

Download `index.html` and open it in any modern web browser.

### Option 2: Free Hosting (Recommended)

For mobile access, host the file on a static service like **GitHub Pages**, **Render**, **Netlify**, or via FTP on your own server.

## 🔥 Database Configuration (Firebase)

Create a free Google Firebase project to save your data (approx. 3 mins).

1. **Create Project:** Go to [Firebase Console](https://console.firebase.google.com/) and create a new project.
2. **Enable Database:** Go to **Build -> Firestore Database**, create a database (e.g., in `eur3`), and select **Start in production mode**.
3. **Enable Auth:** Go to **Build -> Authentication**, select **Get started**, enable **Anonymous** sign-in, and save.
4. **Get API Keys:** In Project Settings (gear icon), register a **Web** app. Copy the values from the `firebaseConfig` object and paste them into `index.html` inside the `userConfig` object (around line 67).

## 🔒 Security Rules

To prevent unauthorized data access, go to **Firestore Database -> Rules**, replace the existing code with the following, and click **Publish**:

```javascript
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    // Helper check for signed-in users (app handles this automatically)
    function isSignedIn() { return request.auth != null; }

    // Allow access only to signed-in app users
    match /artifacts/{appId}/public/data/{document=**} {
      allow read, write: if isSignedIn();
    }
    
    // Block all other access
    match /{document=**} {
      allow read, write: if false;
    }
  }
}
