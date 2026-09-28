# layer-check-group-layer

A `group` structural-kind check fixture: the deployable member of the
`check-group` R10 bed (C2-group).

The `check-group-layer` candy drops `/etc/check-group-marker` in the filesystem
it is deployed into. It is the deployable member of the `check-group` bed — the
externalized `group` structural kind. The marker is asserted by a check authored
on the **deploy node's** plan (a composed candy's `plan:` never reaches a
deploy-scope check runner, so a check authored here would never run at deploy
scope). The bed deploys this member INSIDE its disposable eval VM, so the marker
lands in the guest, never on the operator's host. A dedicated marker path (not
`check-local`'s or `check-structkind`'s) keeps the beds from colliding when
`/verify-beds` fans them out concurrently.

The candy is a **fixture**: it ships no user-facing service and has no `skill:`
entity. Its `plan:` carries a `write:` step plus `file:` `check:` probes and one
`context: [runtime]` `command:` probe. The owning family skill is
`/charly-check:check`; the missing owning `skill:` entity is tracked by
[opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `check-group-layer` |
| Kind exercised | `group:` (the externalized structural kind) |
| Effect | writes `/etc/check-group-marker` (mode `0644`, content `check-group v1`) |
| Plan | a `write:` run step, `file:` `check:` probes, one `runtime` `command:` probe |
| Owns | 0 `skill:` entities (fixture) |
| Service / port | none |

## How to use it

Compose it as a layer ref in a box's nested `candy:` list (the composition list).
A box is a `candy:` node carrying the box's `base:` image and a nested `candy:`
list of layer refs (the nested `candy:` is the composition list; the outer
`candy:` is the box body):

```yaml
my-box:
  candy:                  # the box body (an IMAGE is a `candy:` node carrying `base:`)
    base: fedora          # the box's base image
    candy:                # the box's composition list
      - '@github.com/opencharly/layer-check-group-layer:v2026.239.1626'
```

The fixture is driven by the `check-group` bed as its deployable member, not by a
user box.

## Layout

- `charly.yml` — the `check-group-layer:` candy entity (no `skill:` entity).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning family skill: `/charly-check:check`
- Missing `skill:` entity: [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291)
- Schema skill: `/charly-pod:pod` (the `group:` kind)
- Authoring reference: `/charly-image:layer`
- [`opencharly/marketplace`](https://github.com/opencharly/marketplace) — the projected corpus
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
