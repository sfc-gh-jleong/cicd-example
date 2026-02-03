---
name: "fix-snowflake-cli-config"
created: "2026-02-03T16:23:50.316Z"
status: pending
---

# Plan: Fix Snowflake CLI Connection Error

## Problem

The Snowflake CLI (`snow`) requires a config file at `~/.snowflake/config.toml` - environment variables alone are not sufficient to configure the "default" connection.

## Solution

Add a step in the workflow to dynamically create the config file before running SQL commands.

## Changes to .github/workflows/deploy.yml

Add this step **after** "Install Snowflake CLI" and **before** "Deploy Database Setup":

```
      - name: Configure Snowflake CLI
        run: |
          mkdir -p ~/.snowflake
          cat > ~/.snowflake/config.toml << EOF
          [connections.default]
          account = "${{ secrets.SNOWFLAKE_ACCOUNT }}"
          user = "${{ secrets.SNOWFLAKE_USER }}"
          password = "${{ secrets.SNOWFLAKE_PASSWORD }}"
          warehouse = "${{ secrets.SNOWFLAKE_WAREHOUSE }}"
          EOF
```

The `env` block with `SNOWFLAKE_CONNECTIONS_DEFAULT_*` variables can be removed as they are no longer needed.
