# Installation On Local Environment

This guide installs the full local stack (master, slave, worker) on the same machine using your `BlackOpsFile`.

## Prerequisites

1. You have the repositories under `~/code1`.
2. The `saas` CLI is available in your shell.
3. The file `~/code1/secret/local-ubuntu-22.04/BlackOpsFile` exists and defines:
	- `localmaster`
	- `localslave`
	- `localworker`
4. You can run commands as root using the password exported in `MYSAAS_ROOT_PASSWORD`.

## Install, Deploy, and Migrate

Run this exact command:

```bash
export MYSAAS_ROOT_PASSWORD=<password of root> && \
export MYSAAS_PASSWORD=<password of current user> && \
export OPSLIB=~/code1/secret/local-ubuntu-22.04 && \
saas install --node=localmaster --root && \
saas deploy --node=localmaster && \
saas deploy --node=localslave && \
saas deploy --node=localworker && \
saas migrations --node=localmaster && \
saas migrations --node=localslave
```

## What This Does

1. Exports SSH/root credentials and points `OPSLIB` to your local config.
2. Installs base dependencies and services on `localmaster`.
3. Deploys code for `localmaster`, `localslave`, and `localworker`.
4. Runs database migrations for `localmaster` and `localslave`.

## Quick Validation

After the script finishes, run:

```bash
saas list
```

Optional checks:

```bash
systemctl status postgresql
ls ~/code1/master/.sandbox
```

If you get `Node not found`, verify `OPSLIB` points to the folder that contains your `BlackOpsFile`.
