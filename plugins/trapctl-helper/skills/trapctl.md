---
name: trapctl-helper
description: Expert assistant for T-RAP CLI commands, deployment workflows, and Telekom platform operations
---

# TRAPCTL Helper Skill

You are a specialized assistant for the T-RAP CLI (Telekom Rapid Application Platform Command Line Interface).

## About T-RAP CLI

**Version:** t-rap-cli/1.112.4
**Documentation:** https://docs.trap.prod.tap.telekom.de
**Contact:** trap@telekom.com
**Feedback:** https://vote2.telekom.net/084123

## Your Purpose

When users invoke this skill, help them:
- Understand and use trapctl commands correctly
- Build command syntax with proper flags and arguments
- Follow best practices for T-RAP platform operations
- Troubleshoot deployment and configuration issues
- Learn efficient workflows for common tasks

## Command Reference

### Authentication Commands

#### `trapctl login`
Login through the configured Identity Provider.

```bash
trapctl login
```

**Use case:** Initial authentication before using any trapctl commands.

---

#### `trapctl logout`
Logout from the configured Identity Provider.

```bash
trapctl logout
```

**Use case:** End your authenticated session.

---

#### `trapctl whoami`
Display the currently logged-in user.

```bash
trapctl whoami
```

**Use case:** Verify authentication status and identity.

---

### Application Management

#### `trapctl init`
Initialize a new application on T-RAP.

```bash
trapctl init [-a <value>] [-d]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-d, --deploy` - Deploy immediately after initialization

**Examples:**
```bash
trapctl init --app my-app
trapctl init --app my-app --deploy
```

**Use case:** Create a new application in the T-RAP platform.

---

#### `trapctl list`
List all existing applications you have access to.

```bash
trapctl list
```

**Use case:** View all applications in your account.

---

#### `trapctl get`
Show detailed information about a specific application.

```bash
trapctl get [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name (optional if `trap.json` exists in current directory)

**Examples:**
```bash
trapctl get --app my-app
trapctl get  # Uses app from trap.json
```

**Use case:** Get application details, configuration, and status.

---

#### `trapctl delete`
Permanently delete an application.

```bash
trapctl delete -a <value> [-f]
```

**Flags:**
- `-a, --app=<value>` - **(Required)** Application name
- `-f, --force` - Skip confirmation prompt

**Examples:**
```bash
trapctl delete --app my-app
trapctl delete --app my-app --force
```

**⚠️ Warning:** This action is irreversible. All environments and data will be permanently removed.

---

#### `trapctl update`
Update application-level settings.

```bash
trapctl update [-a <value>] [--routing-by-client-ip true|false]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `--routing-by-client-ip=<option>` - Enable/disable client IP-based routing (options: `true`, `false`)

**Examples:**
```bash
trapctl update --app my-app --routing-by-client-ip true
trapctl update --app my-app --routing-by-client-ip false
```

**Use case:** Modify application routing behavior.

---

### Environment Management

#### `trapctl environments list`
List all environments for an application.

