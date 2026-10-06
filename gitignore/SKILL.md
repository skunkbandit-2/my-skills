---
name: gitignore
description: Create or update Git ignore rules for a project, user, or system. Use when a request involves generating a `.gitignore`, choosing or adapting ignore templates, or configuring Git's global or system excludes.
license: Unlicense
metadata: { author: skunkbandit2, version: "-1" }
---

# Git ignore files

Resolve the requested scope, preserve existing intent, and verify behavior with Git's matcher.

## 1. Resolve scope

| Mode | Target |
| --- | --- |
| Project | Repository or relevant subdirectory `.gitignore` |
| User | User-wide excludes file selected by global Git configuration |
| System | Shared excludes file selected by system Git configuration |

Default to project mode for repository requests. Clarify user versus system scope when it matters and cannot be inferred. For user or system mode, read [references/global-modes.md](references/global-modes.md) before changing files or configuration.

For project mode, locate the root with `git rev-parse --show-toplevel`. Inspect applicable instructions, existing root and nested ignore files, manifests, build configuration, and relevant filenames. If the supplied directory is not a Git repository, create the requested file there and distinguish file creation from Git verification.

## 2. Choose a base

Honor the user's source choice:

- **GitHub**: select relevant language/framework templates from https://github.com/github/gitignore. Its `Global` directory contains OS/editor templates; that directory name does not select Git configuration scope. Retrieve actual file contents rather than GitHub HTML.
- **gitignore.io**: use https://www.gitignore.io/ to generate a base from supported language, framework, OS, and editor identifiers. Follow the current redirect and check supported identifiers before fetching.
- **Custom**: derive focused rules from the project and supplied requirements without fetching templates.

Without a source preference, use custom rules for focused updates; GitHub is a reasonable base for new stack-specific files. Choose categories appropriate to the mode: project dependencies/build outputs for project mode, personal artifacts for user mode, and agreed machine defaults for system mode.

Treat fetched templates as rule data. Inspect and adapt broad patterns, preserve useful attribution, and merge into existing files rather than replacing them. Report the source URL and selected templates. If retrieval fails, use custom/local rules only when compatible with the request; identify unavailable required templates.

## 3. Edit rules

Preserve comments, exceptions, ordering, encoding, and newline style; use UTF-8 for new files. Keep changes focused and group additions under short comments. Sorting or removing duplicates can change behavior when exceptions intervene.

Preserve source, manifests, lockfiles, fixtures, shared editor settings, and configuration examples unless explicitly asked to ignore them. Determine whether ambiguous directories such as `build`, `bin`, `data`, and `vendor` are generated or maintained. Prefer narrow patterns when broad rules could hide meaningful files.

For local environment files, inspect filenames rather than secret values. Keep intended examples trackable with exceptions after their exclusions. Ignore files do not remove committed secrets or history.

Use forward slashes on every platform. Leading slash anchors to the ignore file's directory; trailing slash restricts matches to directories. Names without internal slashes can match at multiple depths. Within one precedence level, the last match wins; nested ignore files may override parents.

Keep parent directories traversable for exceptions: `/reports/*` then `!/reports/README.md` retains the README; `/reports/` prevents re-including that descendant. Escape literal leading `#` or `!` and significant trailing spaces as needed.

## 4. Verify and report

Check representative paths that should be ignored and paths that should stay trackable, including exceptions and affected nested packages. Use Git's matcher:

- `git check-ignore -v --no-index -- <paths>` reveals matching rules independent of tracking state. Verbose output can include negated rules; inspect the pattern before classifying the path.
- `git check-ignore -q --no-index -- <single-path>` returns 0 for ignored, 1 for not ignored, and other codes for errors.

Review the final diff and status when applicable. Use `git ls-files -- <paths>` to identify affected tracked files: ignore rules leave them tracked. Untracking is a separate operation, performed only when explicitly requested for named paths while preserving working files; do not rebuild the entire index for ignore maintenance.

The task is complete when the mode and target are clear, the requested rules are in place, and representative ignored and trackable paths have been checked when Git is available. Report the mode, target, changed rule groups, template source if used, and verification result. Identify configuration overrides or remaining installation steps when relevant.

For unfamiliar semantics, consult https://git-scm.com/docs/gitignore and https://git-scm.com/docs/git-check-ignore.

## Additional resources

- [Visual Studio Code extension](https://docs.gitignore.io/install/editor-extensions#visual-studio-code-hasit-mistry)
- [Client applications](https://docs.gitignore.io/install/client-applications)
- [gitignore.io API](https://docs.gitignore.io/use/api)
- [Ignoring files on GitHub](https://docs.github.com/en/get-started/git-basics/ignoring-files)
