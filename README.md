# Fleet Ledger

A phone-friendly web app for running a family car-leasing fleet (Uber/Bolt drivers on a daily rate). It tracks daily payments, driver balances, owner costs, off-road days, mileage, document expiries and profit per car, and it imports M-Pesa Till/Paybill statements.

- **Hosting:** GitHub Pages (free). Serves the single `index.html` file.
- **Data and logins:** Firebase (free Spark plan). Firestore stores the data and Firebase Authentication handles the email/password logins.
- **Family members** only need an email and password. They don't need Claude, Google or GitHub accounts.
- **Drivers** don't log in. They get their balance as a WhatsApp/SMS statement from the app.

Setup takes about 30 minutes.

---

## 1. Create the Firebase project (stores the data)

1. Go to https://console.firebase.google.com and sign in with a Google account the family business will own long term.
2. Click **Create a project** and name it, e.g. `family-fleet`. Google Analytics isn't needed.
3. **Build > Firestore Database > Create database.**
   - Location: pick the closest region, e.g. `europe-west1` or `africa-south1` if offered. This can't be changed later.
   - Start in **production mode**.
4. **Build > Authentication > Get started > Sign-in method** and enable **Email/Password**.
5. **Authentication > Users > Add user** for yourself and each family member who will use the app. Choose a temporary password for each and tell them. They can reset it later from the sign-in screen.
6. **Firestore Database > Rules:**
   - Paste the contents of `firestore.rules`.
   - Replace the example emails with the real ones.
   - Click **Publish**.
   - Whenever someone joins or leaves, edit this list and publish again.
7. **Project settings (gear icon) > General > Your apps.** Click the **</>** (Web) icon, register an app called "Fleet Ledger" (skip Firebase Hosting), and copy the `firebaseConfig` values it shows.
8. Open `index.html`, search for `FIREBASE_CONFIG`, and paste in your values.

## 2. Put it on GitHub Pages (hosts the app)

1. Create a free account at https://github.com if you don't have one.
2. Click **New repository** and name it, e.g. `fleet-ledger`.
   - **Public** works with free GitHub Pages. The code is visible, but your data isn't, because the Firestore rules protect it.
   - **Private** with Pages needs a paid GitHub plan.
3. Click **Add file > Upload files**, then drag in `index.html`, `firestore.rules` and `README.md`, and click **Commit changes**.
4. Go to **Settings > Pages > Build and deployment**. Set Source to **Deploy from a branch**, Branch to `main`, folder `/ (root)`, and click **Save**.
5. After a minute or two the site is live at `https://<your-username>.github.io/fleet-ledger/`.
6. Back in Firebase, go to **Authentication > Settings > Authorized domains > Add domain** and add `<your-username>.github.io`.

## 3. Use it

- Open the link on your phone and sign in.
- Use the browser menu's **Add to Home screen** so it opens like an app.
- Follow the 4 setup steps: start date, cars, drivers, assignments.
- Each week, import the M-Pesa statement from **Collect > Import M-Pesa statement**.

To update the app later, edit `index.html` on GitHub (pencil icon) or upload a new version. Pages republishes automatically.

---

## Good to know

- **Cost:** the Firebase free plan allows 50,000 reads and 20,000 writes per day and 1 GB of storage. A fleet of 10–50 cars uses a small fraction of that.
- **Weak network:** entries are saved on the phone first and synced when the connection returns.
- **Backups:**
  - In the Firebase console you can export Firestore data (this needs the paid Blaze plan, which still costs nothing at this volume).
  - Alternatively, ask for a "Download backup" button to be added to the app.
- **Security:**
  - Only emails listed in `firestore.rules` can read or write data.
  - Remove a person's email (and delete their user in Authentication) when they stop helping.
  - Never paste service-account keys or passwords into `index.html`. The Firebase web config is safe to publish.
- **Other hosts:** the same `index.html` also works on Firebase Hosting, Netlify or Cloudflare Pages. Remember to add that domain under Authorized domains.
