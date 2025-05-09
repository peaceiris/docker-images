---
mode: 'agent'
description: 'Create a new pull request'
---

## Steps to Create a Pull Request

Follow the steps below to create a pull request.

1. Run the git tool `git_status`.
2. If the current branch is main or master, run the git tool `git_create_branch` to create a new branch.
    - The `branch_name` should concisely describe the changes.
3. If a new branch was created, run the git tool `git_checkout` to switch to the new branch.
4. Use the git tool `git_add` to stage the changed files.
5. Run the git tool `git_commit` to commit the changes.
    - The Git commit message `message` must follow the Git Commit Message Convention.
6. Push the new branch to GitHub.
7. Create a pull request.
    - Use the commit message as the title.
    - Use the following template for the body. Leave the comments as they are.
    - Example command:

```sh
gh pr create --title "docs: update copilot instructions" --body "$(cat <<'EOF'
## Overview

(List a summary of the pull-request changes here in bullet points)

## References

(List any related links for this pull-request here)
EOF
)"
```

## Git Commit Message Convention

Use the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) convention for Git commit messages.
By following this convention, commit messages will be consistent and the changes will be clear.

Keep commit messages as short and concise as possible.

Example commit messages:

- `feat: add new feature`
- `fix: remove bug`
- `docs: add section`
- `style: apply gofmt`
- `refactor: split functions`
- `test: add new test`
- `chore: fix log message`
- `build: bump lib from v1 to v2`
