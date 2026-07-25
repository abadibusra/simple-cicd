# Simple CI/CD Pipeline with Jenkins

A complete CI/CD pipeline built with Jenkins that automatically tests,
builds, and deploys a Flask app on every git push.

## What it does

push → Jenkins polls repo (every 2 min) → Checkout → Setup → Lint →
Test → Build Image → Deploy → cleanup

- A failing lint or test STOPS the pipeline — broken code never gets
  built or deployed
- Every image is tagged with the build number for traceability and
  rollback
- Old images are auto-pruned (keeps last 2 — tunable retention policy)

## Pipeline stages

| Stage | Tool | What it checks/does |
|---|---|---|
| Checkout | git | pulls the exact commit that triggered the build |
| Setup | venv + pip | builds a clean, reproducible environment |
| Lint | ruff | code quality gate — fails on issues |
| Test | pytest | behavior gate — fails on broken functionality |
| Build Image | docker | packages app into a versioned image |
| Deploy | docker | replaces the running container with the new version |
| post/always | shell | notifications + image retention cleanup |

## Key decisions

(write 3-4 of these in your own words — see below)

## Tech

Jenkins (declarative pipeline, pipeline-as-code) · Docker · Python/Flask ·
pytest · ruff · Git/GitHub (SCM polling + credentials)
