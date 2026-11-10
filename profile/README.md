# Harbourworks

**Developer tools for people who ship fast and can't afford to ship broken.**

---

## Seaworthy

### Know your AI-built app is safe before you launch it.

Cursor, Claude Code, Lovable and Bolt write working code fast. They also skip authorisation, leak keys into the browser and leave database tables open to anyone with your public key. Seaworthy finds those gaps before your users do.

```
npx seaworthy-cli
```

```
  Seaworthy  ·  my-app  ·  Next.js, Supabase, Stripe
  142 files, 41 ms. Nothing left this machine.

  ✖ Not seaworthy: 2 critical, 1 high

   CRITICAL   Table public.payments has no row-level security   SW-D01
  supabase/migrations/0001_init.sql:20
  > 20 │ create table public.payments (
  Why  Anyone with your public anon key can read, edit and delete every row.
  Fix  alter table public.payments enable row level security; then add policies.

   CRITICAL   Supabase service-role key committed   SW-S01
  .env:3
  >  3 │ NEXT_PUBLIC_SUPABASE_SERVICE_ROLE_KEY=eyJhbGciOiJI••••••••
  Why  This key bypasses row-level security entirely.
  Fix  Roll the key in Supabase, then use it only in server code.
```

**Your code never leaves your machine.** Seaworthy runs locally and in your own CI runner. It doesn't upload anything and you don't need an account. The only network call sends dependency names and versions to the public OSV vulnerability database, and `--offline` turns that off too.

**It checks logic, not just patterns.** Seaworthy works through your Supabase migrations to confirm row-level security is really on, and traces your API routes to confirm someone checks who owns a record before returning it. It also tells a public anon key apart from a service-role key, so you don't drown in false alarms.

**It's built for apps that call LLMs.** It flags prompt injection paths, model output rendered as HTML, agent tools with shell access and no confirmation, and MCP servers with no authentication.

**Every finding comes with a fix.** You get the file and line, two sentences on why it matters, and a fix: a patch, or a prompt to paste straight back into your AI coding tool.

### What it checks

| Area | Checks |
| --- | --- |
| Secrets | Keys committed to the repo, `.env` not ignored, secrets exposed to the browser, LLM keys in client code |
| Database | Supabase tables without RLS, policies that allow everyone, open Firebase rules, public storage buckets |
| Auth | Routes with no auth check, records fetched without an ownership check, missing rate limits, wildcard CORS, SQL injection |
| Payments | Stripe webhooks without signature checks, prices taken from the client |
| AI and agents | Prompt injection, unsafe model output, over-powered agent tools, unauthenticated MCP servers |
| Supply chain | Known vulnerable dependencies, packages that don't exist or appeared yesterday |
| Headers | Missing CSP, frame options and HSTS |

### In CI, on every pull request

`seaworthy init` adds a GitHub Action that scans each pull request, posts a summary comment, uploads results to GitHub code scanning, and blocks the merge on critical or high findings.

### Free and Pro

| | **Free** | **Pro** |
| --- | --- | --- |
| Every check, local CLI and GitHub Action | ✔ | ✔ |
| Terminal, Markdown, SARIF and JSON reports | ✔ | ✔ |
| Fix text and paste-ready AI prompts | ✔ | ✔ |
| Fix pull requests, generated in your own CI | | ✔ |
| Scan history and trends per repo | | ✔ |
| Shareable report for clients and investors | | ✔ |
| Team policy, scheduled scans, Slack and email alerts | | ✔ |
| Priority support | | ✔ |
| Licence | Free for any use except a competing product. Source-available; each release becomes Apache 2.0 after two years | Commercial licence, per key. Founding-member pricing at launch |

Pro never stores your code. It keeps only finding metadata: rule, file path, line and time.

---

<sub>Seaworthy is an aid, not an assurance: no tool finds everything. Harbourworks is run by Dynamo Maritime Ltd, registered in England and Wales, company number 17152743.</sub>
