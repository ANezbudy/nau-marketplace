# TRAPCTL Command Reference

T-RAP CLI contains the command line interface implementation for the Telekom Rapid Application Platform.

**Version:** t-rap-cli/1.112.4
**Documentation:** https://docs.trap.prod.tap.telekom.de
**Contact:** trap@telekom.com
**Feedback:** https://vote2.telekom.net/084123

---

## Table of Contents

- [Commands](#commands)
  - [access](#access)
  - [build-log](#build-log)
  - [config](#config)
  - [delete](#delete)
  - [deploy](#deploy)
  - [deployments](#deployments)
  - [environments](#environments)
  - [get](#get)
  - [init](#init)
  - [integrations](#integrations)
  - [list](#list)
  - [login](#login)
  - [logout](#logout)
  - [logs](#logs)
  - [metrics](#metrics)
  - [plan](#plan)
  - [promote](#promote)
  - [token](#token)
  - [update](#update)
  - [whoami](#whoami)

---

## Commands

### access

Grant or revoke access to a group for an existing application.

#### access grant

Grant access to a group for an existing application.

**Usage:**
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

---

#### access revoke

Revoke access from the authorized group for an existing application.

**Usage:**
```bash
trapctl access revoke [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name

**Examples:**
```bash
trapctl access revoke --app my-app
```

---

### build-log

Prints build logs of an application version.

**Usage:**
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

---

### config

Get or modify the current configuration of the application for a given environment.

#### config get

Get the current configuration of the application for a given environment.

**Usage:**
```bash
trapctl config get [-a <value>] [-e <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - Environment name

**Examples:**
```bash
trapctl config get --app my-app
trapctl config get --app my-app -e my-env
```

---

#### config set

Modify configuration of the application for a given environment.

**Usage:**
```bash
trapctl config set [-a <value>] [-e <value>] [-s <value>...] [-d <value>...]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - Environment name
- `-s, --set=<value>...` - Set configuration key-value pairs (format: `KEY:value`)
- `-d, --delete=<value>...` - Delete configuration keys

**Examples:**
```bash
trapctl config set --app my-app --set KEY:value --delete KEY2
trapctl config set --app my-app --environment my-env --set KEY:value --delete KEY2
```

---

### delete

Deletes an application using T-RAP.

**Usage:**
```bash
trapctl delete -a <value> [-f]
```

**Flags:**
- `-a, --app=<value>` - **(Required)** Application name
- `-f, --force` - Force deletion without confirmation

**Examples:**
```bash
trapctl delete --app my-app
```

---

### deploy

Deploys an application using T-RAP.

**Usage:**
```bash
trapctl deploy [-a <value>] [-e <value>] [-p <value>] [-s]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - Environment name
- `-p, --package=<value>` - Package ID
- `-s, --show-failed-build-logs` - Show build logs if deployment fails

**Examples:**
```bash
trapctl deploy --app my-app
trapctl deploy --app my-app --environment uat
trapctl deploy --app my-app --package 123
trapctl deploy --app my-app --show-failed-build-logs
```

---

### deployments

Lists the existing deployments for an application.

**Usage:**
```bash
trapctl deployments [-a <value>] [-e <value>] [--columns <value> | -x] [--filter <value>] [--no-header | [--csv | --no-truncate]] [--output csv|json|yaml] [--sort <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - Environment name
- `-x, --extended` - Show extra columns
- `--columns=<value>` - Only show provided columns (comma-separated)
- `--csv` - Output is csv format (alias: `--output=csv`)
- `--filter=<value>` - Filter property by partial string matching (ex: `name=foo`)
- `--no-header` - Hide table header from output
- `--no-truncate` - Do not truncate output to fit screen
- `--output=<option>` - Output in a more machine friendly format (options: `csv`, `json`, `yaml`)
- `--sort=<value>` - Property to sort by (prepend '-' for descending)

**Examples:**
```bash
trapctl deployments
```

---

### environments

Create, delete, get, or list environments for an application.

#### environments create

Creates a new environment for a given application. It is created in the non-production network segment by default, which can be overridden by a flag.

**Usage:**
```bash
trapctl environments create -e <value> [-a <value>] [--network-segment non-production|production]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - **(Required)** Environment name
- `--network-segment=<option>` - Network segment (options: `non-production`, `production`) [default: `non-production`]

**Examples:**
```bash
trapctl environments create --environment my-env --app my-app --network-segment non-production
trapctl environments create --environment my-env --app my-app --network-segment production
```

---

#### environments delete

Deletes an environment of an application.

**Usage:**
```bash
trapctl environments delete -a <value> -e <value> [-f]
```

**Flags:**
- `-a, --app=<value>` - **(Required)** Application name
- `-e, --environment=<value>` - **(Required)** Environment name
- `-f, --force` - Force deletion without confirmation

**Examples:**
```bash
trapctl environments delete --app my-app --environment my-env
trapctl environments delete --app my-app --environment my-env --force
```

---

#### environments get

Show details of an existing environment of an application.

**Usage:**
```bash
trapctl environments get -e <value> [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - **(Required)** Environment name

**Examples:**
```bash
trapctl environments get --app my-app --environment my-env
```

---

#### environments list

List the existing environments of the application.

**Usage:**
```bash
trapctl environments list [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name

**Examples:**
```bash
trapctl environments list --app my-app
```

---

### get

Show details of an existing application.

**Usage:**
```bash
trapctl get [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name

**Examples:**
```bash
trapctl get --app my-app
```

---

### init

Initializes a new application.

**Usage:**
```bash
trapctl init [-a <value>] [-d]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-d, --deploy` - Deploy after initialization

**Examples:**
```bash
trapctl init --app my-app
```

---

### integrations

Manage integrations for an application.

#### integrations cicd-auth

Show or set CICD Auth Integration for an application.

##### integrations cicd-auth get

Show CICD Auth Integration for an application.

**Usage:**
```bash
trapctl integrations cicd-auth get [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name

**Examples:**
```bash
trapctl integrations cicd-auth get --app my-app
```

---

##### integrations cicd-auth set

Set CICD Auth Integration for an application.

**Usage:**
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

---

#### integrations internet-exposure

Get, set, or delete Internet Exposure Integration for an environment.

##### integrations internet-exposure get

Show Internet Exposure Integration for an environment.

**Usage:**
```bash
trapctl integrations internet-exposure get -e <value> [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - **(Required)** Environment name

**Examples:**
```bash
trapctl integrations internet-exposure get -a my-app -e environment-name
```

---

##### integrations internet-exposure set

Set the Internet Exposure Integration for an environment.

**Usage:**
```bash
trapctl integrations internet-exposure set -e <value> [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - **(Required)** Environment name

**Examples:**
```bash
trapctl integrations internet-exposure set -a app-name -e environment-name
```

---

##### integrations internet-exposure delete

Removes Internet Exposure Integration from given environment.

**Usage:**
```bash
trapctl integrations internet-exposure delete -e <value> [-a <value>] [-f]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - **(Required)** Environment name
- `-f, --force` - Force deletion without confirmation

**Examples:**
```bash
trapctl integrations internet-exposure delete --app my-app --environment my-env
trapctl integrations internet-exposure delete --app my-app --environment my-env --force
```

---

#### integrations oauth-proxy

Get, set, or delete OAuth Proxy Integration per environment.

##### integrations oauth-proxy get

Show OAuth Proxy Integration for an environment.

**Usage:**
```bash
trapctl integrations oauth-proxy get -e <value> [-a <value>] [--show-client-secret]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - **(Required)** Environment name
- `--show-client-secret` - Show client secret in output

**Examples:**
```bash
trapctl integrations oauth-proxy get -a my-app -e environment-name
trapctl integrations oauth-proxy get -a my-app -e environment-name --show-client-secret
```

---

##### integrations oauth-proxy set

Set the OAuth Proxy Integration for an environment.

**Usage:**
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
trapctl integrations oauth-proxy set -a app-name -e environment-name --client-id my-client-id
trapctl integrations oauth-proxy set -a app-name -e environment-name --client-id my-client-id --prompt-client-secret
trapctl integrations oauth-proxy set -a app-name -e environment-name --client-id my-client-id --issuer-url https://example.com/oauth
```

---

##### integrations oauth-proxy delete

Removes OAuth Proxy Integration from given environment.

**Usage:**
```bash
trapctl integrations oauth-proxy delete -e <value> [-a <value>] [-f]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - **(Required)** Environment name
- `-f, --force` - Force deletion without confirmation

**Examples:**
```bash
trapctl integrations oauth-proxy delete --app my-app --environment my-env
trapctl integrations oauth-proxy delete --app my-app --environment my-env --force
```

---

### list

Lists the existing applications.

**Usage:**
```bash
trapctl list
```

**Examples:**
```bash
trapctl list
```

---

### login

Logins through the configured Identity Provider.

**Usage:**
```bash
trapctl login
```

**Examples:**
```bash
trapctl login
```

---

### logout

Logout through the configured Identity Provider.

**Usage:**
```bash
trapctl logout
```

**Examples:**
```bash
trapctl logout
```

---

### logs

Shows the logs of a running application.

**Usage:**
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
trapctl logs --app my-app --environment my-env --since-seconds 60
```

---

### metrics

Show metrics of an application.

**Usage:**
```bash
trapctl metrics [-a <value>] [-e <value>] [-p] [-s <value>] [-m cpu|memory|network]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-e, --environment=<value>` - Environment name
- `-m, --metric=<option>` - Metric type (options: `cpu`, `memory`, `network`)
- `-p, --per-instance` - Show metrics per instance
- `-s, --since-hours=<value>` - Show metrics from the last N hours [default: `1`]

**Examples:**
```bash
trapctl metrics --app my-app --environment production
trapctl metrics --app my-app --environment production --per-instance --metric network --since-hours 4
```

---

### plan

Prints the deploy plan of an application.

**Usage:**
```bash
trapctl plan [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name

**Examples:**
```bash
trapctl plan --app my-app
```

---

### promote

Promotes the version of one environment to another.

**Usage:**
```bash
trapctl promote -s <value> -t <value> [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `-s, --source-environment=<value>` - **(Required)** Source environment name
- `-t, --target-environment=<value>` - **(Required)** Target environment name

**Examples:**
```bash
trapctl promote --app my-app --source-environment development --target-environment production
trapctl promote -a my-app -s development -t production
```

---

### token

Get, list, generate, or regenerate authentication tokens for an application.

#### token get

Get the existing token.

**Usage:**
```bash
trapctl token get [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name

**Aliases:**
- `trapctl token list`

**Examples:**
```bash
trapctl token get --app my-app
```

---

#### token list

Get the existing token (same as `token get`).

**Usage:**
```bash
trapctl token list [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name

**Aliases:**
- `trapctl token list`

**Examples:**
```bash
trapctl token get --app my-app
```

---

#### token generate

Regenerate an authentication token for an application.

**Usage:**
```bash
trapctl token generate [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name

**Aliases:**
- `trapctl token generate`

**Examples:**
```bash
trapctl token regenerate --app my-app
```

---

#### token regenerate

Regenerate an authentication token for an application (same as `token generate`).

**Usage:**
```bash
trapctl token regenerate [-a <value>]
```

**Flags:**
- `-a, --app=<value>` - Application name

**Aliases:**
- `trapctl token generate`

**Examples:**
```bash
trapctl token regenerate --app my-app
```

---

### update

Update application settings.

**Usage:**
```bash
trapctl update [-a <value>] [--routing-by-client-ip true|false]
```

**Flags:**
- `-a, --app=<value>` - Application name
- `--routing-by-client-ip=<option>` - Enable or disable routing by client IP (options: `true`, `false`)

**Examples:**
```bash
trapctl update --app my-app --routing-by-client-ip true
trapctl update --app my-app --routing-by-client-ip false
```

---

### whoami

Prints who is the currently logged in user.

**Usage:**
```bash
trapctl whoami
```

**Examples:**
```bash
trapctl whoami
```

---

## Common Flags

Most commands support the following common flag:

- `-a, --app=<value>` - Application name (can be omitted if working in a directory with a `trap.json` file)

---

## Additional Resources

- **Documentation:** https://docs.trap.prod.tap.telekom.de
- **Support:** trap@telekom.com
- **Feedback:** https://vote2.telekom.net/084123
- **Help:** Run `trapctl help` or `trapctl [COMMAND] --help` for more information

---

## Notes

1. Many commands support the `-a, --app` flag which can be omitted if you're working in a directory that contains a `trap.json` file.
2. Commands with `-f, --force` flags will skip confirmation prompts.
3. For token commands, `get` and `list` are aliases, as are `generate` and `regenerate`.
4. The `deployments` command supports extensive formatting options for machine-readable output.
5. Integration commands are scoped per environment except for CICD auth which is application-wide.
