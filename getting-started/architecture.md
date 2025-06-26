# Architecture Overview

Data Explorer consists of two main components that work together to provide data pipeline functionality.

## Components

### Data Explorer Interface
The user-facing interface where users build and manage data pipelines.

**Features:**
- Visual pipeline builder
- Results visualization

**Deployment Options:**
- Canvas/Widget designer (default)
- Standalone page (contact support)

### Query Engine
The backend service that executes data pipelines.

**Responsibilities:**
- Pipeline execution
- Data source connections

**Deployment:**
- SaaS accounts (lucyday.io, lucyhq.com): Pre-configured
- Self-hosted: Manual setup required