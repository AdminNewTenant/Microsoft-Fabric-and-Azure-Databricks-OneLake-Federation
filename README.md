# Microsoft-Fabric-and-Azure-Databricks-OneLake-Federation

## Architecture Documentation

### Overview

This document describes the Medallion Architecture implemented on Microsoft Fabric, leveraging OneLake Federation in read-only mode. The architecture is designed for multi-cloud, enterprise-grade analytics, enabling governed data ingestion, scalable data processing, advanced analytics, and business intelligence—while enforcing strong security and compliance controls.
The solution follows a Bronze / Silver / Gold (Lakehouse) pattern and clearly separates responsibilities across ingestion, historization, data science, analytics, and consumption layers.

### High-Level Design Principles

Centralized data lake using OneLake as the logical data foundation
Read-only federated access to external data sources when required
Medallion architecture for progressive data quality, enrichment, and optimization
Clear separation of concerns between ingestion, processing, analytics, and consumption
Security-by-design leveraging identity, monitoring, and governance services
Git-enabled development lifecycle for all Microsoft Fabric artifacts

### Architecture Layers

1. Data Sources (Multi-Cloud)
This architecture supports heterogeneous operational data sources located across multiple platforms and clouds:
SQL Server
Oracle
Azure SQL Database
Cosmos DB
On-premises or site-specific OLTP systems
These systems remain the systems of record and are accessed without modification to upstream workloads.

2. Ingestion Layer

Fabric Real-Time Intelligence

Used for near real-time or streaming ingestion scenarios
Supports event-driven data flows and time-sensitive workloads

Database Mirroring

Enables near-real-time replication from transactional databases into Fabric
Provides low-latency ingestion while minimizing impact on source systems
Data is ingested as-is, without transformation, to preserve raw fidelity.

3. Historization Layer (Restricted Access)

Acts as a compliance and audit buffer
Data is retained for traceability and replay purposes
No direct user access ("No Access")
Accessible only through controlled pipelines or automated processes
This layer ensures historical integrity while isolating raw data from analytics users.

4. Lakehouse Medallion Layers
All transformation and analytical workloads operate within Microsoft Fabric Lakehouses.Bronze Lakehouse (Raw)

Stores ingested data in original format
Minimal transformations (schema alignment, metadata capture)
Source-aligned naming and structure
Silver Lakehouse (Curated)

Cleansed and standardized datasets
Business rules applied
Deduplication, normalization, and enrichment
Gold Lakehouse (Or LH)

Analytics-optimized, business-ready datasets
Aggregations, KPIs, and dimensional models
Optimized for BI, AI, and semantic modeling

5. Data Processing & Orchestration
Fabric Data Pipelines

Orchestrate ingestion, transformation, and promotion between layers
Support scheduling, dependency management, and error handling
Git-versioned for CI/CD and traceability

6. Advanced Analytics & Personas
Data Science (Azure Databricks)

Used by Data Scientists
Advanced analytics, feature engineering, and ML experimentation
Direct access to curated Lakehouse data
Data Analysis (Azure Databricks)

Used by Data Analysts
Exploratory analytics and complex transformations
Works primarily with Silver and Gold datasets

Azure Databricks is deliberately isolated from ingestion and historization layers to maintain governance boundaries.

7. Data Consumption & AI
Power BI

Semantic models built on Gold Lakehouse
Enterprise reporting and self-service BI
Supports Direct Lake where applicable
Copilot Studio

Conversational analytics and AI assistants
Grounded on governed, curated data
Microsoft Foundry

AI/ML lifecycle management
Model deployment and operationalization


Support & Governance Services
These services apply across all layers:
