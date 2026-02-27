# PR Source Guard for `main`

This repo demonstrates a GitHub Actions check that blocks pull requests into `main` unless the source branch matches approved production patterns.

## Rule

Workflow file: `.github/workflows/enforce-main-from-prod.yml`

When a PR targets `main`, the check passes only if the source branch is:

- `prod`
- `prod-*` (for example, `prod-hotfix-2026-02-27`)

Any other source branch fails the check.

## How It Works

The workflow runs on pull request events (`opened`, `reopened`, `synchronize`, `ready_for_review`) where base branch is `main`.

It reads:

- `github.event.pull_request.base.ref` (target branch)
- `github.event.pull_request.head.ref` (source branch)

Then it exits `0` for allowed branches and exits `1` with an error message for everything else.

## Important Setup

To enforce this as a hard gate, add branch protection on `main` and require this workflow check to pass before merge.

## Example Runs

- Failure example (blocked): https://github.com/beyondenough/issues-example/actions/runs/21960328564
- Success example (allowed): https://github.com/beyondenough/issues-example/actions/runs/21960345368
