# GitHub deployment model demo

- `.github/workflows/deploy.yml`: manual trigger with an environment dropdown.
- `.github/workflows/terraform-deploy.yml`: reusable workflow (would live in `platform/shared-workflows`).
- `environments/<env>/terragrunt.hcl`: per-environment config in source control.

## Setup

1. Push to GitHub.
2. Settings > Environments: create `dev`, `uat`, `pre`, `prod`.
   - Add required reviewers on `pre` and `prod`.
   - Optionally add variables `AZURE_CLIENT_ID`, `AZURE_TENANT_ID`, `AZURE_SUBSCRIPTION_ID` (without them, Azure login is skipped).
3. Tag a release: `git tag v1.0.0 && git push --tags`.

## Demo

1. Actions > Deploy > Run workflow: `dev` from `main` succeeds.
2. `prod` from `main` fails at the ref check.
3. `prod` from tag `v1.0.0` waits for approval, then deploys.
   Each run builds an artifact named `demo-app-<env>-<sha>` and the deploy job echoes that it is using it.
