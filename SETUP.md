# Flatplan — setup

This is one self-contained file (`index.html`) plus a rules file (`firestore.rules`) you paste into the Firebase console once. Nobody touches config after that — operators just open the link and sign in.

Budget about 20 minutes the first time. You only do this section once per magazine (not once per issue).

## 1. Create the Firebase project

1. Go to https://console.firebase.google.com and click **Add project**.
2. Name it anything (e.g. "flatplan"). You can decline Google Analytics — not needed.
3. Once it's created, click the **</> (web)** icon on the project overview page to register a web app. Give it any nickname. You do **not** need Firebase Hosting — you're deploying via GitHub Pages instead.
4. Firebase will show you a `firebaseConfig` object like this:

   ```js
   const firebaseConfig = {
     apiKey: "AIza...",
     authDomain: "flatplan-xxxxx.firebaseapp.com",
     projectId: "flatplan-xxxxx",
     storageBucket: "flatplan-xxxxx.appspot.com",
     messagingSenderId: "...",
     appId: "..."
   };
   ```

   Copy this whole block and paste it over the placeholder `firebaseConfig` near the top of the `<script type="module">` section in `index.html` (search for `PASTE_YOUR_API_KEY`). This key is meant to be public in client-side code — it identifies your project, it doesn't grant access by itself. Access is controlled by the security rules in step 3.

## 2. Turn on email-link sign-in

1. In the Firebase console, go to **Build → Authentication → Get started**.
2. Under **Sign-in method**, enable **Email/Password**, then toggle on **Email link (passwordless sign-in)** underneath it, and save.
3. Still in Authentication, go to **Settings → Authorized domains** and add the domain your flatplan will live at, e.g. `yourname.github.io`. (`localhost` is already there by default, useful for testing before you publish.)

## 3. Create the database and paste the security rules

1. Go to **Build → Firestore Database → Create database**. Choose **Production mode** and pick any region close to you.
2. Once created, go to the **Rules** tab and replace the default contents with everything in `firestore.rules` from this folder, then click **Publish**.
3. Check that the admin email inside `firestore.rules` (`stewart.hawkins@forbes.com.au`) matches the `ADMIN_EMAIL` constant near the top of `index.html`'s script. They must be identical, lowercase, or the admin account will get locked out of its own team-management panel.

## 4. Publish to GitHub Pages

Same process as your other single-file apps: create a repo (or a folder in an existing one), add `index.html` through the GitHub web interface, and turn on GitHub Pages for that repo in **Settings → Pages**. Once it's live, open the published URL — that's the address you add to Authorized domains in step 2.3 if you haven't already.

## 5. Sign in as admin and invite the team

1. Open the published site. Enter **your own email** (must match `ADMIN_EMAIL` exactly) and request a sign-in link.
2. Click the link from your email — you'll land back in the app, be asked to set a display name, and then see the empty flatplan screen.
3. Click **Team** in the header and add each operator's email address. They don't need a Google account or any software — just that email.
4. Click **+ New issue**, give it a name and total page count, and you're in.

Each operator then just visits the same link, enters their own email, clicks the link that arrives, sets their display name once, and they're editing — no InDesign, no license, no install.

## Notes on cost and limits

- Everything here runs on Firebase's free **Spark** plan. A small team doing this a few times a month will not come close to the free daily limits (50,000 document reads / 20,000 writes per day), and unlike some competitors, the free tier doesn't pause your project during quiet weeks between issues.
- If you ever outgrow the free tier, Firebase prompts you to upgrade rather than silently failing — nothing breaks unexpectedly.
- If a script fails to load after Firebase updates their SDK (unlikely to happen suddenly, but just in case): go to https://firebase.google.com/docs/web/setup, copy the `<script type="module">` import lines shown there, and swap them for the three `import` lines at the top of `index.html`'s script block. Nothing else in the file needs to change.

## What's deliberately left for v2

Per-page comments, photo/proof thumbnail uploads, PDF export of the whole flatplan, and cloning an issue from a previous one as a starting template. The current version covers the core problem — one live, shared grid everyone can see and edit — without those extras.