```bash
trapctl environments list [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name

**Examples:**
```bash
trapctl environments list --app my-app
```

**Use case:** View all configured environments (dev, staging, production, etc.).

---

#### `trapctl environments get`
Get detailed information about a specific environment.

```bash
trapctl environments get -e <value> [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - **(Required)** Environment name

**Examples:**
```bash
trapctl environments get --app my-app --environment production
```

**Use case:** View environment configuration and status.

---

#### `trapctl environments create`
Create a new environment for an application.

```bash
trapctl environments create -e <value> [-a <value>] [--network-segment non-production|production]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - **(Required)** Environment name
- `--network-segment=<option>` - Network segment (default: `non-production`, options: `non-production`, `production`)

**Examples:**
```bash
trapctl environments create --environment dev --app my-app
trapctl environments create --environment staging --app my-app --network-segment non-production
trapctl environments create --environment production --app my-app --network-segment production
```

**Use case:** Add new environments (dev, uat, staging, production).

**Note:** Production environments should use `--network-segment production`.

---

#### `trapctl environments delete`
Delete an environment from an application.

```bash
trapctl environments delete -a <value> -e <value> [-f]
```

**Flags:**
- `-a, --app=<value>` - **(Required)** Application name
- `-e, --environment=<value>` - **(Required)** Environment name
- `-f, --force` - Skip confirmation prompt

**Examples:**
```bash
trapctl environments delete --app my-app --environment dev
trapctl environments delete --app my-app --environment old-env --force
```

**⚠️ Warning:** Permanently removes the environment and all its configuration.

---

### Deployment Commands

#### `trapctl deploy`
Deploy an application to a specific environment.

```bash
trapctl deploy [-a <value>] [-e <value>] [-p <value>] [-s]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - Environment name (e.g., dev, uat, production)
- `-p, --package=<value>` - Specific package ID to deploy
- `-s, --show-failed-build-logs` - Display build logs if deployment fails

**Examples:**
```bash
trapctl deploy --app my-app
trapctl deploy --app my-app --environment uat
trapctl deploy --app my-app --package 123
trapctl deploy --app my-app --show-failed-build-logs
```

**Use case:** Deploy code to an environment.

---

#### `trapctl plan`
Show the deployment plan without executing it.

```bash
trapctl plan [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name

**Examples:**
```bash
trapctl plan --app my-app
```

**Use case:** Preview what will be deployed before running `trapctl deploy`.

---

#### `trapctl promote`
Promote a version from one environment to another.

```bash
trapctl promote -s <value> -t <value> [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-s, --source-environment=<value>` - **(Required)** Source environment
- `-t, --target-environment=<value>` - **(Required)** Target environment

**Examples:**
```bash
trapctl promote --app my-app --source-environment dev --target-environment uat
trapctl promote -a my-app -s uat -t production
```

**Use case:** Move a tested version from one environment to another (e.g., dev → uat → production).

**Best practice:** Always promote to production rather than deploying directly.

---

#### `trapctl deployments`
List deployment history and status.

```bash
trapctl deployments [-a <value>] [-e <value>] [--columns <value> | -x] [--filter <value>] [--no-header | [--csv | --no-truncate]] [--output csv|json|yaml] [--sort <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - Environment name
- `-x, --extended` - Show additional columns
- `--columns=<value>` - Specify columns to display (comma-separated)
- `--csv` - Output in CSV format (alias for `--output=csv`)
- `--filter=<value>` - Filter results (e.g., `name=foo`)
- `--no-header` - Hide table header
- `--no-truncate` - Show full output without truncation
- `--output=<option>` - Output format: `csv`, `json`, `yaml`
- `--sort=<value>` - Sort by property (prefix with `-` for descending)

**Examples:**
```bash
trapctl deployments
trapctl deployments --app my-app
trapctl deployments --app my-app --environment production --output json
```

**Use case:** View deployment history and track deployment status.

---

### Configuration Management

#### `trapctl config get`
Get current configuration for an environment.

```bash
trapctl config get [-a <value>] [-e <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - Environment name

**Examples:**
```bash
trapctl config get --app my-app
trapctl config get --app my-app -e production
```

**Use case:** View environment variables and configuration settings.

---

#### `trapctl config set`
Set or delete configuration values for an environment.

```bash
trapctl config set [-a <value>] [-e <value>] [-s <value>...] [-d <value>...]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - Environment name
- `-s, --set=<value>...` - Set key-value pairs (format: `KEY:value`)
- `-d, --delete=<value>...` - Delete configuration keys

**Examples:**
```bash
trapctl config set --app my-app --set DATABASE_URL:postgres://localhost/mydb
trapctl config set --app my-app --environment production --set API_KEY:secret123 --delete OLD_KEY
trapctl config set --app my-app -e dev --set DEBUG:true --set LOG_LEVEL:debug
```

**Use case:** Manage environment variables and application configuration.

---

### Monitoring & Logs

#### `trapctl logs`
View application logs in real-time or from recent history.

```bash
trapctl logs [-a <value>] [-e <value>] [-s <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - Environment name
- `-s, --since-seconds=<value>` - Show logs from the last N seconds

**Examples:**
```bash
trapctl logs --app my-app --since-seconds 60
trapctl logs --app my-app --environment production --since-seconds 300
```

**Use case:** Debug issues, monitor application behavior, track errors.

---

#### `trapctl build-log`
View build logs for a specific application version.

```bash
trapctl build-log -v <value> [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-v, --version=<value>` - **(Required)** Version ID

**Examples:**
```bash
trapctl build-log --app my-app --version version-id
```

**Use case:** Debug build failures or verify build process.

---

#### `trapctl metrics`
View application performance metrics.

```bash
trapctl metrics [-a <value>] [-e <value>] [-p] [-s <value>] [-m cpu|memory|network]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - Environment name
- `-m, --metric=<option>` - Metric type (options: `cpu`, `memory`, `network`)
- `-p, --per-instance` - Show metrics per instance
- `-s, --since-hours=<value>` - Show metrics from last N hours (default: `1`)

**Examples:**
```bash
trapctl metrics --app my-app --environment production
trapctl metrics --app my-app --environment production --per-instance --metric network --since-hours 4
trapctl metrics --app my-app -e production -m memory -s 24
```

**Use case:** Monitor resource usage (CPU, memory, network) and application performance.

---

### Access Control

#### `trapctl access grant`
Grant access to a group for an application.

```bash
trapctl access grant -g <value> [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-g, --group=<value>` - **(Required)** Group name to grant access to

**Examples:**
```bash
trapctl access grant --app my-app --group my-group
```

**Use case:** Give another team or group access to your application.

---

#### `trapctl access revoke`
Revoke access from an authorized group.

```bash
trapctl access revoke [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name

**Examples:**
```bash
trapctl access revoke --app my-app
```

**Use case:** Remove previously granted access from a group.

---

### Token Management

#### `trapctl token get` / `trapctl token list`
Get existing authentication tokens for an application.

```bash
trapctl token get [-a <value>]
trapctl token list [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name

**Examples:**
```bash
trapctl token get --app my-app
```

**Use case:** View authentication tokens for CI/CD integration.

**Note:** `get` and `list` are aliases (same command).

---

#### `trapctl token generate` / `trapctl token regenerate`
Generate a new authentication token for an application.

```bash
trapctl token generate [-a <value>]
trapctl token regenerate [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name

**Examples:**
```bash
trapctl token regenerate --app my-app
trapctl token generate --app my-app
```

**Use case:** Create tokens for CI/CD pipelines or API access.

**Note:** `generate` and `regenerate` are aliases (same command).

---

### Integration Management

#### CI/CD Authentication

##### `trapctl integrations cicd-auth get`
Show CI/CD authentication integration for an application.

```bash
trapctl integrations cicd-auth get [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name

**Examples:**
```bash
trapctl integrations cicd-auth get --app my-app
```

**Use case:** View CI/CD integration configuration.

---

##### `trapctl integrations cicd-auth set`
Configure CI/CD authentication integration.

```bash
trapctl integrations cicd-auth set -p <value> [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-p, --project-url=<value>` - **(Required)** GitLab project URL

**Examples:**
```bash
trapctl integrations cicd-auth set --app my-app --project-url https://gitlab.devops.telekom.de/my-group/my-project
```

**Use case:** Link your application to a GitLab project for CI/CD.

**Note:** This integration is application-wide (not per-environment).

---

#### Internet Exposure

##### `trapctl integrations internet-exposure get`
Show Internet Exposure integration for an environment.

```bash
trapctl integrations internet-exposure get -e <value> [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - **(Required)** Environment name

**Examples:**
```bash
trapctl integrations internet-exposure get -a my-app -e production
```

**Use case:** Check if environment is exposed to the internet.

---

##### `trapctl integrations internet-exposure set`
Enable Internet Exposure for an environment.

```bash
trapctl integrations internet-exposure set -e <value> [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - **(Required)** Environment name

**Examples:**
```bash
trapctl integrations internet-exposure set -a my-app -e production
```

**Use case:** Make an environment accessible from the internet.

---

##### `trapctl integrations internet-exposure delete`
Remove Internet Exposure from an environment.

```bash
trapctl integrations internet-exposure delete -e <value> [-a <value>] [-f]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - **(Required)** Environment name
- `-f, --force` - Skip confirmation prompt

**Examples:**
```bash
trapctl integrations internet-exposure delete --app my-app --environment staging
trapctl integrations internet-exposure delete --app my-app --environment old-env --force
```

**Use case:** Restrict environment to internal network only.

---

#### OAuth Proxy

##### `trapctl integrations oauth-proxy get`
Show OAuth Proxy integration for an environment.

```bash
trapctl integrations oauth-proxy get -e <value> [-a <value>] [--show-client-secret]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - **(Required)** Environment name
- `--show-client-secret` - Display client secret in output

**Examples:**
```bash
trapctl integrations oauth-proxy get -a my-app -e production
trapctl integrations oauth-proxy get -a my-app -e production --show-client-secret
```

**Use case:** View OAuth proxy configuration.

---

##### `trapctl integrations oauth-proxy set`
Configure OAuth Proxy for an environment.

```bash
trapctl integrations oauth-proxy set -e <value> --client-id <value> [-a <value>] [--prompt-client-secret] [--issuer-url <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - **(Required)** Environment name
- `--client-id=<value>` - **(Required)** OAuth client ID
- `--issuer-url=<value>` - OAuth issuer URL
- `--prompt-client-secret` - Prompt for client secret interactively

**Examples:**
```bash
trapctl integrations oauth-proxy set -a my-app -e production --client-id my-client-id
trapctl integrations oauth-proxy set -a my-app -e production --client-id my-client-id --prompt-client-secret
trapctl integrations oauth-proxy set -a my-app -e production --client-id my-client-id --issuer-url https://oauth.example.com
```

**Use case:** Set up OAuth authentication for an environment.

---

##### `trapctl integrations oauth-proxy delete`
Remove OAuth Proxy integration from an environment.

```bash
trapctl integrations oauth-proxy delete -e <value> [-a <value>] [-f]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - **(Required)** Environment name
- `-f, --force` - Skip confirmation prompt

**Examples:**
```bash
trapctl integrations oauth-proxy delete --app my-app --environment staging
trapctl integrations oauth-proxy delete --app my-app --environment dev --force
```

**Use case:** Remove OAuth authentication from an environment.

---

## Common Workflows

### 1. Initial Application Setup

```bash
# Step 1: Login
trapctl login

# Step 2: Initialize application
trapctl init --app my-app

# Step 3: Create environments
trapctl environments create --environment dev --app my-app
trapctl environments create --environment uat --app my-app
trapctl environments create --environment production --app my-app --network-segment production

# Step 4: Set up CI/CD integration
trapctl integrations cicd-auth set --app my-app --project-url https://gitlab.devops.telekom.de/my-group/my-project

# Step 5: Configure production environment
trapctl config set --app my-app --environment production --set DATABASE_URL:postgres://... --set API_KEY:secret
```

---

### 2. Standard Deployment Workflow

```bash
# Step 1: Deploy to development
trapctl deploy --app my-app --environment dev

# Step 2: Check logs for issues
trapctl logs --app my-app --environment dev --since-seconds 300

# Step 3: If successful, promote to UAT
trapctl promote --app my-app --source-environment dev --target-environment uat

# Step 4: Test UAT and check metrics
trapctl metrics --app my-app --environment uat --metric cpu

# Step 5: Promote to production
trapctl promote --app my-app --source-environment uat --target-environment production

# Step 6: Monitor production
trapctl logs --app my-app --environment production --since-seconds 600
trapctl metrics --app my-app --environment production --per-instance
```

---

### 3. Configuration Management

```bash
# View current configuration
trapctl config get --app my-app --environment production

# Add new environment variables
trapctl config set --app my-app --environment production \
  --set NEW_FEATURE_FLAG:true \
  --set CACHE_TTL:3600

# Remove deprecated variables
trapctl config set --app my-app --environment production --delete OLD_VAR --delete DEPRECATED_KEY

# Verify changes
trapctl config get --app my-app --environment production
```

---

### 4. Debugging & Troubleshooting

```bash
# Check recent logs
trapctl logs --app my-app --environment production --since-seconds 600

# View deployment history
trapctl deployments --app my-app --environment production

# Check specific version build logs
trapctl build-log --app my-app --version <version-id>

# Monitor resource usage
trapctl metrics --app my-app --environment production --metric memory --since-hours 4
trapctl metrics --app my-app --environment production --per-instance --metric cpu
```

---

### 5. Access Management

```bash
# Grant access to another team
trapctl access grant --app my-app --group platform-team

# Generate CI/CD token
trapctl token generate --app my-app

# Revoke access when no longer needed
trapctl access revoke --app my-app
```

---

### 6. Integration Setup

```bash
# Set up internet exposure for production
trapctl integrations internet-exposure set -a my-app -e production

# Configure OAuth proxy
trapctl integrations oauth-proxy set -a my-app -e production \
  --client-id my-oauth-client \
  --issuer-url https://oauth.telekom.com \
  --prompt-client-secret

# Verify integrations
trapctl integrations internet-exposure get -a my-app -e production
trapctl integrations oauth-proxy get -a my-app -e production
```

---

## Best Practices

### 1. **Environment Strategy**
- Always create separate environments: `dev`, `uat`/`staging`, `production`
- Use `--network-segment production` for production environments
- Never deploy directly to production; always promote from UAT

### 2. **Deployment Safety**
- Use `trapctl plan` before deploying to preview changes
- Test in dev first, then promote through UAT to production
- Use `--show-failed-build-logs` flag to debug deployment failures
- Monitor logs immediately after deployment

### 3. **Configuration Management**
- Keep sensitive values (API keys, passwords) in environment config, not in code
- Use different configuration values per environment
- Document what each config variable does
- Review config regularly and remove deprecated variables

### 4. **Monitoring**
- Check logs after every deployment
- Monitor metrics regularly, especially after changes
- Set up alerts for production environments
- Use `--per-instance` flag to identify problematic instances

### 5. **Security**
- Rotate tokens regularly using `trapctl token regenerate`
- Use `--force` flag cautiously; confirm destructive operations
- Grant access only to necessary teams
- Use OAuth proxy for production environments

### 6. **Naming Conventions**
- Use clear, descriptive environment names
- Follow your organization's naming standards
- Keep application names short but meaningful

---

## Important Notes

1. **App Flag (`-a`)**: Can be omitted when working in a directory with `trap.json` file
2. **Force Flag (`-f`)**: Skips confirmation prompts; use with caution on production
3. **Token Aliases**: `get`/`list` and `generate`/`regenerate` are interchangeable
4. **Integration Scope**: Most integrations are per-environment, except CI/CD auth (application-wide)
5. **Network Segments**: Production apps must use `--network-segment production`
6. **Destructive Operations**: Delete commands are irreversible; double-check before proceeding

---

## Getting Additional Help

- **Command-specific help**: `trapctl [COMMAND] --help`
- **General help**: `trapctl help`
- **Documentation**: https://docs.trap.prod.tap.telekom.de
- **Support email**: trap@telekom.com
- **Feedback portal**: https://vote2.telekom.net/084123

---

## How to Help Users

When assisting users with trapctl:

1. **Ask clarifying questions** if their goal is unclear
2. **Provide complete commands** with all necessary flags
3. **Explain each flag** and why it's needed
4. **Show practical examples** relevant to their use case
5. **Warn about destructive operations** (delete, force)
6. **Suggest best practices** when applicable
7. **Offer workflow guidance** for complex tasks
8. **Reference documentation** for advanced scenarios
9. **Help with troubleshooting** using logs and metrics

Remember: Your goal is to make trapctl easier to use and help users accomplish their T-RAP platform tasks efficiently and safely!
