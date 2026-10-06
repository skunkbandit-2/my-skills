# User and system modes

Inspect `git config --show-origin --show-scope --get-all core.excludesFile`, the value at the requested scope, and the effective pathname before editing. Resolve configured paths with Git's pathname handling and platform path utilities. Read and preserve existing target rules.

## User

Use the existing user-owned file where possible. Without an explicit setting, check Git's default `$XDG_CONFIG_HOME/git/ignore` or `$HOME/.config/git/ignore` before choosing a new file. If selecting a new file, write it first and configure only the requested setting:

`git config --global core.excludesFile <absolute-path>`

Quote paths for the active shell. Preserve unrelated Git configuration.

## System

Choose a durable shared absolute path readable by intended users, rather than a profile or tilde path. Ask for a target when machine conventions do not identify one. Write the file first, then configure:

`git config --system core.excludesFile <shared-absolute-path>`

Follow environment permissions. If installation is unavailable, stage the file in a writable directory and provide exact installation commands for the selected platform and target. State that installation remains outstanding; do not substitute user mode.

## Effective behavior

A higher-precedence `core.excludesFile` setting can override the system setting. User and system values do not automatically combine multiple ignore files. Explain this when it affects the requested result and verify the effective configuration in a representative repository.

Configuration scope differs from pattern scope: leading `/` in these external ignore files anchors to each repository root.

Check ignored and retained paths using the main skill's Git matcher commands. If no repository is available, use a temporary isolated repository for pattern checks, distinguishing them from verification of installed configuration.

Consult https://git-scm.com/docs/git-config for unfamiliar configuration behavior.
