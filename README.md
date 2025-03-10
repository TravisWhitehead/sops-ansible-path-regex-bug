# Reproducing `community.sops.sops_encrypt` (Ansible) bug w/ `path_regex`

## Setup

Optionally, you can use `mise` to setup an environment with the same versions as me.

See `.mise.toml` config.

```sh
# examine the config
cat .mise.toml
# install the specified dependencies
mise trust
mise install

# activate the environment in this shell
eval "$(mise activate)"

# install Python deps (ansible) in venv and community.sops
mise run deps
```

## Run Ansible

```sh
ansible-playbook playbook.yaml -vvv
```

## `.sops.yaml` with `path_regex`
Take a look at the `path_regex` rules in `.sops.yaml`

```sh
cat .sops.yaml
```

```yaml
---
creation_rules:
  # this fails because it tries to match on part of the path (foobar/)
  - path_regex: output/examplesecret\.sops\.yaml
    key_groups:
      - age:
          - "age187022d9nqqe79qaphrn99stjdyt26kchm539s5w2mljdtw0p7cvq4xwreu"
  # this works because it is only matching on the filename
  # - path_regex: examplesecret\.sops\.yaml
  #   key_groups:
  #     - age:
  #         - "age187022d9nqqe79qaphrn99stjdyt26kchm539s5w2mljdtw0p7cvq4xwreu"
```
