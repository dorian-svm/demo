# AGENTS.md

Guidance for coding agents working in this repository.

## Project purpose

`dorian-svm/demo` is a fork of GitHub's Dependabot demo repository. It holds small
sample projects with outdated dependencies, used to try out Dependabot security
and version updates (see `README.md`). It is not a product: there is no application
logic to extend, and the dependency versions are intentionally old.

## Repository state

Tracked files (complete list):

- `README.md`: purpose and how to enable Dependabot on a fork.
- `.github/dependabot.yml`: Dependabot version updates for `github-actions`, `npm` (`/javascript`), and `bundler` (`/ruby`), weekly.
- `.github/workflows/android.yml`: "Android CI" on push/PR to `main`. Sets up JDK 1.8 and has no build or test step.
- `.github/workflows/greetings.yml`: posts a first-interaction message on new PRs and issues.
- `javascript/package.json`: npm manifest (`hot-formula-parser`, `lodash`). `main` is `index.js`, which does not exist.
- `javascript/package-lock.json` (lockfileVersion 1) and `javascript/yarn.lock` (yarn v1): two lockfiles for the same manifest.
- `ruby/Gemfile` and `ruby/Gemfile.lock`: gems `business` and `uk_phone_numbers`. Lockfile pins Bundler 2.7.2.
- `.gitignore`: ignores `node_modules/`.

## Commands

The repo defines no build, test, lint, or format scripts.

- `package.json` has no `scripts` section. Build, test, and lint commands: unknown.
- No test runner, linter, formatter, or type-checker config exists (no jest, eslint, rspec, rubocop, or similar config files).
- No `Rakefile`, `Makefile`, or Gradle build files are present, even though the Android CI workflow exists.
- Dependency install: unknown as a repo-defined command. Lockfiles exist for npm and yarn in `javascript/`, and Bundler is used in `ruby/` (`bundle install` is the standard Bundler command, not defined in the repo).
- Single-test command: unknown (no test suite is present).

Do not treat the Android CI workflow as evidence of a runnable build: it only installs JDK 1.8.

## Toolchain versions

The repo pins only the following:

- Bundler 2.7.2 (`BUNDLED WITH` in `ruby/Gemfile.lock`).
- JDK 1.8 (`java-version: 1.8` in `.github/workflows/android.yml`).
- Node.js and Ruby versions: unknown (no `.nvmrc`, `engines`, `.ruby-version`, or `.tool-versions`).

## Environment variables and services

- `GITHUB_TOKEN`: used by `greetings.yml` via `secrets.GITHUB_TOKEN`. Provided by GitHub Actions; not needed locally.
- No application environment variables are referenced in the repo.
- No external services (databases, queues, APIs) are required. Other service needs: unknown.

## Code layout

- `javascript/`: npm and yarn project with a manifest and two lockfiles. No source files.
- `ruby/`: Bundler project with a Gemfile and lockfile. No source files.
- `.github/`: Dependabot config and GitHub Actions workflows.

There is no `src/`, `lib/`, `test/`, or `spec/` directory.

## Conventions

Nothing is enforced by tools beyond what is listed above. Follow these from history:

- Commit messages: short imperative or descriptive subject, sentence case, no trailing period. Dependabot commits use `Bump <pkg> from <old> to <new> in /<dir>`. Human PRs often end with `(#NNN)`. Examples: `Fix typo in README for enabling Dependabot (#215)`, `Update Bundler version in Gemfile.lock to 2.7.2`. No Conventional Commits prefixes are used.
- Branches: the default branch is `main`. Workflows trigger on `main`.
- Error handling, logging, and naming conventions: unknown (no application code exists).
- When changing dependencies, keep the npm `package-lock.json` and `yarn.lock` consistent with `package.json`, or state which lockfile was updated. The existing lockfiles already disagree on `hot-formula-parser` (3.0.0 in npm, 3.0.2 in yarn).

## Things not to touch

- Lockfiles (`javascript/package-lock.json`, `javascript/yarn.lock`, `ruby/Gemfile.lock`): machine-generated. Yarn's lockfile says "DO NOT EDIT". Change them only through the package manager.
- Outdated versions in `package.json` and `Gemfile`: they are the demo's purpose. Do not upgrade them unless the task asks for it, because Dependabot tests against them.
- `.github/dependabot.yml`: changes alter what Dependabot scans. Edit only when asked.
- `.github/workflows/*.yml`: CI and greeting automation. Edit only when asked.
- Do not commit `node_modules/` (already ignored).
- Do not add secrets or tokens to any file.
