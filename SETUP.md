# Us Planner: setup (about 10 minutes, all free)

A shared planner PWA for two iPhones: events, calendar (week + month), weekly work rosters, and shared lists. Changes sync live between you both and work offline.

You need two free things: **Firebase** (the sync database) and **GitHub Pages** (hosting the app).

## 1. Create the Firebase project

1. Go to https://console.firebase.google.com and sign in with Google.
2. **Add project**, name it (e.g. `us-planner`), turn Google Analytics off, **Create**.
3. **Build > Authentication > Get started > Sign-in method > Anonymous > Enable > Save**.
4. **Build > Firestore Database > Create database**. Pick a location (e.g. `australia-southeast1`), choose **Production mode**, **Create**.
5. In Firestore, open the **Rules** tab, replace everything with the rules below, and **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /spaces/{space}/items/{item} {
      allow read, write: if request.auth != null;
    }
  }
}
```

This means only people who know your exact space code can find your data (the root collection can't be listed).

6. **Project settings (gear icon) > General > Your apps > Web (`</>`)**, register an app (no hosting), and copy the `firebaseConfig` values.
7. Open `config.js` and paste each value over `PASTE_HERE`.

## 2. Host it on GitHub Pages

1. Create a new **public** repository on GitHub (e.g. `us-planner`).
2. Upload all files in this folder (`index.html`, `config.js`, `manifest.json`, `sw.js`, both icons) to the repo root.
3. **Settings > Pages > Source: Deploy from a branch > `main` / root > Save**.
4. After a minute your app is at `https://YOUR-USERNAME.github.io/us-planner/`.

## 3. Install on both iPhones

1. Open the link in **Safari**.
2. Tap **Share > Add to Home Screen**.
3. Open the app from the home screen. Enter your own name and a **shared space code** (same on both phones, long and random, e.g. `jb-and-sam-k7x2q9`).

That's it. Anything either of you adds shows up on the other phone within a second or two.

## Using it

- **Week / Month**: tap **+** to add an event. Tap an event to edit or delete it. Your items are blue, your partner's are pink.
- **Roster**: set Work / Off and start and end times for each day. Your partner sees them on the Week and Month views.
- **Lists**: shared groceries, to-dos, ideas. Tick to complete, ✕ to remove.

## Good to know

- The space code is the "password". Keep it private and long. Anyone who has it can read and edit your planner.
- Free Firebase limits (Spark plan) are far above what two people will ever use.
- To change the space or name later: delete the app from the home screen, clear Safari website data for the site, and re-open.
