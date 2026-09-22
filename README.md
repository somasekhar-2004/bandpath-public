# bandpath-public

Public, source-free static pages for the Bandpath IELTS app, served via GitHub Pages.

Contains only page assets — no app source code.

## Pages

- `index.html` — email-confirmation landing page. Set as the mobile app's `EMAIL_CONFIRMATION_REDIRECT_URL` and added to Supabase's Auth → URL Configuration → Redirect URLs allowlist. Reads the query string Supabase's own `/auth/v1/verify` redirect appends (`?code=...` on success, `?error=...` on failure) and shows a matching message — it never calls the Supabase API itself. Logic mirrors `lib/confirmationPageState.ts` in the main app repo; keep both in sync if it changes.
