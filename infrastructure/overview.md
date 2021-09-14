# Azure Synapse in a day demos - Infrastructure

## Overview

This is a hands-on lab for infrastructure, telecommunications, transportation, and manufacturing using Azure Synapse Analytics. It provides step-by-step procedures for basic analysis platform construction, IoT sensor data visualization, and predictive maintenance features.

## Building analytic solutions with Azure Synapse Analytics (Basic)

### About the latest data analysis infrastructure

The modern data analytics infrastructure, called the Modern Data Warehouse pattern, is based on the following points to address big data:

- Scalability

    By focusing on a managed infrastructure of PaaS, scalability that is difficult to get on-premises can be achieved. By incorporating a distributed processing infrastructure such as Spark, data processing in petabytes can be performed quickly.

- Use of data lakes

    The data lake accumulates unstructured data, such as JSON and image data. By implementing structured processing by distributed processing, visualization of data that has been difficult to use in the past and the use of AI will be promoted.

In traditional Azure data analytic infrastructures, the solution was for architects to combine PaaS such as Azure Data Factory, Azure Databricks, Azure SQL Data Warehouse, and Azure Data Lake Storage to accommodate a variety of workloads.

**Traditional Azure analytic infrastructure configuration**

![The traditional analytics pattern is displayed.](media/traditional-azure-analytics.png "Traditional Azure Analytics")

With Azure Synapse Analytics, enjoy advantages of traditional scalable cloud data analytic infrastructures while:

- Building analytical infrastructures more quickly
- Reducing infrastructure complexity
- Reducing costs and improving development efficiency with integrated workspace and management screen

**Azure Synapse Analytics**

![Synapse Analytics takes the place of ADF, Azure Databricks, and Azure SQL Data Warehouse.](media/azure-synapse-analytics.png "Azure Synapse Analytics")

### Scenario

Build a flight delay visualization solution. With Azure Synapse Analytics, an analytic infrastructure can be built in a short time frame.

This lab will guide you through the basic deployment of Azure Synapse Analytics and creating DWH, and practicing GUI-based distributed ETL processing and reporting.

### Hands-on architecture

You will configure an architecture such as the following:

![The architecture diagram for the lab.](media/lab-1-diagram.png "Lab architecture diagram")

To build the architecture, complete the following tasks:

1. Deploy Azure Synapse Analytics
2. Create a link between flight delay data in the data lake
3. Create a SQL Pool
4. Create distributed ETL processing in the GUI with Mapping Data Flow
5. Visualize flight delay

Click on **Next** in the below to proceed with the lab. 
