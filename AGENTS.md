# AGENTS.md — layer-shortbread

Standalone candy repo for the `shortbread` layer — a source-built Tilemaker plus
the official Shortbread tilemaker config fleet, for Shortbread-schema OSM vector
tiles. The candy lives in `charly.yml` at the repo root, including the embedded
`skill:` entity projected into the marketplace corpus as
`/charly-versa:shortbread`.

Canonical files:

- `charly.yml` — the `shortbread:` candy entity (the `require:`, the
  `distro.{arch,fedora}:` build-dependency sections, the source-build steps, and
  the `check:` probes) and the `shortbread-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-versa:shortbread` — the owning skill. Why Tilemaker is source-built,
  the config bundle, the DAG invocation, and the check probes. Load before
  editing or troubleshooting the candy.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `command:`/`check:`, `distro:` sections). Load before
  editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- `charly box validate` at the repo root checks the manifest parses and
  validates.
- The candy's `plan:` `check:` steps split into build-context (the Tilemaker
  binary, its PATH resolution, `process.lua`, `config.json`, the stripped `.git`,
  the pre-created output dir, the `lua` runtime package) and a runtime probe
  (`shortbread-tiles-dir-runtime`).
- Tilemaker ships no `--version` flag; the probe resolves it by name instead.
- The Boost 1.91+ `-lboost_system` strip in the build step is deliberate — keep
  it, or the link fails on CachyOS/Arch.

## Modify this repo

- Edit the `shortbread:` candy entity AND the `shortbread-skill:` skill entity in
  `charly.yml` together. The skill is the projected usage source, so a build-dep
  or path change not mirrored in the skill leaves the corpus stale.
- Keep the build-dependency lists aligned with the `distro:` sections and the
  `package_map` in the runtime probe (`lua` → `lua-devel` on Fedora).
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
