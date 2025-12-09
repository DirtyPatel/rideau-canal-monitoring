# Rideau Canal Real-Time Monitoring System
Name: Dharti Patel

Student ID: 040775191

Course: CST8916 - Fall 2025

1. Project Title and Description

Rideau Canal Real-Time Environmental Monitoring System

This project simulates a full cloud-based IoT monitoring pipeline for tracking winter conditions at multiple Rideau Canal locations. A Python-based sensor simulator sends environmental readings to Azure, where the data is processed, stored, and visualized through a web dashboard in real time.

2. Student Information

Name: Dharti Patel
Student ID: 040775191

Repositories

Main Documentation Repo
https://github.com/DirtyPatel/rideau-canal-monitoring/tree/main

Sensor Simulation Repo
https://github.com/DirtyPatel/rideau-canal-sensor-simulation

Web Dashboard Repo
https://github.com/DirtyPatel/rideau-canal-dashboard

3. Scenario Overview

- Problem Statement

Winter operations teams need continuous real-time visibility into ice thickness, surface temperature, snow accumulation, and external temperature across various Rideau Canal locations.
Because real physical sensors were not available, a virtual IoT monitoring pipeline was created to simulate this environment.

- System Objectives

  - Simulate realistic sensor readings for three canal locations

  - Ingest data into Azure using IoT Hub

  - Process and aggregate telemetry using Stream Analytics

  - Store structured results in Cosmos DB

  - Archive raw data using Blob Storage

  - Visualize data in real-time through a web dashboard

  - Deploy dashboard as a cloud-hosted application

4. System Architecture

- Architecture Diagram

Located in architecture/architecture-diagram.png

- Data Flow Explanation

1) The Python sensor simulator generates readings every few seconds.

2) Messages are sent to Azure IoT Hub.

3) Azure Stream Analytics processes incoming data and applies a 30-second tumbling window:

  - Aggregated data → Cosmos DB

  - Raw unprocessed data → Blob Storage

4) The web dashboard retrieves aggregated values from Cosmos DB through a Node.js backend API.

5) The dashboard is hosted on Azure App Service and updates automatically.

### Azure Services Used

Azure IoT Hub
Azure Stream Analytics
Azure Cosmos DB
Azure Blob Storage
Azure App Service

5. Implementation Overview

IoT Sensor Simulation

- Generates random environmental readings for:

  - Dow’s Lake

  - Fifth Avenue

  - NAC

- Sends data to IoT Hub every few seconds

- Built using Python + Azure IoT Device SDK


### Azure IoT Hub Configuration

- One device registered

- Device connection string stored in .env

- Acts as the ingestion point for all sensor data

### Stream Analytics Job

- Input: IoT Hub

- Outputs: Cosmos DB and Blob Storage

- Window: TumblingWindow (30 seconds)

- Full query is stored in stream-analytics/query.sql



### Cosmos DB Setup

- Database: RideauCanalDB

- Container: SensorAggregations

- Partition key: /location

- Stores processed 30-second aggregated records

### Blob Storage Configuration

- Container stores raw incoming IoT messages

- Useful for auditing or long-term batch analytics

### Web Dashboard

- Built with HTML, CSS, JavaScript, Chart.js

- Backend API built using Node.js + Cosmos DB SDK

- Displays:

  - Latest values

  - Location trends

  - Real-time updates

  - Polls the backend every few seconds


### Azure App Service Deployment

- Dashboard deployed as a Node.js application

- Cosmos DB credentials added through App Settings

- Public URL accessible once deployed

6. Repositories

### 1. Main Documentation Repository
https://github.com/DirtyPatel/rideau-canal-monitoring/tree/main

- **Description:** Complete project documentation, architecture, screenshots, and guides

### 2. Sensor Simulation Repository
https://github.com/DirtyPatel/rideau-canal-sensor-simulation

- **Description:** IoT sensor simulator code

### 3. Web Dashboard Repository
 https://github.com/DirtyPatel/rideau-canal-dashboard

- **Description:** Web dashboard application

7. Demo

https://youtu.be/dPU1QFqGEFU

8. Setup Instructions

- Prerequisites

  - Python 3

  - Node.js

  - Azure Subscription


- High-Level Setup Steps

1. Run Python simulator → sends data to IoT Hub

2. Start Stream Analytics job

3. Verify Cosmos DB receives aggregated data

4. Launch dashboard locally or through Azure App Service


9. Results and Analysis

- Sample Outputs

  - Aggregated records visible in Cosmos DB

  - Raw unprocessed JSON files in Blob Storage

  - Live charts updating on dashboard

- Data Analysis

  - Values refresh every few seconds

  - All three locations produce consistent simulated data

  - Aggregations processed correctly

- Performance Observations

  - Stream Analytics processes messages almost instantly

  - Dashboard updates smoothly without delay

10. Challenges and Solutions

- I had some some issues with Iot HUB connection not  working but i resolved it after verifying kys and regenratig the credentials.
- Stream analytics not outputing, but i fixed input alias and corrected JSON paths.
-
11. AI Tools Disclosure

AI tools (ChatGPT) used to assist with documentation formatting, grammar polishing, and troubleshooting explanations.

I used it to make the python code and the strean analytics.

All configuration, Azure setup, and system testing were completed by me.