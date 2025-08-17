# Komodo-op Stack

This repository contains the Komodo stack configuration for deploying [komodo-op](https://github.com/0dragosh/komodo-op), which synchronizes secrets from 1Password to Komodo global variables.

## Overview

komodo-op is a prerequisite infrastructure component that:
- Runs 1Password Connect server for secure secret access
- Synchronizes secrets from 1Password vault to Komodo
- Enables other stacks to use secrets via Komodo variables

## Architecture

```
1Password Vault → 1Password Connect → komodo-op → Komodo Global Variables → Application Stacks
```

## Files

- `docker-compose.yaml`: Base compose file from upstream komodo-op
- `compose.override.yaml`: Environment-specific overrides for Komodo deployment
- `stack.toml`: Komodo stack and resource sync configuration
- `renovate.json`: Automatic dependency updates from upstream

## Prerequisites

1. **1Password Connect Setup**:
   - Generate Connect server credentials in 1Password admin
   - Create service account token
   - Store credentials in 1Password

2. **Komodo API Credentials**:
   - Generate API key/secret in Komodo Core UI
   - Store in 1Password

## Deployment

This stack is deployed via Komodo resource sync. The `stack.toml` file defines:

- **Stack**: Deploys komodo-op services to `home-server`
- **Resource Sync**: Monitors this repository for changes

### Environment Variables

The stack uses these Komodo global variables:
- `KOMODO_API_KEY`: API key for Komodo authentication
- `KOMODO_API_SECRET`: API secret for Komodo authentication
- `OP_SERVICE_ACCOUNT_TOKEN`: 1Password Connect service account token
- `OP_VAULT_UUID`: UUID of 1Password vault to sync

### Initial Bootstrap Required

**⚠️ Important**: Before first deployment, these variables must exist in Komodo global variables.

#### Manual Setup
1. Access Komodo UI → Variables
2. Add each variable

This creates all required variables by reading from 1Password and using the Komodo API.

### Networking

komodo-op connects to Komodo Core using `host.docker.internal:9120` since both services run on the same Docker host. This approach:
- Eliminates the need to store the Komodo host URL in 1Password
- Automatically works regardless of hostname changes
- Uses Docker's built-in host networking capability

## Deployment Order

1. **Bootstrap variables** (first time only): `./scripts/deploy.sh bootstrap-komodo-op`
2. **Deploy komodo-op** (this stack) via Komodo resource sync
3. **Wait for secret sync** (30-60 seconds)
4. **Deploy application stacks** that depend on synced secrets

## Monitoring

- **Logs**: Check komodo-op container logs for sync status
- **Health**: 1Password Connect API available at `http://localhost:8080/health`
- **Sync Status**: Monitor Komodo global variables for new secrets

## Upstream Tracking

Renovate automatically monitors:
- `0dragosh/komodo-op` repository for new releases
- Docker images for security updates
- Creates PRs for review before merging changes