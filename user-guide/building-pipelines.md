# User Guide

## Getting Started

Data Explorer is accessible through multiple entry points:

1. **Canvas Queries** - Navigate to `/apps/lucy/canvas/queries`
2. **Widget Data Binding** - When configuring data sources for widgets
3. **Standalone Interface** - Custom UIs in UXP (`/apps/uxp/screen/<custom-page-id>`)



***

## Initial Pipeline Creation

Upon accessing Data Explorer, you have three options:

### 1. Natural Language Query

Enter a natural language question about your data. AI generates a pipeline automatically based on your request.

### 2. Sample Questions

Select from pre-generated sample questions based on available data sources. This creates an initial pipeline.

### 3. Manual Source Selection

Choose an initial data source from the categorized list with search functionality to build from scratch.

All options lead to the **Pipeline Builder** interface.

***

## Pipeline Builder Interface

The interface consists of two main panels:

### Left Sidebar

Contains three tabs at the bottom:

**Pipeline Tab**

* Displays initial source and list of steps
* Provides options to add, remove, and configure steps

**Data Sources Tab**

* Lists all sources used in the pipeline
* Provides options to remove or configure sources
* Saved queries require parameter configuration

**Params Tab**

* Manage pipeline parameters
* Provides options to add, remove, and configure parameters

Selecting items from the left panel updates the right-side configuration area to display details of the selected item and preview data up to that step based on the configured parameters.

### Right Panel

Contains two sections:

* **Configuration Area (Top)**: Displays step-specific settings and descriptions
* **Preview Section (Bottom)**: Displays a preview of data-based on the selected item in the left panel, automatically refreshing when configuration changes are made.

***

## Adding and Configuring Steps

1. Add steps sequentially to transform your data.
2. Configure step properties as required.
3. The preview automatically refreshes (some steps require clicking "**Apply**").
4. Continue adding steps until the desired output is achieved.

## Parameter Configuration

Parameters enable pipeline reusability with dynamic values.

### Parameter Properties

| Property      | Description                                             |
| ------------- | ------------------------------------------------------- |
| Name          | Unique identifier for the parameter                     |
| Type          | Data type (text, number with rounding options, boolean) |
| Default Value | Used when no value is provided during execution         |

### Using Parameters

* Parameters are available in **Filter** steps
* The preview uses **default values**
* Execution uses provided values or defaults to default value

***

## Saving and Using Pipelines

### Save Options by Entry Point

**Standalone Interface**

* Displays a **Save** button.
* Shows the JSON pipeline definition for copying
* Can be used in Lucy actions or custom widgets

**Canvas Queries**

* Provide a **name** and **description**.
* The pipeline is saved as a query.
* Appears in the queries page listing

**Widget Designer**

* Provide a **name** and **description**.
* Optional: save the pipeline for reuse.
* Returns to the Widget Designer with the pipeline bound.
* Configure widget settings and preview data.

***

## Sample Pipeline Definition

Here's an example of a Data Explorer pipeline definition:

```json
{
   "sources": [
       {
           "id": "collection:DataExplorerTesting/IBMSData",
           "source": "collection:DataExplorerTesting/IBMSData",
           "name": "IBMSData",
           "type": "collection",
           "model": "DataExplorerTesting",
           "collection": "IBMSData",
           "schema": [
               {
                   "name": "AssetKey",
                   "type": "string"
               },
               {
                   "name": "AssetID",
                   "type": "string"
               },
               {
                   "name": "EquipmentTypeName",
                   "type": "string"
               }
           ]
       }
   ],
   "steps": [
       {
           "id": "filter-1750952477932",
           "name": "FILTER",
           "type": "filter",
           "source": "collection:DataExplorerTesting/IBMSData",
           "filter": {
               "$or": [
                   {
                       "AssetID": {
                           "$eq": "{$.deparams.assetId|string}"
                       }
                   }
               ]
           }
       }
   ],
   "params": [
       {
           "id": "p-1750952465565",
           "name": "assetId",
           "type": "string",
           "defaultValue": null
       }
   ]
}
```

## Pipeline Execution Outside Canvas

When executing a pipeline outside canvas through manual execution, a dictionary of parameters and values (`configuredParams`) can be passed.

Based on the above example:

```json
{
    "sources": [...],
    "steps": [...],
    "params": [...],
    "configuredParams": {
        "assetId": "BC4-01-FCU-001"
    }
}
```

### Microservice Execution

Use iviva's microservice endpoint to execute pipelines:

```bash
curl --request POST \
  --url '<iviva base url>/components/QueryEngine/v1/execution' \
  --header 'authorization: APIKEY <api key>' \
  --header 'content-type: application/json' \
  --data '{
    "sources": [...],
    "steps": [...],
    "params": [...],
    "configuredParams": {...}
  }'
```

### Custom Widget Integration

In custom widgets, use the `uxpContext.executeComponent` wrapper to call microservice endpoints for pipeline execution.

```tsx
const pipeline = {
    "sources": [...],
    "steps": [...],
    "params": [...],
    "configuredParams": {...}
}

uxpContext.executeComponent('QueryEngine', '/v1/execution', pipeline)
    .then(result => {
        // handle response 
    })
    .catch(e => {
        // handle failures
    })
```

### Sample Response

```json
{
  "result": [
    {
      "_id": "67bbfc31d89a75a0b117d8e8",
      "AssetKey": "619",
      "AssetID": "BC4-01-FCU-001",
      "EquipmentTypeName": "FCU",
      "EquipmentTypeKey": "2",
      "PointName": "Status",
      "LastPointValue": "1",
      "LastPointUpdatedTime": "2024-05-15T06:59:03.459Z",
      "LocationKey": "201",
      "LocationName": "Australia.C4 Tower.Low Rise.Floor 01",
      "EquipmentTypeLabel": "Fan Coil Unit",
      "PointUnit": ""
    }
  ],
  "trace": {
    "parsing": 0.002,
    "loading": 0,
    "execution": 0.021,
    "total": 0.023,
    "steps": [
      {
        "id": "filter-1750948946659",
        "elapsed": 0
      }
    ]
  }
}
```

## Parameter Usage in Widgets

When using pipelines with parameters in widgets:

* Widget prompts for parameter values
* Use context data: filters, widget parameters, user inputs, or other data sources
* Dropdown displays the available values for selection
* Dynamic binding enables responsive data visualisation
