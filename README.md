# android-signer-action

[![GitHub release (latest by date)](https://img.shields.io/github/v/release/caffeine-soft/android-signer-action?style=flat-square)](https://github.com/caffeine-soft/android-signer-action/releases)
[![Marketplace](https://img.shields.io/badge/Marketplace-Android_Signer_Action-blue?style=flat-square&logo=github)](https://github.com/marketplace/actions/android-signer-action)

Signs an Android AAB/APK file using jarsigner.

This GitHub Action allows you to easily sign Android `.apk` and `.aab` files using `jarsigner` inside your CI/CD pipelines.

## Usage

Create a workflow file (e.g., `.github/workflows/sign.yml`) in your repository:

```yaml
name: Sign Android Release

on:
  push:
    branches:
      - main

jobs:
  sign:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v6

      # (Assume you have a step here that builds your APK/AAB into the 'app/build/outputs' directory)

      - name: Sign Android Release
        uses: caffeine-soft/android-signer-action@v1
        with:
          release_dir: 'app/build/outputs'
          signing_key: ${{ secrets.SIGNING_KEY }}
          alias: ${{ secrets.ALIAS }}
          key_store_password: ${{ secrets.KEY_STORE_PASSWORD }}
          key_password: ${{ secrets.KEY_PASSWORD }}
```

## Inputs

| Input | Description | Required | Provide as Secret |
| --- | --- | --- | --- |
| `release_dir` | Directory containing the APK/AAB files to sign | Yes | No |
| `signing_key` | Base64 encoded string of the keystore | Yes | Yes |
| `alias` | Key alias | Yes | Yes |
| `key_store_password` | Password for the keystore | Yes | Yes |
| `key_password` | Password for the key alias | Yes | Yes |

## Preparing the `signing_key`

You need to encode your `.jks` or `.keystore` file to Base64 to use it as a GitHub Secret:

```sh
base64 -i my-release-key.jks | pbcopy
# Or on Linux:
base64 -w 0 my-release-key.jks | xclip -selection clipboard
```

Then add this copied string as a Repository Secret named `SIGNING_KEY`.
