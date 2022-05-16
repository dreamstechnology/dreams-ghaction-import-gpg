## About

GitHub Action to import a GPG key

- Written in bash as a composite Github Action, which means fewer dependencies
- Uses [GnuPG](https://gnupg.org/)
- Uses `gpg-agent` to preset and cache the passphrase

Inspired by <https://github.com/crazy-max/ghaction-import-gpg>

## Usage

### Sign git commit

```yaml
name: Sign commit

on: [push]

jobs:
  sign-commit:
    runs-on: ubuntu-20.04
    steps:
      - name: Checkout repo
        uses: actions/checkout@v2

      # Need to checkout the action repo like this because it's a private repo
      - name: Checkout action from private repo
        uses: actions/checkout@v2
        with:
          repository: dreamstechnology/dreams-ghaction-import-gpg
          ref: refs/tags/v0.1
          token: ${{ secrets.GITHUB_TOKEN }}
          path: ./.github/actions/dreams-ghaction-import-gpg

      - name: Import GPG key
        uses: ./.github/actions/dreams-ghaction-import-gpg
        with:
          private-key: ${{ secrets.GPG_PRIVATE_KEY }}
          passphrase: ${{ secrets.GPG_KEY_PASS }}

      - name: Git sign commit
        shell: bash
        run: |
          echo Hej > foo.txt
          git add .
          git commit -S -m "✍️ and 🦭"
```

## Action Inputs

Following inputs can be used as `step.with` keys

| Name          | Type   | Description                                          | Required |
| ------------- | ------ | ---------------------------------------------------- | -------- |
| `private-key` | String | GPG private key exported as an ASCII armored version | Yes      |
| `passphrase`  | String | Passphrase for the key (in ASCII)                    | Yes      |
