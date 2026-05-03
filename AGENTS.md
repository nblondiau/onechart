# Repository Guidelines

## Project Structure & Module Organization

This repository contains Helm charts for application deployment patterns. Primary charts live under `charts/onechart`, `charts/cron-job`, and `charts/static-site`; each chart keeps its `Chart.yaml`, `values.yaml`, `values.schema.json`, templates, and `tests/` together. Shared template partials are in `charts/common/templates`. Packaged chart archives and Helm repository metadata are published from `docs/`. Root-level files such as `values.yaml`, `values-cron-job.yaml`, and `values-static-site.yaml` are local rendering examples.

## Build, Test, and Development Commands

- `make lint`: runs `helm lint` for all maintained charts.
- `make kubeval`: renders each chart and validates generated manifests against Kubernetes 1.20 and 1.24 schemas with `kubeval`.
- `make test`: updates chart dependencies and runs `helm unittest` for all chart test suites.
- `make package`: packages charts into `docs/` and regenerates `docs/index.yaml`.
- `make all`: runs linting, manifest validation, tests, and packaging.
- `make debug`: renders `charts/onechart` with root `values.yaml` and `--debug`.
- `make debug-cron-job`: renders `charts/cron-job` with `values-cron-job.yaml` and `--debug`.

Install prerequisites before running the full workflow: Helm 3, `kubeval`, and the `helm-unittest` plugin.

## Coding Style & Naming Conventions

Use YAML with two-space indentation. Keep Helm templates readable by using existing helper partials in `charts/common/templates` or each chart’s `_helpers.tpl` before adding duplicated template logic. Name resource templates after the Kubernetes object or feature they render, for example `deployment.yaml`, `service.yaml`, or `sealedFileSecret.yaml`. Name tests with the feature and `_test.yaml` suffix, such as `deployment_image_test.yaml`.

## Testing Guidelines

Every chart behavior change should include or update a `helm-unittest` suite under the affected chart’s `tests/` directory. Keep suites focused on the rendered object or feature being asserted. Run `make test` for unit coverage and `make kubeval` when templates or schemas change. Update `values.schema.json` when adding new public values.

## Commit & Pull Request Guidelines

Recent history uses short imperative or descriptive commits, sometimes with prefixes such as `feat:` and `fix:`. Keep commits scoped to one concern, for example `feat: add pod annotations for cron-job template`. Pull requests should describe the chart behavior change, link any related issue, and mention test coverage. Include rendered manifest snippets when they clarify non-trivial template changes.

## Release Notes

Release packaging is chart-driven. Do not hand-edit generated chart archives unless preparing a release; use `make package` so `docs/index.yaml` stays consistent with the packages in `docs/`.
