---
name: drop-pr
description: Opens a GitLab/GitHub merge request from the current branch with a brief e2e body. User-invoked via /drop-pr. Does not touch Jira.
disable-model-invocation: true
---

# drop-pr

Open one MR/PR for the current branch. Stop after the URL is posted. Do not touch Jira (that is `/drop-pr-and-jira`).

Read [pr-body.md](pr-body.md) before writing title or body.

## Existing MR/PR

If an open MR/PR already exists for this branch: **state the URL, keep it, stop.** Do not open a second. Do not edit title or body.

## Authorship

Author and committer must be **Uanela \<uanelaluiswayne@gmail.com\>**. Never add `Co-authored-by` for `uanela_technoplus` (or any TechnoPlus work account). Set `GIT_AUTHOR_*` and `GIT_COMMITTER_*` for that identity. Do not change git config.

## Preconditions

- Dirty tree (uncommitted work): stop and say so. Do not commit unless the user asked.
- No commits ahead of the target **and** no existing MR/PR: stop. There is nothing to drop.
- Never update git config, never `--force` push, never skip hooks.

## Inspect (parallel)

```bash
git status
git diff && git diff --staged
git log --oneline -15
git rev-parse --abbrev-ref HEAD
git remote get-url origin
git symbolic-ref --short refs/remotes/origin/HEAD || true
```

Also `git log <target>..HEAD` and `git diff <target>...HEAD` once the target is known.

**Target branch:** user override → else `origin/HEAD` → else first of `ci/staging`, `main`, `master` that exists on origin.

**Host:** GitLab if origin host is not github.com; GitHub if it is.

Look up an existing open MR/PR for this source branch **before** creating one (GitHub: `gh pr view`; GitLab: list MRs filtered by `source_branch`).

## Push

```bash
git push -u origin HEAD
```

## Create (only if none exists)

Title + body from [pr-body.md](pr-body.md). Pass the body via HEREDOC.

**GitHub** (`gh`):

```bash
gh pr create --title "type(scope): subject" --body "$(cat <<'EOF'
body
EOF
)"
```

**GitLab** (`glab` if installed):

```bash
glab mr create --target-branch <target> --title "type(scope): subject" --description "$(cat <<'EOF'
body
EOF
)"
```

**GitLab without `glab`:** `POST /api/v4/projects/:id/merge_requests` with a token from `git credential fill` for that host (`source_branch`, `target_branch`, `title`, `description`). Do not print the token.

## Done

Reply with the MR/PR URL. If it already existed, say that in one line. No recap of the diff.
