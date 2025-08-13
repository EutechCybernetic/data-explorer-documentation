# Overview

Data Explorer is a tool for building custom data pipelines that query and transform data from multiple sources. It provides a visual interface for creating complex data workflows without writing code.

## Core Concepts

### Data Pipelines

A data pipeline is a sequence of operations that process data from one or more sources. Each pipeline:

* Starts with one or more data sources
* Applies transformation steps in sequence
* Outputs a list of records

### Data Sources

Data Explorer connects to various data sources:

* SQL tables (need to whitelist tables during the query-engine setup)
* Lucy model collections
* Lucy actions or workflows with the "lucy-data-source" capability
* DX applications
* Previously saved queries/pipelines

### Transformation Steps

Pipelines use transformation steps to modify data:

* **Filter**: Remove unwanted records
* **Join**: Combine data from multiple sources
* **Sort**: Order records by specified fields
* **Select**: Choose specific columns
* **Summarize**: Aggregate data with functions like sum, count, and average
* **Custom Transform**: AI-powered data transformation using natural language descriptions
* **Custom Expand**: AI-powered expansion of single rows into multiple rows
* **Historical Data**: Query time-series data
* **Limit**: Restrict the number of results
* **Combine**: Merge multiple data sources

## AI-Powered Features

Data Explorer includes built-in AI capabilities to streamline pipeline creation:

### 1. Natural Language Pipeline Creation

Instead of manually configuring sources and steps, ask questions in natural language and AI will automatically build the pipeline. Examples:

* "Show me all active users and their assigned locations"
* "List assets that haven't been maintained in the last 30 days"

### 2. Copilot Assistant

Use the Copilot feature (top-left corner) to get AI assistance while building pipelines:

* Enter natural language instructions
* AI adds and configures the appropriate steps
* Refine pipelines by refining your instructions

### 3. AI Transformations

* **Custom Transform**: Describe data transformations in plain English
* **Custom Expand**: Convert a single row into multiple rows using natural language instructions

## Key Features

* **Visual Pipeline Builder**: Point-and-click interface for creating pipelines
* **Multi-Source Joins**: Combine data from multiple systems
* **AI-Powered Transformations**: Use natural language to describe data transformations
* **Reusable Pipelines**: Save and reuse pipelines as data sources
* **Real-time Execution**: Execute pipelines on-demand

## Output Format

Data Explorer only supports list-based output. All pipelines return the following:

* A collection of records (rows)
* Each record contains named fields (columns)
* Data types are preserved from source systems and may be modified based on the transformations applied
