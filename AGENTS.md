# AGENTS.md

Instructions for AI agents working in this repository.

## Repo Model

- `main` contains **only** `README.md` and `AGENTS.md` — the project index.
  Never commit prototype code, specs, or assets to `main`.
- **One branch per prototype.** Each branch is self-contained and holds the
  full prototype (specs, code, assets).
- To work on a prototype: `git checkout <project>`. Create a new branch for a
  new prototype; add it to the projects table in `README.md` (on that branch,
  then mirror the row onto `main`).

## Projects

_No prototypes yet._

## Commit Rules (this repo has enforced hooks)

1. Stage files, then mark each one explicitly before committing:

   ```sh
   git add <files>
   mark-for-commit <files>
   ```

   The pre-commit hook rejects commits containing unmarked files.

2. The global `prepare-commit-msg` hook **overwrites messages passed with
   `-m`** using a template (`type(branch): message`). To control the message,
   commit via the editor path:

   ```sh
   cat > /tmp/msg.txt <<'EOF'
   type(scope): subject

   body
   EOF
   printf '#!/bin/sh\ncp /tmp/msg.txt "$1"\n' > /tmp/ed.sh && chmod +x /tmp/ed.sh
   GIT_EDITOR=/tmp/ed.sh git commit
   ```

   Messages follow Conventional Commits: `feat`, `fix`, `docs`, `chore`,
   `refactor`.

3. Commits are SSH-signed through 1Password (`op-ssh-sign`). If signing fails
   with "failed to write commit object", unlock 1Password and retry.

## Running Prototypes

Each branch may ship its own entrypoint. Known ones:

- _No prototypes yet._

## Environment

- Linux, zsh. Python 3.12 (Flask, requests available), Node 20.
- Local CouchDB: `http://localhost:5984` (credentials via `COUCHDB_USER` /
  `COUCHDB_PASSWORD` env vars — never commit them), databases: aias,
  aoos, epistemica, pais, pais_raw, peos, pkts, pkts_raw, prs, ptocs, pwos,
  sociognosis, tecnica.
