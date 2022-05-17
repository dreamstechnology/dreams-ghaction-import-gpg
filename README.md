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

      - name: Import GPG key
        uses: dreamstechnology/dreams-ghaction-import-gpg@v1
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
