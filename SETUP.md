# Setup — one-time console steps

The code is done. These are the steps only you can do (they need your own Google/GitHub login), in order.

## 1. Create a Firebase project
1. Go to https://console.firebase.google.com → **Add project**.
2. Name it anything (e.g. `kaushik-taskboard`). Google Analytics is not needed — you can disable it.

## 2. Enable Firestore
1. In the left sidebar: **Build → Firestore Database → Create database**.
2. Choose **Production mode** (not test mode — the security rules below handle access).
3. Pick any nearby region, then **Enable**.

## 3. Enable Google sign-in
1. Left sidebar: **Build → Authentication → Get started**.
2. Under **Sign-in method**, enable **Google**.
3. Set a support email (your own) and save.

## 4. Register the web app and get your config
1. Project overview (gear icon) → **Project settings → General**.
2. Scroll to **Your apps → Add app → Web** (the `</>` icon).
3. Give it a nickname (e.g. "taskboard"), skip Firebase Hosting (we're using GitHub Pages).
4. Copy the `firebaseConfig` object it shows you.
5. Open `index.html` in this repo, find the `firebaseConfig` block near the top, and paste your real values over the `"REPLACE_ME"` placeholders.

## 5. Publish the security rules
1. Firestore Database → **Rules** tab.
2. Replace the contents with what's in `firestore.rules` in this repo (it restricts all read/write to `kaushik@bidgely.com` only).
3. Click **Publish**.

## 6. Push this repo to GitHub and enable Pages
(Claude will do the `git push` part — this is what happens after.)
1. On GitHub, go to the repo → **Settings → Pages**.
2. Under **Build and deployment → Source**, choose **Deploy from a branch**.
3. Branch: `main`, folder: `/ (root)`. Save.
4. GitHub will give you a URL like `https://kaushik-bidgely.github.io/taskboard/` — that's your live board.

## 7. Authorize the Pages domain in Firebase
Firebase blocks sign-in popups from domains it doesn't recognize.
1. Firebase console → **Authentication → Settings → Authorized domains**.
2. Add your GitHub Pages domain, e.g. `kaushik-bidgely.github.io`.

## 8. Test it
Open the GitHub Pages URL, click **Sign in with Google**, use your `kaushik@bidgely.com` account. You should land on an empty board — then ask Claude to backfill your current categories/tasks (it can do this directly via a Firebase Admin script, see below), or re-create them by hand.

---

## Optional — letting Claude write to this board directly

To let a Claude Code session read/write this board the same way it did on the artifact (e.g. to propose tasks into the `suggestions` collection), you need a service account key:

1. Firebase console → **Project settings → Service accounts**.
2. Click **Generate new private key** → downloads a `.json` file.
3. Save it somewhere OUTSIDE this repo (e.g. `~/.secrets/taskboard-service-account.json`) — never commit it. `.gitignore` here already blocks the common filename patterns as a backstop, but don't rely on that.
4. Tell Claude the file's path when you want it to script an update — it'll use the `firebase-admin` Node package to read/write Firestore directly, the same way it used `ArtifactData` before.

This step is optional and can wait until you actually want Claude to push tasks into the board.
