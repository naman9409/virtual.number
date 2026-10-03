# Google sign-in update

Existing demo customer and administrator logins remain available. Google sign-in creates customer accounts only, with zero wallet balance and document verification required. Customers set their own six-digit PIN. Users appear under Admin → Clients with a Google account label and in the overview totals. Successful sign-ins appear in login activity.

## Update your local project

1. Stop the old process with Ctrl+C. Extract this full project into a new folder and open nexora-fullstack in VS Code.
2. Copy your old working server/.env into the new server folder. Keep PostgreSQL port 5433 and the correct password.
3. Run npm.cmd install.
4. Run npm.cmd run db:setup. This applies additive columns and preserves existing accounts, documents, balances and orders. Do not delete or reset your database.
5. Configure Google as below, then run npm.cmd run dev.

## Google configuration

1. Open https://console.cloud.google.com/ and create/select a project.
2. Open Google Auth Platform. Configure Branding, Audience and Data Access for basic identity (openid, email, profile). If in Testing, add your test users; for general customer availability use the publishing options Google provides.
3. Under Clients, create an OAuth client with application type Web application.
4. Authorized JavaScript origins: add http://localhost:5173 and your exact deployed frontend origin, e.g. https://your-site.netlify.app. Add http://localhost if required by Google's localhost setup. Include any actual alternative development port. No trailing path. This implementation uses a popup credential callback; no redirect URI is needed.
5. Copy the client ID (ends in .apps.googleusercontent.com) into backend server/.env:

GOOGLE_CLIENT_ID=your-client-id.apps.googleusercontent.com

Restart the Node process. No client secret is needed for this ID token flow. Never ask users for their Gmail password in Nexora.

## Hosted configuration

Set GOOGLE_CLIENT_ID on the Node backend host, along with DATABASE_URL and FRONTEND_ORIGINS matching the exact frontend origin. Apply npm run db:setup once to add the columns before starting the new backend. Deploy both frontend and backend; retain your Netlify NETLIFY_BACKEND_URL value. Run npm ci / npm install to install the added google-auth-library dependency. Keep all credentials in environment settings, never GitHub.

## Behavior and checks

Google's official Identity Services button sends an ID token to the backend. The official Google library verifies its signature, audience, issuer and expiry; the backend also checks a short-lived cookie-bound nonce and verified email. The stable Google subject identifies the account. Existing password accounts are not silently linked by matching email, and Google login cannot create an administrator. Account blocking and document approval continue to apply. The original demo logins are preserved. Google users have no known local password.

If the button says Google sign-in is unavailable, set GOOGLE_CLIENT_ID on the backend. For origin_mismatch, add the exact frontend origin to the Google client. If schema columns are missing, apply db:setup against the correct database. New online databases do not automatically contain the laptop's existing records.

Build and integration checks passed with a PostgreSQL-compatible test engine. Actual Google account sign-in must be checked after configuring your own Google client ID and authorized origins; it was not tested against your account.

Official setup: https://developers.google.com/identity/gsi/web/guides/get-google-api-clientid
Server verification: https://developers.google.com/identity/gsi/web/guides/verify-google-id-token
