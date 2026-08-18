# Self-host troubleshooting

This guide covers the most common setup problems when running CareerOps with your own Supabase project. Use the hosted demo at [careerops.telivity.app](https://careerops.telivity.app) only as a reference; never reuse its credentials.

## Blank page or Configure wall

**Symptoms:** the app is blank, or it keeps asking you to configure Supabase.

**Checks:**

1. Confirm that `web/config.js` exists. If it does not, copy `web/config.example.js` to `web/config.js`.
2. Check that `supabaseUrl` is your own Supabase project URL and that `supabaseAnonKey` is your own anon/publishable key, not an example or placeholder value.
3. Reload after saving the file and check the browser console for a malformed URL or JavaScript error.

Never commit `web/config.js`. It is gitignored and may contain deployment-specific values. Never reuse Telivity's demo keys.

## PostgREST errors or a missing relation

**Symptoms:** errors such as `relation ... does not exist`, `PGRST...`, or a board that cannot load.

**Checks and fixes:**

1. Confirm the app is connected to the Supabase project you intended to use.
2. Apply [`supabase/schema.sql`](../supabase/schema.sql) to a fresh project, or apply all required migrations to an existing project.
3. Review [`supabase/README.md`](../supabase/README.md) for the migration order and the full self-host setup.
4. Sign out and back in after applying schema changes so the session uses the intended project.

The client-side table map in [`web/SCHEMA.md`](../web/SCHEMA.md) is a helpful reference, but the SQL schema and migrations are authoritative.

## Rows will not save

**Symptoms:** a role, note, or other change appears to save but disappears, or Supabase reports a permission/RLS error.

**Checks:**

- Verify that you are signed in to the same project configured in `web/config.js`.
- Confirm the relevant tables and policies were applied from `supabase/schema.sql` or the migrations.
- Check that the authenticated user owns the rows being changed; Row Level Security intentionally blocks access to another user's data.
- If you changed projects or schema, sign out and back in, then retry with the browser console open.

Do not disable RLS as a workaround. Fix the project, migration, session, or ownership mismatch instead.

## Search, match, or rewrite fails

**Symptoms:** job search, automatic JD loading, resume matching, tailoring, cover-letter rewriting, or chat does not work.

Core board operations do not require Edge Functions. Search and AI features do:

- `run-search-mt` is needed for job-board search.
- `fetch-jd` is optional; paste the JD manually if it is not deployed.
- `resume-match`, `resume-rewrite`, and `chat` are needed for their server-backed AI actions, unless the relevant BYO OpenAI-compatible key is configured in Settings.
- `ai-free` only reports free-tier usage and is optional.

See the [Edge functions table](../web/README.md#edge-functions) for the complete list, including optional humanizer and provider-secret functions. Deploy the functions you actually use and inspect their logs for your own project. Do not paste API keys or private data into public issues or logs.

## Google OAuth says it is not enabled

This is expected until Google is configured for your own Supabase project. Enable Google in that project's Auth provider settings and configure its callback URL and credentials according to Supabase's documentation. Until then, use another enabled sign-in method; this message does not indicate that the CareerOps app itself is broken.

## Keep credentials and data separate

- Keep `web/config.js` local and uncommitted.
- Never reuse Telivity demo keys.
- Never commit API keys, resumes, job data, or other personal information.
- For the complete setup, see [`web/README.md`](../web/README.md), [`web/SCHEMA.md`](../web/SCHEMA.md), and [`supabase/README.md`](../supabase/README.md).
