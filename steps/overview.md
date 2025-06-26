# Data Explorer Steps

Data Explorer provides various transformation steps to process and manipulate data from multiple sources. Each step transforms data in a specific way as part of your data pipeline. Each step requires a set of properties to be configured.

**Note:** Every step requires one or more data sources to apply the transformation that step provides. When building the pipeline, you begin with an initial source and add one or more steps. For any step, the source can be either a new source or any of the previous steps. The output of a step becomes a data source for subsequent steps.

## Common Properties

All steps inherit these base properties:

| Property | Required | Description |
|----------|----------|-------------|
| Name | Yes | Display name for the step |

## Filter Step

Filter data by one or more fields using various conditions.

| Property | Required | Description |
|----------|----------|-------------|
| Source | Yes | Data source |
| Filters | Yes | Filter conditions and criteria |
| Ignore filters with empty values | No | Skip filtering when filter value is empty |

## Summarize (Aggregate) Step

Summarize and aggregate data. Supports multiple aggregations.

| Property | Required | Description |
|----------|----------|-------------|
| Source | Yes | Data source |
| Aggregation | Yes | Aggregation conditions and criteria |

### Aggregation Types
- `sum` - Sum of numeric values
- `average` - Average of numeric values  
- `first` - First value in group
- `last` - Last value in group
- `min` - Minimum value
- `max` - Maximum value
- `csv` - Comma-separated values
- `count` - Count of records

## Join Step

Join another data source with the current data.

| Property | Required | Description |
|----------|----------|-------------|
| Source 01 | Yes | Primary source |
| Source 02 | Yes | Secondary source to join |
| Match Records by | Yes | Join conditions and criteria |
| Fields to include | Yes | Fields to include from secondary source |

## Custom Transform Step

Transform data and add or modify columns using AI-generated logic.

| Property | Required | Description |
|----------|----------|-------------|
| Source | Yes | Data source |
| Description | Yes | Natural language description of transformation |

## Custom Expand Step

Expand a row into multiple rows using custom AI logic.

| Property | Required | Description |
|----------|----------|-------------|
| Source | Yes | Data source |
| Description | Yes | Natural language description of expansion logic |

## Limit Step

Limit the number of records returned.

| Property | Required | Description |
|----------|----------|-------------|
| Source | Yes | Data source |
| Max number of records | Yes | Maximum number of rows to return |

## Select Step

Select specific fields from the data source.

| Property | Required | Description |
|----------|----------|-------------|
| Source | Yes | Data source |
| Fields to include | Yes | List of field names to include |

## Sort Step

Sort data by one or more fields.

| Property | Required | Description |
|----------|----------|-------------|
| Source | Yes | Data source |
| Sort by | Yes | Sort conditions and criteria |

## Combine (Union) Step

Combine multiple data sources together into one.

| Property | Required | Description |
|----------|----------|-------------|
| Sources | Yes | List of sources to combine |

## Historical Data Step

Query historical values of a given collection and field.

| Property | Required | Description |
|----------|----------|-------------|
| Source | Yes | Data source |
| Field | Yes | Field to match with historical data source |
| Historical data source | Yes | Historical data source |
| Historical source field | Yes | Field in historical data source to match with source |
| Aggregation | Yes | How to aggregate historical data |
| Starts | Yes | Start date/time for historical query |
| Ends | Yes | End date/time for historical query |
| Bucket | Yes | Time bucket for aggregation |

### History Buckets
- `1min` - 1 minute intervals
- `5min` - 5 minute intervals
- `15min` - 15 minute intervals
- `30min` - 30 minute intervals
- `hour` - Hourly intervals
- `day` - Daily intervals
- `week` - Weekly intervals
- `month` - Monthly intervals
- `year` - Yearly intervals
