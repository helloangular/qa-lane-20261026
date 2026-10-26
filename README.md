# qa-lane-20261026

Dedicated QA lane for Helio's GitHub manual test cases (TC-117, TC-283, TC-291, TC-296, TC-297, TC-298, TC-299,
TC-300) on https://cicd.dagly.io. Synthetic: every job prints a line and sleeps; nothing is deployed. The repository
holds no secrets. Every commit that Helio dispatches is tagged `helio/workflow/<sha>` in the same step.

| Workflow | Started by | Environments | Cases |
|---|---|---|---|
| `.github/workflows/qa-governed.yml` | Helio (provider-managed, govern existing environment promotions) | `qa-test`, `qa-prod` (required reviewer: the QA reviewer account; admin bypass off) | TC-283, TC-291, TC-296, TC-297 |
| `.github/workflows/qa-adopt-observe.yml` | a push under `qa-adopt/` on `main` (also `feature/**` pushes and pull requests, which the policy refuses) | `qa-adopt-test` (unprotected) | TC-298 (Observe only) |
| `.github/workflows/qa-adopt-govern.yml` | a push under `qa-govern/` on `main` | `qa-test`, `qa-prod` | TC-298 (Govern existing environment promotions) |
| `.github/workflows/qa-catalog-release.yml` | Helio (Environment Catalog) | catalog environments `qa-cat-preprod`, `qa-cat-prod-east`, `qa-cat-prod-west` (break-glass reviewer: the QA GitHub user) | TC-117, TC-299, TC-300 |
| `.github/workflows/qa-probe.yml` | the lane builder once | `qa-probe` | connection readiness probe |

The catalog template is `helloangular/helio-client-capacity-20260916/.github/workflows/qa-lane-deploy.yml@qa-lane-20261026`.
Per-commit switches for the catalog jobs: `.helio-qa/catalog-fail/<environment>` (that job fails) and
`.helio-qa/catalog-slow/<environment>` (that job runs 300 s). `VERSION` is changed to make a new commit.
