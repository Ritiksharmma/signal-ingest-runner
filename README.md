# signal-ingest-runner

A free Actions runner for the [Signal](https://signal.sharmaritik.com) ingest
job — nothing else. This repo carries **no application code**: it exists only
because [`Ritiksharmma/my-website`](https://github.com/Ritiksharmma/my-website)
(private) is billed for its own Actions minutes, and this repo, being public,
is not.

## What it does

`.github/workflows/ingest.yml` runs every 15 minutes. It checks out
`my-website` with a read-only, repo-scoped token, then runs the same
`npm run signal:migrate` / `npm run signal:ingest` commands `my-website`'s own
ingest workflow used to run — against the same Neon database. GitHub bills
Actions minutes to whichever repo the *workflow* lives in, not whichever repo
it checks out, so running the workflow from here costs nothing while the code
and data it touches stay exactly where they were.

## Secrets this repo needs

| Secret | Scope | Notes |
|---|---|---|
| `MY_WEBSITE_PAT` | Fine-grained PAT, `my-website` only, Contents: Read-only | Used solely for `actions/checkout` against the private repo. Rotate before it expires. |
| `SIGNAL_DATABASE_URL` | Same Neon connection string as `my-website`'s own secret of the same name | Same database, same schema, same everything — this job is not a fork of Signal's data, it's the same ingest running from a different Actions billing context. |

## Why this exists instead of just making my-website public

Keeps the site's source and git history private while still getting free,
unmetered scheduled Actions runs — see the history in
`my-website`'s `.github/workflows/signal-ingest.yml` and
`docs/signal/PHASE-1.md` for the incident that prompted this split.
