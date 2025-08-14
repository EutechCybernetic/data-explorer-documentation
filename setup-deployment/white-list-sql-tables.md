# Whitelist SQL Tables

Only explicitly whitelisted database tables are available in Data Explorer. Only tables with fixed and stable structures should be whitelisted.

## Configuration Steps

### Step 1: Locate the Query Engine Service

The `lucy-query-engine` service typically runs in a Docker container named `iviva-queryengine`.

### Step 2: Configure Environment Variable

Add the `SQL_TABLE_WHITELIST` environment variable with a comma-separated list of table names.

**Example:**

```bash
SQL_TABLE_WHITELIST=AssetMaster,UserView,LocationView
```

**For Docker deployments:** Add this line to your `.env` file in the docker-compose directory:

```yaml
SQL_TABLE_WHITELIST=AssetMaster,UserView,LocationView
```

### Step 3: Restart the Service

Apply changes by restarting the service:

```bash
sudo docker compose -f /path/to/docker-compose.yml --env-file /path/to/.env --profile queryengine up -d
```

The whitelisted tables will now appear as available sources in Data Explorer.
