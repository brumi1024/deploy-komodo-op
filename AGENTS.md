# AGENTS.md

Komodo stack for `0dragosh/komodo-op`, which copies a 1Password vault into Komodo global variables.
`docker-compose.yaml` is upstream unchanged; local changes belong in `compose.override.yaml` and `stack.toml`.

- The bootstrap variables in `README.md` are created by `brumi1024/homelab-infra` (`05_bootstrap_komodo_op.yml`); this stack only consumes them.
- The sync that deploys this repo is defined in `brumi1024/komodo-resource-syncs`.
- Consumers reference synced values as `[[OP__KOMODO__<STACK>__<NAME>]]`, see `brumi1024/komodo-app-stacks`.
