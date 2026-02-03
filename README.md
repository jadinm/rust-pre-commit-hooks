# Rust Pre-Commit hooks

This repository contains a list of pre-commit hooks for rust.

Check the following repositories for hooks using:

- https://github.com/EmbarkStudios/cargo-deny for cargo deny
- https://github.com/doublify/pre-commit-rust for cargo fmt, cargo clippy & cargo check

## Available hooks

### `cargo-audit`

```yaml
- repo: https://github.com/jadinm/rust-pre-commit-hooks
  rev: v1.1.0
  hooks:
  - id: cargo-audit
```

### `cbindgen-verify`

For a repo with a single package:

```yaml
- repo: https://github.com/jadinm/rust-pre-commit-hooks
  rev: v1.1.0
  hooks:
  - id: cbindgen-verify
    args:
    - --verify
    - --output
    - <crate_name>.h
```

For a repo with a workspace containing multiple packages, for each package, you'll need a hook:

```yaml
- repo: https://github.com/jadinm/rust-pre-commit-hooks
  rev: v1.0.0
  hooks:
  - id: cbindgen-verify
    args:
    - --verify
    - --package
    - /path/to/package/cbindgen.toml
    - --crate
    - /path/to/package
    - --output
    - /path/to/package/<crate_name>.h
```

## Setting Up Dev Environment

### Pre-commit Installation

This repository uses [`pre-commit`](https://pre-commit.com/) to check code.
This requires an additional manuel step.

There are two possible ways of enabling pre-commit, either only for this repository or user-wide.

If you go for a per-repositoy install, run here:

```shell
pre-commit install
```

If you think that running `pre-commit install` on each of your projects is tedious, there is an alterantive.
You can enable the pre-commit hooks user-wide.

For that, we use [Git Template Directory](https://git-scm.com/docs/git-init#_template_directory) that stores files to be copied over any newly cloned or initilized repository.
You need to choose a folder that will hold these scripts:

```shell
git config --global init.templateDir /path/to/chosen/template/directory
```

Then, we ask pre-commit to generate a pre-commit hook for that template directory:

```shell
pre-commit init-templatedir /path/to/chosen/template/directory
```

From now on, every newly cloned or initialized repository will have a hook bash scripts `.git/hooks/pre-commit`.
This script executes pre-commit software if any hook is configured in a `.pre-commit-config.yaml`.
If there is no hook defined, the commits are always accepted.
