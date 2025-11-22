### **Story 4.23: Authentication — Google Sign-In Integration (Supabase)**
- **Status:** Done
- **Ownership:** Frontend + Platform (TBD)
- **Target Deadline:** TBD

**As a** user,
**I want** to sign in with Google,
**so that** authentication is fast and familiar.

#### **Context & References**
- Builds on: Story 1.2 Authentication Service Integration (Supabase, SSR middleware).
- Stack: Supabase Auth; Next.js middleware for route protection.

#### **Scope**
- Enable and configure the Google OAuth provider in Supabase (client ID/secret, redirect URIs).
- Add "Continue with Google" to the login UI; call `supabase.auth.signInWithOAuth({ provider: 'google' })`.
- Handle OAuth return and session persistence; JWT flows continue to protect API routes.
- Error handling and analytics for provider failures or cancellations.

---

#### **Acceptance Criteria**
1. **Supabase configuration**
   - Google provider enabled with correct client ID/secret; redirect URIs include local and production domains.
   - Secrets are stored securely; environment variables (`NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`) remain in use.

2. **Login UI**
   - Login page shows a prominent "Continue with Google" button.
   - Clicking initiates OAuth and returns the user to the app with an active session.

3. **Middleware & session**
   - SSR route protection continues to work; logged-in users pass, others redirect to `/login`.
   - API client attaches JWT from Supabase; interceptors unchanged.

4. **Error handling & analytics**
   - Provider disabled, token expiration, and user cancellations show helpful messages.
   - Events logged for success/failure metrics.

---

#### **Dev Notes**
- Redirect URIs: include `http://localhost:3000`, staging, and production domains with `/auth/callback` if used.
- Consider an OAuth-driven onboarding step if new user profile fields are required.

#### **API Alignment**
- Existing middleware and JWT validation (Story 1.2) remain; no backend changes required for basic OAuth.

#### **Tasks / Subtasks**
- [x] Configure Google provider in Supabase project (AC 1)
- [x] Add UI button and OAuth flow (AC 2)
- [x] Verify SSR protection and API JWT flows (AC 3)
- [x] Implement error states and analytics (AC 4)

#### **Testing**
- Unit: login component states.
- Integration: OAuth redirect and session persistence.
- E2E: sign in/out with Google; protected routes; error scenarios.

#### **Definition of Done**
- Users can sign in with Google; sessions persist; protected routes behave correctly; tests pass.