# Macro Tracker

Personal calorie & macro tracker. Static site (no build step), Firestore for
storage, Firebase Auth for sign-in, GitHub Pages for hosting. See
`calorie-tracker-spec.md` for the full spec.

## First-time setup

1. **Create the Firebase project**
   - Go to https://console.firebase.google.com → Add project.
   - In the project, go to **Build → Firestore Database → Create database**
     (start in production mode; pick any region).
   - Go to **Build → Authentication → Get started**, then enable the
     **Google** and **Email/Password** sign-in providers (Sign-in method tab).
   - Go to **Project settings → General → Your apps → Add app → Web (`</>`)**,
     register the app, and copy the `firebaseConfig` object it gives you.

2. **Paste the config into `index.html`**
   - Find the `firebaseConfig` object near the top of the `<script>` block
     and replace the `YOUR_...` placeholders with your real values.

3. **Paste the security rules**
   - In the Firebase console, go to **Firestore Database → Rules**, replace
     the contents with `firestore.rules` from this repo, and click Publish.

4. **Add your domain to the Auth allowlist** (needed for Google sign-in)
   - **Authentication → Settings → Authorized domains** → add your GitHub
     Pages domain (e.g. `<username>.github.io`) once it's live.

5. **Enable GitHub Pages**
   - Push this repo to GitHub, then in the repo's **Settings → Pages**, set
     the source to the `main` branch, root folder.

## Updating

Bump `APP_VERSION` in `index.html` on every deployed change — the app polls
its own `index.html` for a newer version and prompts users to reload.
