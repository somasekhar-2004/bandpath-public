# bandpath-public

Public, source-free static pages for the Bandpath IELTS app, served via GitHub Pages.

Contains only page assets — no app source code.

## Pages

- `index.html` — email-confirmation landing page. Set as the mobile app's `EMAIL_CONFIRMATION_REDIRECT_URL` and added to Supabase's Auth → URL Configuration → Redirect URLs allowlist. Reads the query string Supabase's own `/auth/v1/verify` redirect appends (`?code=...` on success, `?error=...` on failure) and shows a matching message — it never calls the Supabase API itself. Logic mirrors `lib/confirmationPageState.ts` in the main app repo; keep both in sync if it changes.
- `reset-password/index.html` — password-recovery page. Set as the mobile app's `PASSWORD_RESET_REDIRECT_URL` (a **separate** URL from the one above — never the same) and added to Supabase's Auth → URL Configuration → Redirect URLs allowlist as its own entry. Unlike the confirmation page, this one has real work to do: it initializes a Supabase client (implicit-flow, using the same public `EXPO_PUBLIC_SUPABASE_URL`/`EXPO_PUBLIC_SUPABASE_ANON_KEY` the mobile app itself ships with — never a service-role key or any other secret), listens for the `PASSWORD_RECOVERY` auth event to pick up the recovery link's session tokens, and lets the user set a new password via `updateUser({ password })`. See the page's own header comment for exactly why this must be an implicit-flow client rather than PKCE. Logic mirrors `lib/passwordResetPageState.ts` in the main app repo (link-state determination and password validation); keep both in sync if either changes.
- `privacy/index.html` — public Privacy Policy for the Bandpath IELTS app, readable without login (linked from app stores and the app's own Help screen). Static content only — never calls the Supabase API. Content is a direct audit of the main app repo's actual data practices at the time of writing, not a generic template; update it if the app's real data collection, third-party services, or account-deletion flow change.
