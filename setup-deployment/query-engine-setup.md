# Query Engine Setup

The Query Engine executes data pipelines and must be configured before using Data Explorer.

## Environment Variables

| Variable | Description | Required | Example |
|----------|-------------|----------|---------|
| `DB` | ASP database connection string | Yes | `user id=sa;pwd=password;data source=localhost;initial catalog=asp` |
| `MONGO_URI` | MongoDB connection string | Yes | `mongodb://localhost:27017` |
| `OPENAI_API_KEY` | OpenAI API key for AI features | Yes | `sk-...` |
| `ANTHROPIC_API_KEY` | Anthropic API key | No | `sk-ant-...` |
| `PORT` | Query Engine port (default: 21000) | No | `80` |

## Docker Setup

Use the pre-built Docker image:

```bash
docker run -d -p 21000:21000 --restart always \
  -e DB="<asp db connection string>" \
  -e MONGO_URI="<mongodb connection string>" \
  -e OPENAI_API_KEY="<openai api key>" \
  -e ANTHROPIC_API_KEY="<anthropic api key>" \
  iviva.azurecr.io/services/lucy-query-engine:v1
```

## Iviva Configuration

Add Query Engine to Iviva configuration:

```yaml
ServiceRegistry.QueryEngine: <Query Engine URL>
# Example: http://localhost:21000
```

## Verification

### Check Query Engine Status
Test if Query Engine is running:
```bash
curl http://<query engine url>/status
# Example: http://localhost:21000/status
```

### Check Iviva Integration
**Option 1: Via Canvas Interface**
1. Navigate to `/apps/lucy/canvas/queries`
2. Click the plus icon
3. If Data Explorer interface loads → Working
4. If error message appears → Query Engine not running

**Option 2: Via API**
```bash
curl --request GET \
--url http://<your-iviva-host>:<port>/components/QueryEngine/status \
--header 'authorization: APIKEY <your-api-key>'
```

Replace `<your-iviva-host>`, `<port>`, and `<your-api-key>` with your actual values.

Expected response:
```json
{
  "msg": "query engine is running",
  "details": {
    "supportParams": true
  }
}
```

## SaaS Accounts

For lucyday.io or lucyhq.com accounts, Query Engine is pre-configured and ready to use.