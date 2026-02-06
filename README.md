ClimbLog 🧗‍♂️

Online Climbing Coach – By a climber, for a climber.

ClimbLog is a free, open-source Single Page Application (SPA) designed for planning, monitoring, and analyzing climbing workouts. The project focuses on simplicity and accessibility – it requires no complex installation, and the entire code resides in a single file, making it incredibly easy to modify and host.

🌟 Key Features

Workout Planning: Schedule training sessions by date and location (e.g., gym names).

Interactive Timer: Built-in timer handling complex intervals (work time, rest between reps, rest between sets) with audio cues.

Exercise Database: Editable library of exercises with customizable default parameters (sets, reps, load).

Progress Tracking: History of completed workouts and analytics for specific exercises.

Coach Notes: Dedicated space for training goals and feedback.

Cloud Sync: Integrated with Google Firebase to keep your data safe and accessible across devices.

🛠 Tech Stack

The application uses a modern "No-Build" approach, meaning it runs without compilers, Node.js, or complex development environments.

Frontend: HTML5 + React 18 (loaded via CDN).

Styling: Tailwind CSS (loaded via CDN).

On-the-fly Compilation: Babel (Standalone) – compiles JSX directly in the browser.

Backend (BaaS): Google Firebase (Firestore Database + Authentication).

Audio: Web Audio API (generates timer sounds without external MP3 files).

🚀 How to Run (Installation & Hosting)

Since the app is just a single index.html file, "installation" is trivial.

Option 1: Local Run

Download the index.html file.

Open it in any modern web browser.

Done! (Remember to configure Firebase as described below).

Option 2: Free Hosting (Recommended)

To access the app from your phone at the gym, host it on a free static file hosting service.

GitHub Pages: Upload the file to a repository and enable Pages in settings.

Render / Netlify / Vercel: Simply drag & drop the file or connect your Git repository.

Own Server: Upload via FTP.

🔥 Database Configuration (Firebase)

To save your workouts, you need to create a free Google Firebase project. It takes about 3 minutes.

Step 1: Create a Project

Go to Firebase Console.

Click "Add project".

Name your project (e.g., ClimbLog-YourName).

You can disable Google Analytics.

Click Create project.

Step 2: Enable Database (Firestore)

In the left panel, go to Build -> Firestore Database.

Click Create database.

Select a location (e.g., eur3 - Europe West).

Select Start in production mode.

Step 3: Enable Authentication

The app uses "Anonymous Authentication" to secure data against unauthorized external access.

In the left panel, go to Build -> Authentication.

Click Get started.

Select Anonymous from the Sign-in method list.

Toggle Enable and click Save.

Step 4: Get API Keys

Click the gear icon (Project settings) next to "Project Overview".

Under "Your apps", click the Web icon (</>).

Register the app (e.g., ClimbLog Web).

You will see a configuration object (const firebaseConfig = { ... }).

Copy the values (apiKey, authDomain, projectId, etc.) and paste them into the index.html file inside the userConfig object (around line 67).

🔒 Security Rules

This is a crucial step to prevent unauthorized users from deleting your data.

In the Firebase Console, go to Firestore Database -> Rules.

Delete existing rules and paste the following code:

rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {

    // Helper function to check if user is signed in (app does this automatically)
    function isSignedIn() {
      return request.auth != null;
    }

    // Allow access to data only for signed-in app users
    match /artifacts/{appId}/public/data/{document=**} {
      allow read, write: if isSignedIn();
    }
    
    // Block access to everything else
    match /{document=**} {
      allow read, write: if false;
    }
  }
}


Click Publish.

Additional Security (Optional)

In the Google Cloud Console, you can edit your Browser key and add Website restrictions (e.g., your-site.netlify.app). This ensures your API key cannot be used on other domains.

📄 License

This project is licensed under the MIT License. Feel free to modify, use, and share it with friends.
Happy climbing! 💪
