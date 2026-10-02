---
name: drop-mr
description: Opens a GitLab/GitHub merge request from the current branch with a brief e2e body. User-invoked via /drop-mr. Does not touch Jira.
disable-model-invocation: true
---

# drop-mr

Open one MR for the current branch. Stop after the MR URL is posted. Do not touch Jira (that is `/drop-mr-and-jira`).

Read [mr-body.md](mr-body.md) before writing title or body.

## Preconditions

- Dirty tree (uncommitted work): stop and say so. Do not commit unless the user asked.
- No commits ahead of the target: stop. There is nothing to drop.
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

## Push

```bash
git push -u origin HEAD
```

## Create

Title + body from [mr-body.md](mr-body.md). Pass the body via HEREDOC.

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

If an open MR already exists for this branch: reuse it (update title/body only if they are wrong). Do not open a second.

## Done

Reply with the MR URL only plus one line if something was off (retargeted, reused existing). No recap of the diff.