# Data Sources

Data Explorer connects multiple data sources to build comprehensive pipelines. Each source type has specific requirements and capabilities.

## SQL Tables

Connect to SQL database tables (relevant database for the iviva account) for structured data queries.

**Requirements:**

* Tables must be whitelisted during query-engine setup
* The database connection must be configured in the Query Engine environment variables

**Features:**

* Access to account data

## Lucy Collections

Access all collections from Lucy models without restrictions.

**Requirements:**

* No additional configurations required

**Features:**

* All Lucy model collections available
* Capability to integrate and combine custom data with account data from other sources, such as SQL, DX, etc.

## Lucy Actions/Workflows

Use Lucy actions and workflows as data sources.

**Requirements:**

* Action/workflow must have the capability "lucy-data-source"

**Features:**

* Capability to integrate and combine dynamic/custom data from other sources such as web services and other external sources

## DX Apps

Connect to DX applications as data sources.

**Requirements:**

* No additional configurations required

**Features:**

* Capability to access custom application-specific data

## Saved Queries/Pipelines

Reuse previously built Data Explorer pipelines as data sources.

**Requirements:**

* Any saved pipeline automatically becomes available as a data source for creating new pipelines

**Features:**

* Pipeline composition
* Modular data processing
