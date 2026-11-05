# Gwyn's Brain Lab — private family web app

This is an actual mobile-responsive app, **not a spreadsheet**. It has interactive 1–5 ratings, optional reflections, conditional side-effect severity questions, medication/food/sleep timing, independent Gwyn/Mum/Dad/Teacher logins, Firestore-backed persistence, and comparative graphs. It can be hosted free on Firebase Hosting (within free-tier limits).

## Important current status

The app source is built, but **it is not hosted or connected to a database yet**. A parent must complete the setup below. The Firebase project must be controlled by a parent, and access should not be given to teachers until the parent has checked the sharing rules and obtained appropriate consent. By design, teachers see only their own entries; Gwyn and both parents can see family entries. Gwyn's optional private note is visible to approved parents, not teachers. This is **not an end-to-end encrypted messaging app**.

## Setup (approximately 20–40 minutes)

1. At https://console.firebase.google.com create a project controlled by a parent. Review data processing, data residency and privacy settings before entering any child's health information.
2. Under **Authentication > Sign-in method**, enable **Email/Password**. Create a **Web App** in project settings and copy its Firebase configuration JSON.
3. Under **Firestore Database**, create a database and publish the contents of `firestore.rules` as its security rules. **Do not leave Firestore in test mode.**
4. Under **Hosting**, follow Firebase's instructions to host the `index.html` and `app.js` files in the same public directory. You can use the Firebase CLI (`npm install -g firebase-tools`, `firebase login`, `firebase init hosting`, `firebase deploy --only hosting`) or a hosting provider that serves these two files over HTTPS. The `firestore.rules` file is not a web asset.
5. Open the deployed URL. Paste the Firebase configuration JSON into the first screen and save it. Each device needs this configuration once. (Alternatively, preconfigure the app for deployment by editing its initialization code.)
6. Each person creates their own account. The app will display their Firebase UID while awaiting approval. In **Firestore > profiles**, create a document whose document ID is that UID, with field `role` set to one of `gwyn`, `mum`, `dad`, or `teacher`. This is intentionally a manual parent-controlled approval step. Sign out and back in if needed.
7. **Only then** begin entering health information. Pin the hosted link in your family WhatsApp group. Create recurring 07:30 and 19:30 device reminders. The app does not automatically send WhatsApp group messages.

## Usage

- Gwyn: daily focus, emotions, self-perception, medication dose/time, breakfast/meal timing, perceived onset/offset, sleep, conditional side-effect severity and free text.
- Parents: independent daily observations, routines, prompts and symptoms.
- Teachers: their own school observations; they cannot view family health entries.
- Graphs: show dated scores separately for Gwyn, Mum and Dad; no invented zero scores for missing entries.
- Each person's entry for a date can be updated by saving again on that date. Historical records remain stored after medication changes.

## Limits and safety

- This is an observational family tracker, not a prescribing or diagnostic system. Medication brand/formulation and food instructions must be confirmed with the pharmacist/prescriber. Don't change dose or meal timing based on graphs.
- The app uses Firebase Authentication and Firestore; persistent records are shared across devices only after Firebase is configured and deployed.
- Parents can see all family entries, including Gwyn's optional private reflections. Explain that to Gwyn before he uses it.
- Teachers can see only their own entries. Parents should avoid granting teacher access to the family account.
- The first release does not include push notifications, WhatsApp Business API integration, charts by dose phase, automatic weight/growth interpretation, offline entry, or export-to-PDF. Those are not represented as complete.
- The symptom severity scales are for recording experience; they are not a substitute for seeking medical help for significant adverse effects.
