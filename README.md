# ABYSSBOUND — Firebase Version

This version replaces Supabase + Resend with Firebase Authentication + Cloud Firestore.

## 1. Create Firebase project
1. Open https://console.firebase.google.com/
2. Create a project.
3. Add a Web App.
4. Copy the Firebase web configuration into `config.js`.

## 2. Enable Authentication
Firebase Console → Authentication → Sign-in method → Email/Password → Enable.

This version does NOT require email verification for account creation, so registration does not depend on Resend or SMTP. You can enable verification later if you want it. Firebase supports email/password authentication on the web.

## 3. Create Firestore
Firebase Console → Firestore Database → Create database.
Use Production mode, then paste the contents of `firestore.rules` into the Rules tab and Publish. The rules allow each signed-in player to read/write only their own `players/{uid}` document.

## 4. Configure the game
Edit `config.js` and replace all `YOUR_...` values with the Web App config from Firebase Project Settings → Your apps. Firebase web API keys are intended for client apps; the database is protected by Firestore Security Rules.

## 5. Deploy to Vercel
Rename/use `index.html` as the site entry file. Upload the folder to GitHub and import the repository into Vercel, or deploy it with Vercel.

## Player data
Each player is stored at:
`players/{Firebase Auth UID}`

The document stores the existing ABYSSBOUND player state, including heroes, gold, gems, progression, quests, achievements, equipment, settings and other game data.

## Save behavior
The game keeps its existing save calls and sends them to Firestore. It also queues saves to avoid excessive writes. Important events such as purchases, progression, hero selection, quests and logout continue to call the existing save layer.

## Registration capacity
Firebase documents a 100 new-account creations/hour limit per IP by default, while registered user accounts are unlimited. Firebase also allows a temporary increase to the account-creation limit through the console for planned launches. Authentication has additional quotas, so high-volume launches should be tested and scheduled appropriately.

Firestore's current free quota is 20,000 document writes/day, 50,000 reads/day, 20,000 deletes/day and 1 GiB stored data.

Sources:
- https://firebase.google.com/docs/auth/limits
- https://firebase.google.com/docs/auth/web/password-auth
- https://firebase.google.com/docs/firestore/security/rules-conditions
- https://firebase.google.com/docs/firestore/pricing
