# Komodo-op Stack

Komodo stack configuration to deploy [0dragosh/komodo-op](https://github.com/0dragosh/komodo-op), which synchronizes secrets from 1Password to Komodo global variables.

## What is komodo-op?

komodo-op syncs secrets from a 1Password vault to Komodo global variables, making them available to other Komodo stacks without hardcoding secrets.

## Required Environment Variables

These must exist as Komodo global variables before deployment:

- `KOMODO_API_KEY`: Komodo API key
- `KOMODO_API_SECRET`: Komodo API secret  
- `OP_SERVICE_ACCOUNT_TOKEN`: 1Password Connect service account token
- `OP_VAULT_UUID`: 1Password vault UUID to sync from
- `OP_CREDENTIALS_BASE64`: Base64-encoded 1Password Connect credentials file

## Requirements

1. **1Password Account** with Connect server credentials
2. **Komodo Core** instance running
3. **Initial variable setup** - bootstrap required environment variables in Komodo before first deployment

## Files

- `docker-compose.yaml`: Base compose from upstream
- `compose.override.yaml`: Komodo-specific overrides
- `stack.toml`: Komodo deployment configuration