# Understanding Data Explorer

Data Explorer is a tool for building custom data pipelines that query and transform data from multiple sources. It provides a visual interface for creating complex data workflows without writing code.

## Core Concepts

### Data Pipelines
A data pipeline is a sequence of operations that process data from one or more sources. Each pipeline:
- Starts with one or more data sources
- Applies transformation steps in sequence
- Outputs a list of records

### Data Sources
Data Explorer connects to various data sources:
- SQL tables (need to whitelist tables during query-engine setup)
- Lucy model collections
- Lucy actions/workflows that have the capability "lucy-data-source"
- DX applications
- Previously saved queries/pipelines

### Transformation Steps
Pipelines use transformation steps to modify data:
- **Filter**: Remove unwanted records
- **Join**: Combine data from multiple sources
- **Sort**: Order records by specified fields
- **Select**: Choose specific columns
- **Summarize**: Aggregate data with functions like sum, count, average
- **Custom Transform**: AI-powered data transformation using natural language descriptions
- **Custom Expand**: AI-powered expansion of single rows into multiple rows
- **Historical Data**: Query time-series data
- **Limit**: Restrict number of results
- **Combine**: Merge multiple data sources

## Key Features

- **Visual Pipeline Builder**: Point-and-click interface for creating pipelines
- **Multi-Source Joins**: Combine data from different systems
- **AI-Powered Transformations**: Use natural language to describe data transformations
- **Reusable Pipelines**: Save and reuse pipelines as data sources
- **Real-time Execution**: Execute pipelines on-demand

## Output Format

Data Explorer only supports list-based output. All pipelines return:
- A collection of records (rows)
- Each record contains named fields (columns)
- Data types are preserved from source systems and may be modified based on the transformations applied
