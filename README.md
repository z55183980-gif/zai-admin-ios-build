# Zai Admin iOS builds

This public repository contains only the manual GitHub Actions build workflow.
Application source remains in the private `z55183980-gif/zai-admin-ios` repository.
No source, application binaries, test bundles, signing files, or diagnostic logs
are published as artifacts here.

Run **Build and upload TestFlight** on `main`, supplying the full 40-character
commit SHA from the private source repository's `main` history. The workflow
checks the selected revision, generates the native project, compiles and tests
it, signs the application, then uploads it to TestFlight. The public log reports
stage results only; compiler/test diagnostics are ephemeral and deleted when the
job finishes. Troubleshoot failures in the private source repository.

Configure the protected `appstore` environment before running:

- Secrets: `SOURCE_DEPLOY_KEY`, `BUILD_CERTIFICATE_BASE64`, `P12_PASSWORD`,
  `BUILD_PROVISION_PROFILE_BASE64`, `KEYCHAIN_PASSWORD`,
  `APPSTORE_API_PRIVATE_KEY`.
- Variables: `IOS_TEAM_ID`, `APPSTORE_API_KEY_ID`, `APPSTORE_ISSUER_ID`, and
  optionally `IOS_MARKETING_VERSION` (defaults to `1.0`).

`SOURCE_DEPLOY_KEY` must be a read-only deploy key registered only on the private
source repository. Store all passwords and private keys in GitHub Secrets;
never place their values in these files, workflow inputs, logs, or issues.
Restrict the environment to the `main` branch. This workflow has no push or pull
request trigger, does not check out public contributions, and grants no write
permission to its GitHub token.

Build numbers start at `2.0.1`, above the previous private-workflow series, and
increase with the workflow run number and attempt. Keep this repository's run
history; rebuilding an older run after publishing a newer run can produce an
obsolete build number.
