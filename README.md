# Covid 19 Project:

## Table of Contents

- [1) Introduction](#1-introduction)
- [2) Project Structure](#2-project-structure)
- [3) Repository Structure](#3-repository-structure)

## 1) Introduction:
This project implements an end-to-end data pipeline on Microsoft Azure to ingest, process, and visualize COVID-19 data alongside population statistics. By combining case data from the European Centre for Disease Prevention and Control (ECDC) with population figures, the pipeline enables meaningful analysis of infection trends relative to demographic context, culminating in interactive dashboards for stakeholders. 

The repository contains resource exported from Azure Data Factory. 

## 2) Project Structure:

The pipeline follows a modern data engineering pattern — Ingest → Store → Transform → Warehouse → Publish — built on Azure's native data services. Data is first ingested from two sources: the ECDC COVID-19 dataset via an HTTP connector, and population data stored in Azure Blob Storage, both orchestrated through Azure Data Factory and staged in Azure Data Lake Gen2 for scalable raw storage. The data then undergoes transformation using Azure Data Factory pipelines and Azure Databricks, which handle cleansing, joining, and enrichment tasks such as calculating infection rates relative to population. The transformed output is loaded into an Azure SQL Database, serving as a structured data warehouse optimized for querying and reporting. Finally, the modeled data is published and connected to Power BI, enabling interactive dashboards that allow stakeholders to explore COVID-19 trends, case rates, and demographic insights.

## 3) Repository Structure:

```
.
├── dataflow/                                     # transforming data from the raw directory
│   ├── df_transform_casesDeaths.json              
│   ├── df_transform_hospitalAdmissions.json                               
│   └── df_transform_testing.json
├── dataset/                                      # all datasets participated in each step's input and output
│   ├── ds_ablob_ecdcFileList.json           
│   ├── ds_ablob_population.json
│   ├── ds_adls_countryLookup.json
│   ├── ds_adls_dimDate.json
│   ├── ds_adls_processed_ecdc_casesDeaths.json
│   ├── ds_adls_processed_ecdc_hospitalAdmissionsDaily.json
│   ├── ds_adls_processed_ecdc_hospitalAdmissionsWeekly.json
│   ├── ds_adls_processed_ecdc_testing.json
│   ├── ds_adls_raw_ecdc.json
│   ├── ds_adls_raw_ecdc_casesDeaths.json
│   ├── ds_adls_raw_ecdc_hospitalAdmissions.json
│   ├── ds_adls_raw_ecdc_testing.json
│   ├── ds_adls_raw_population.json
│   ├── ds_http_ecdc.json
│   ├── ds_sql_casesDeaths.json
│   ├── ds_sql_hospitalAdmissionsDaily.json                           
│   └── ds_sql_testing.json
├── factory/                                      # datafactory.
│   └── michael-covid19-adf.json    
├── linkedService/                                # external sources including, data blob storage, datalake, databricks, external source, azure sql.
│   ├── ls_ablob_michaelcovid19blob.json             
│   ├── ls_adls_michaelcovid19datalake.json
│   ├── ls_databricks_covid19.json
│   ├── ls_http_ecdc.json                       
│   └── ls_sql_covid19_db.json
├── pipeline/                                     # all processing pipeline.
│   ├── pl_execute_population_pipeline.json       # execute ingest and processing population dataset.      
│   ├── pl_ingest_ecdc.json                       # ingest ecdc files.
│   ├── pl_ingest_population.json                 # ingest population dataset.
│   ├── pl_load_casesDeaths.json                  # load cases and deaths records to sql database.
│   ├── pl_load_hospitalAdmissionsDaily.json      # load hospital admissions to sql database.
│   ├── pl_load_testing.json                      # load testing to sql database.
│   ├── pl_process_casedDeaths.json               # process cases and deaths.
│   ├── pl_process_hospitalAdmissions.json        # process hospital admissions.
│   ├── pl_process_population.json                # trigger databrick compute to process population dataset.
│   └── pl_process_testing.json                   # process testing dataset.
└── trigger/                                      # all trigger pipeline.
    ├── tr_event_blobCreated.json                 # trigger execute population pipeline when uploading new files.               
    ├── tr_ingest_ecdc.json                 
    ├── tr_load_casesDeaths.json                 
    ├── tr_load_hospitalAdmissions.json     
    ├── tr_load_testing.json                     
    ├── tr_process_casesDeaths.json             
    ├── tr_process_hospitalAdmissions.json     
    ├── tr_process_testing.json                
    └── tr_schedule_daily.json                   
  
```
