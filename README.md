# Vue Firebase Authentication App

A simple Vue 3 application that demonstrates Firebase Authentication and Firestore integration. This application provides a login interface and displays documents from a Firestore collection after successful authentication.

## Features

- User authentication using Firebase Email/Password
- Secure login/logout functionality
- Display of Firestore documents after authentication
- Responsive design
- Environment variable configuration for Firebase credentials

## Prerequisites

- Node.js (version 20 or higher)
- npm (comes with Node.js)
- A Firebase project with:
  - Authentication enabled (Email/Password method)
  - Firestore Database created
  - A 'documents' collection in Firestore

## Setup

1. Clone the repository
2. Install dependencies:
   ```bash
   npm install
   ```

3. Create a `.env` file in the root directory with your Firebase configuration:
   ```
   VITE_FIREBASE_API_KEY=your_api_key
   VITE_FIREBASE_AUTH_DOMAIN=your_auth_domain
   VITE_FIREBASE_PROJECT_ID=your_project_id
   VITE_FIREBASE_STORAGE_BUCKET=your_storage_bucket
   VITE_FIREBASE_MESSAGING_SENDER_ID=your_messaging_sender_id
   VITE_FIREBASE_APP_ID=your_app_id
   ```

   You can find these values in your Firebase Console:
   1. Go to Project Settings (gear icon)
   2. Scroll down to "Your apps"
   3. Under the web app configuration, you'll find all these values

4. Set up Firestore Security Rules:
   ```javascript
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /documents/{document=**} {
         allow read: if request.auth != null;
         allow write: if false;
       }
     }
   }
   ```

## Running the Application

Development mode:
```bash
npm run dev
```

Build for production:
```bash
npm run build
```

Preview production build:
```bash
npm run preview
```

## Application Structure

- `src/App.vue` - Main application component with login form and document display
- `src/firebase.js` - Firebase configuration and initialization
- `src/main.js` - Vue application entry point

## Security Notes

- Never commit your `.env` file to version control
- Keep your Firebase configuration values secure
- The application uses environment variables to protect sensitive information
- All Firebase credentials are loaded at runtime from environment variables

## Firebase Setup Steps

1. Create a new Firebase project at [Firebase Console](https://console.firebase.google.com)
2. Enable Authentication:
   - Go to Authentication > Sign-in method
   - Enable Email/Password authentication
3. Set up Firestore:
   - Go to Firestore Database
   - Create a database if you haven't already
   - Create a 'documents' collection
   - Add some test documents with a 'title' field
4. Configure Security Rules:
   - Go to Firestore Database > Rules
   - Apply the security rules provided above

## Troubleshooting

If you encounter the "Missing or insufficient permissions" error:
1. Verify that you're properly logged in
2. Check that the Firestore security rules are correctly configured
3. Ensure your Firebase configuration in the `.env` file is correct
4. Verify that the 'documents' collection exists in your Firestore database
