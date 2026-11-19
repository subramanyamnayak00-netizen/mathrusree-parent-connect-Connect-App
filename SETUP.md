# Mathrusree Study Circle – Parent Connect

This is a responsive Progressive Web App starter with a sample-data demo and a Firebase-backed live mode.

## Included
- Parent login and admin login using Firebase Authentication
- Parent dashboard: attendance, results, homework, notices, gallery and fee status
- Admin quick actions to add notices, homework, results, attendance, fees and gallery links
- Firestore security rules separating admin records from parent records
- Installable PWA shell and free Firebase Hosting deployment instructions

## 1. Preview the demo
Because browsers restrict JavaScript modules when opening a file directly, run a local static server:
- On a computer with Python: open this folder in a terminal and run `python -m http.server 8000`
- Visit `http://localhost:8000`
- Tap **Explore demo**. Demo changes are temporary and stay only in the current page session.

You can also deploy the files to Firebase Hosting and use the demo without connecting Firebase.

## 2. Create a Firebase project
1. Go to https://console.firebase.google.com/ and create a project.
2. Add a Web app in Project settings and copy its configuration.
3. Paste the values into `firebase-config.js`, replacing every `PASTE_...` value.
4. In Authentication > Sign-in method, enable **Email/Password**.
5. In Firestore Database, create a database.
6. Open Firestore Rules and publish the contents of `firestore.rules`.

Do not publish real student data until the institute has approved the system and the security rules have been tested.

## 3. Create accounts and link a student
For the first admin:
1. In Firebase Authentication, add an admin user with an email and strong password.
2. In Firestore, create a document in `users` whose document ID is exactly that user's Authentication UID.
3. Set fields: `name` (string), `role` (string `"admin"`).

For each parent:
1. Create the parent's account in Firebase Authentication.
2. Create `users/{parent-uid}` with fields `name` (string), `role` (string `"parent"`), `studentIds` (array of student document IDs).
3. In `students`, create a student document with `name`, `className`, `section`, `roll`, and `parentUid` (the parent's Authentication UID).
4. For each record in `attendance`, `results`, `homework`, and `fees`, include `studentId` and `parentUid` matching that student and parent.

Only admins can create or edit student records. Parents should be given their own sign-in details privately.

## 4. Deploy with a free Firebase Hosting subdomain
1. Install Node.js and Firebase CLI on a computer: `npm install -g firebase-tools`
2. In this folder run `firebase login`
3. Run `firebase init hosting`, choose the project, use `.` as the public directory, configure as a single-page app: **No**.
4. If asked to overwrite `index.html`, choose **No**.
5. Run `firebase deploy --only hosting`.
6. Firebase will show your live `https://PROJECT_ID.web.app` address.

Firebase's no-cost Spark plan has quotas and limits. Check current pricing and product availability before relying on it for an institute's ongoing service: https://firebase.google.com/pricing

## Important limitations in this starter
- The app currently displays the first student linked through the records returned to the parent account. Multi-child switching, PDF report cards, push notifications, fee payment gateway, and native Play Store packaging are not included yet.
- The gallery accepts image URLs; it does not upload files to cloud storage.
- The language button is a placeholder toggle; complete Telugu translations still need to be added.
- Admin quick actions add records for the first linked student. Student creation, parent invitation workflow, editing/deleting records, analytics, and bulk attendance are future enhancements.
- Firebase Authentication and Firestore are used for the live app. Do not put service-account keys or private credentials in browser code.
- The demo is for preview only. It is not a secure production database and must not be used for actual student records.
