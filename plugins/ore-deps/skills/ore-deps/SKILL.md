---
name: ore-deps
description: Updates the project's dependencies to the latest stable versions.
---

## Planning phase

1. Identify the language runtimes, package managers, outdated packages, and the versions specified in CI files.
2. Review all changelogs and release notes, and check compatibility (including peer dependencies).
3. Sort each package, and each dependency specific to CI files (actions, tools fetched inline, etc.), into one of the following.
   - **Individual handling**: needs code or configuration changes.
   - **Bulk update**: can be bumped as-is.
   - **Skip**: cannot be updated because of a compatibility problem.
4. Present the following information to the user, and move on to the execution phase only after getting their confirmation.
   - Language runtimes: current version → new version, the update/skip decision, and the reason.
   - Package managers: same as above.
   - Individually handled packages: package name, current version → new version, and a summary of the required code changes.
   - Bulk-updated packages: a list of package names and new versions.
   - Skipped packages: package name and the reason for skipping.
   - Individually handled CI items: item name, current version → new version, and a summary of the required configuration changes.
   - Bulk-updated CI items: a list of item names and new versions.
   - Skipped CI items: item name and the reason for skipping.

## Execution phase

### Language runtimes

1. Find and update every file that specifies a version.
2. Confirm that the build and tests pass.
3. Commit separately. Example message: `Update Node.js to v22.0.0`

### Package managers

1. Update to the latest version.
2. Confirm that the build and tests pass.
3. Commit separately. Example message: `Update pnpm to v10.0.0`

### Individually handled packages

Do the following for one package at a time.

1. Update it.
2. Fix the code based on the changelog and release notes.
3. Confirm that the build and tests pass.
4. Commit separately. Example message: `Update foo to v3.0.0`

### Bulk-updated packages

1. Update all the remaining packages together.
2. Confirm that the build and tests pass.
3. Commit them together. Message: `Update dependencies`
4. If the tests fail:
   1. Identify the culprit package from the error. If you cannot, revert everything, then update and test one package at a time until you find it.
   2. Revert everything.
   3. Handle the culprit package as an individually handled package (update → fix → test → separate commit).
   4. Update, test, and commit the remaining packages together again (if that fails, repeat recursively).

### CI files

Handle dependencies specific to CI files (actions, tools fetched inline, etc.). Runtime and package manager specifications inside CI files are handled as search targets of the `Language runtimes` and `Package managers` sections.

Do not verify with a build or tests (CI verifies them).

#### Individually handled CI items

Do the following for one item at a time.

1. Update the version in the CI file.
2. Fix the configuration based on the changelog and release notes.
3. Commit separately. Example message: `Update actions/setup-node to v5`

#### Bulk-updated CI items

1. Update all the remaining CI items together.
2. Commit them together. Message: `Update CI dependencies`
