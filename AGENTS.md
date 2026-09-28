# AGENTS.md — layer-check-group-layer

Standalone candy repo for the `check-group-layer` fixture — the deployable member
of the `check-group` R10 bed (C2-group, the externalized `group` structural
kind). It writes `/etc/check-group-marker` and carries **no `skill:` entity**.

Canonical files:

- `charly.yml` — the `check-group-layer:` candy entity (a `write:` run step,
  `file:` `check:` probes, one `context: [runtime]` `command:` probe; no `skill:`
  entity).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-check:check` — the owning family skill: the check plan authoring
  reference, the deploy-scope check model, the disposable beds, and the R10
  change classes. Load before editing any `plan:` step.
- `/charly-pod:pod` — the `kind: pod` / deploy schema reference, including the
  `group:` structural kind this fixture exercises. Load when a change touches the
  kind shape.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, package sections, service declarations).
  Load before editing any entity field or plan step.
- **Missing owning skill:** this fixture has no `skill:` entity, so no
  `/charly-check-group-layer:*` page is projected for it. The gap is recorded
  against the named batch
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has no
  per-repo candy gate. Its only workflow file is
  `.github/workflows/tag-on-merge.yml`.
- The fixture's proof is the `check-group` bed: the marker written inside the
  disposable eval VM and the deploy-node check that asserts it. A check authored
  on this composed candy's `plan:` would never reach a deploy-scope runner — the
  asserting check lives on the deploy node.

## Modify this repo

- Keep the marker path dedicated (`/etc/check-group-marker`): the sibling beds
  (`check-local`, `check-structkind`) fan out concurrently and must not collide.
- Keep the marker content stable (`check-group v1`): the bed asserts it.
- If an owning skill is authored, add the `skill:` entity here and update this
  signpost and the README in the same change.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
