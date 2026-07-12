# Alfred AWS Console Granted

![demo](./.github/demo.gif)

Alfred Workflow to use [granted](https://granted.dev) to quickly open multiple AWS Console in Browser.

- Open Different AWS Accounts in Browser at the same time.
- Open Directly to a specific AWS Service.
- Open to a specific AWS Region.
- Prompt for Login/MFA if needed.

This workflow is a wrapper around [granted](https://granted.dev) CLI tool. Most of it's come from Granted itself. Thanks for the amazing tool.

## Prerequisites

- [granted](https://granted.dev) installed and configured.
  1. Install Granted

      ```bash
      brew tap common-fate/granted
      brew install granted
      ```

  2. Follow Granted's [Getting Started](https://docs.commonfate.io/granted/getting-started/)

## Installation

[Download](https://github.com/sherifabdlnaby/alfred-aws-granted/releases/latest) the latest `alfred-granted-*.alfredworkflow` file from [Releases](https://github.com/sherifabdlnaby/alfred-aws-granted/releases/latest).

Each release bundle ships with a build-provenance attestation. To verify it came from this repo's pipeline before installing:

```bash
gh attestation verify alfred-granted-*.alfredworkflow --owner sherifabdlnaby
```

## Usage

1. Open Alfred and type `aws` to see the list of AWS Accounts you have access to.
   1. Use ⌘ to open to Console without selecting a service.
   2. Use ⎇ to open to a specific region (By default granted uses Profile's region).

## License

[MIT License](./LICENSE)
Copyright (c) 2023 Sherif Abdel-Naby

## Credits

- [granted](https://granted.dev) for the Amazing CLI tool to manage AWS Console Access.
- Inspired by [aws-vault-alfred-workflow](https://github.com/kangaechu/aws-vault-alfred-workflow)

## Development

This is a **data-only** Alfred workflow: it's `info.plist`, `regions.json`, `services.json`, the `services/*.svg` icons, and image assets — no source code or scripts. The "build" simply zips those files into a `.alfredworkflow`.

The project uses [**mise**](https://mise.jdx.dev) to pin its lint/format tools, expose tasks, and wire git hooks (no language runtime is needed).

<details>
<summary><b>Install mise</b> (first time on this machine)</summary>

```sh
brew install mise   # macOS / Linux (Homebrew)
# or: apt / dnf / pacman — see https://mise.jdx.dev/installing-mise.html
```

Activate it in your shell, then confirm:

```sh
echo 'eval "$(mise activate zsh)"' >> ~/.zshrc   # zsh; use bashrc for bash
# restart the shell
mise doctor
```

</details>

1. **Set up the repo** (once, and per new clone/worktree):

   ```sh
   mise trust && mise run setup
   ```

   `setup` installs the pinned tools and, via the `postinstall` hook, the [hk](https://hk.jdx.dev) pre-commit hook.
2. **Common tasks:**

   ```sh
   mise run check          # run all linters/formatters (alias: lint); add --fix to auto-fix
   mise run build          # zip the workflow into build/alfred-granted.alfredworkflow
   mise tasks              # list every task
   mise run <task> --help  # a task's flags
   ```

Commits automatically run the same `check` pipeline on staged files via hk. Fix any failures with `mise run check --fix`. CI runs `mise run check` (and a build smoke) on every PR. Releases are label-driven: add one of `major` / `minor` / `patch` / `skip-release` on the PR; merging tags, builds the `.alfredworkflow`, attests provenance, and publishes. Unlabeled merges fall back to `minor`.

See [`AGENTS.md`](./AGENTS.md) for how to extend the data/tooling (including for AI agents).

## Contribution

PR(s) are Open and Welcomed.
