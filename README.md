# QMetry to Grafana Visualization Project

## Overview
This project provides a complete workflow to convert QMetry JSON test execution data into a time-series format suitable for InfluxDB, followed by visualizing this data in Grafana. The goal is to offer real-time insights into test execution metrics through interactive graphs and dashboards in Grafana.

## Objective
- Transform QMetry JSON data to InfluxDB-compatible format.
- Store the data in InfluxDB for efficient time-series management.
- Use Grafana to visualize and analyze test execution metrics, including test durations, pass/fail status, and cycles.

## Project Structure
- **Dockerfile**: Defines the environment for running the Python script and dependencies within a Docker container.
- **docker-compose.yml**: Sets up and manages the Docker services for InfluxDB, Grafana, and the Python script.
- **insert_qmetry_data.py**: Python script that converts QMetry JSON data and writes it into InfluxDB.
- **qmetry_data.json**: Sample JSON file containing test execution data from QMetry.

## Prerequisites
- **Docker**
- **Docker Compose**

## Setup Instructions

### Step 1: Clone the Repository
```bash
git clone https://github.com/kgrover31/qmetry_influx_grafana_demo.git
cd qmetry_influx_grafana_demo
```

### Step 2: Update qmetry_data.json
Update the qmetry_data.json file with the latest QMetry data, or use the existing sample for demonstration purposes.

### Step 3: Run Docker Compose
This command will build and run the necessary containers for InfluxDB, Grafana, and the Python data ingestion script.
```bash
docker-compose up -d
```

### Step 4: Access Grafana
- Open Grafana in your browser at http://localhost:3000.
- Login with default credentials (admin/admin), then follow the prompts to update the password.

### Step 5: Configure InfluxDB as a Data Source in Grafana
1. Navigate to Configuration > Data Sources > Add Data Source.
2. Select InfluxDB from the list.
3. Set the URL to http://influxdb:8086 and the database to qmetry_data.
4. Click Save & Test to verify the connection.

### Step 6: Visualize Data in Grafana
1. Create a new dashboard in Grafana.
2. Add panels to the dashboard:
   - Time Series Graph: Shows execution duration over time.
   - Bar Chart: Displays pass/fail distribution by test cycle.
3. Use InfluxDB queries to filter and analyze metrics as needed

## Example Queries

### Execution Duration
To visualize the execution duration over time:
```sql
SELECT "duration" FROM "test_executions" WHERE $timeFilter
```

### Pass/Fail Distribution by Cycle
To display the pass/fail distribution by test cycle:
```sql
SELECT COUNT("status") FROM "test_executions" WHERE "status" = 'PASS' GROUP BY "test_cycle"
```

## Additional Information
Refer to the project report for a comprehensive guide covering each step, challenges encountered, and future considerations. The report also includes screenshots for visual reference, documenting each stage of the setup and configuration within Grafana.

## Future Considerations
- **Automate Data Fetching**: To keep the data up-to-date, consider implementing a scheduled job to fetch data directly from QMetry APIs and push it to InfluxDB at regular intervals.
- **Enhanced Alerting in Grafana**: Configure alerting in Grafana for real-time notifications on critical metrics, such as high failure rates or prolonged test execution times.
- **Extended Data Analysis**: Expand data points to include failure reasons, error codes, or other relevant metrics to support deeper analysis within Grafana.
- **Additional Visualizations**: Add custom dashboards for specific insights, such as tester performance or comparisons between different test cycles.

This `README.md` outlines the setup and future possibilities for enriching the project with more automated data integrations and analytics capabilities.
