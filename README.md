# anderix/.github

Org-wide defaults for the anderix repos. Right now that means reusable CI
workflows: the policy lives here once, and each repo calls it instead of
carrying its own copy.

## Calling a workflow

A consumer repo's `.github/workflows/ci.yml` is a few lines:

```yaml
name: ci

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    uses: anderix/.github/.github/workflows/rust-ci.yml@main
```

The trigger stays in the caller, because when a repo builds is the caller's
business; what a build checks is the fleet's.

Pinning `@main` rather than a tag is deliberate. A correction to the policy
should reach every repo the next time it builds, which is the reason the file
is here and not copied into each one.

## Why this repo exists separately

excelano has its own `.github` repo holding the same kind of policy. The
duplication is deliberate: tools under anderix may not depend on excelano, and
a workflow reference is a dependency. Two copies across two orgs keeps the
boundary; it is not the nineteen copies this arrangement exists to prevent.

This repo must stay public. A private one cannot serve reusable workflows to
public callers.

## What does not belong here

GitHub will also serve org-wide community health files from this repo —
`SECURITY.md`, `CONTRIBUTING.md`, issue templates — to any repo that lacks its
own. Security policies are deliberately not consolidated that way: they differ
per tool in exactly the lines that matter, describing what that binary can
reach and what it stores. The reasoning is in `~/notes/dry_boundary.md`.
