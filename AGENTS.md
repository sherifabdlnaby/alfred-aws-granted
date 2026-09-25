# AGENTS.md

## Toolchain (mise)

This project uses [**mise**](https://mise.jdx.dev) to pin tools, expose tasks, and wire git hooks. `mise.toml` is the source of truth. Don't install tools by hand or add ad-hoc scripts; add a mise tool or task instead.

This is a **data-only Alfred workflow** — `info.plist`, `regions.json`, `services.json`, the `services/*.svg` icons, and image assets. There is **no source code and no scripts**, so `[tools]` pins only linters/formatters plus the hook runner (`hk`); there is no language runtime to install. The "build" just zips the data into a `.alfredworkflow`.

**Setup** (once, and per new worktree): `mise trust && mise run setup`.

**Run via mise.** Run `mise run check` before you call work done. A few examples, not the full list:

```sh
mise run check          # all linters/formatters/validators (alias: lint); add --fix to auto-fix
mise run build          # zip the data into a .alfredworkflow (--version stamps info.plist)
mise run test           # no-op — data-only workflow, no tests
mise tasks              # discover every task
mise run <task> --help  # a task's flags
```

Prefer `mise run <task>` over calling the tool directly, so local, hooks, and CI stay in sync.

## Git hooks (hk)

`mise run setup` installs the [hk](https://hk.jdx.dev) hooks (on Git 2.54+ they live in git config, so an empty `.git/hooks/` does not mean no hooks). Commits run the commit gates on staged files, pushes run the push gates, and CI runs both as `mise run check`, so a green commit is not yet a green CI. Fix failures with `mise run check --fix`. Don't disable steps to push a commit through; `git commit --no-verify` skips hooks for a WIP commit.

For a shorter loop, target steps: `mise run check --step typos` or `--skip-step lychee` (tab-completes step names).

## Releases

Merging to `main` with a bump label (`major` / `minor` / `patch`) tags, builds the `.alfredworkflow`, attests provenance, and publishes a GitHub Release. Use `skip-release` when the change should not cut a version. Category labels (`feature`, `bug`, `docs`, `ci`, …) only group the generated notes. Manual RC: Actions → Release → `prerelease: true`.

## Project notes

- The workflow is **data + assets only**: `info.plist`, `regions.json`, `services.json`, `services/*.svg`, `icon.png`, `region.png`. Add/extend an account, region, or service by editing the JSON and (for a new service) dropping its `.svg` icon under `services/` — no code involved.
- `info.plist` carries a placeholder version (`1.3.37`). `mise run build --version vX.Y.Z` stamps the real version into a throwaway copy at build time, then restores the placeholder so the working tree stays clean.
- **Linter coverage gaps to know about:** binary assets (`*.png`, `*.gif`) are excluded from linting. There is no SVG-specific linter, so the `services/*.svg` icons get only generic whitespace/newline checks. JSON is formatted by `prettier`, scoped to `*.json` only. AWS service names and example ARNs/account IDs can trip `typos`/`betterleaks` — allowlist confirmed false positives in `.config/typos.toml` / `.config/betterleaks.toml`, don't disable the step.
- `mise run clean` removes `build/`.

## Extending the setup

Changing tools, tasks, env, or hooks? Edit the config, don't bolt on scripts, then run `mise run check`. Where things live:

- **`mise.toml`**: the source of truth for `[tools]`, `[tasks]`, `[env]`/`[vars]`, `[hooks]`, `[doctor.checks]` (prerequisites mise can't install; `setup` runs them first), and `[settings]`.
- **`mise.lock`**: resolved versions plus checksums. Commit it; regenerate with `mise install` then `mise lock --platform macos-arm64,linux-x64` after a `[tools]` change.
- **`.config/mise/`**: project-local state (the `setup` stamp is gitignored; the directory is committed so the stamp has a home). File tasks can live in `.config/mise/tasks/`.
- **`.config/hk.pkl`**: the pre-commit and `check` pipeline (linters and formatters, in Pkl). Add a lint step to a gate tier here.
- Linter configs live beside it in `.config/`; `hk.pkl` points each tool at its file. zizmor's lives at `.github/zizmor.yml`.

For tool, task, and hook syntax, see the [mise](https://mise.jdx.dev) and [hk](https://hk.jdx.dev) docs.
