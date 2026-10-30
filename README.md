# Taskboard

A personal Kanban-style task tracker: categories as columns, nested subtasks, drag-and-drop, color-coded priority, Now/Next/Later timing, due dates (auto-sorted), a status update log per task, a board/list view toggle, and light/dark theme.

Originally built as a Claude Artifact, now a standalone static page backed by Firebase (Firestore + Google Auth), deployable to GitHub Pages.

- `index.html` — the whole app (HTML/CSS/JS, no build step).
- `firestore.rules` — security rules restricting all read/write to one email.
- `SETUP.md` — one-time console steps to stand this up (Firebase project, auth, GitHub Pages).

Data access is gated by Google Sign-In; only the email set in `firestore.rules` (and `ALLOWED_EMAIL` in `index.html`) can read or write. The page's source and Firebase config are not secret — Firebase apps are designed to have their client config public; real access control happens in `firestore.rules`.

See `SETUP.md` to get this running.
