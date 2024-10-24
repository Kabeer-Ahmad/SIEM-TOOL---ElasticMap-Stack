# SIEM-TOOL---ElasticMap-Stack
SIEM TOOL - ElasticMap (ElasticSearch, LogStash, Kibana)

# Security Information and Event Management (SIEM) with Elastic Stack

### [Click Here to Watch the Video Tutorial](https://drive.google.com/file/d/10FyJsIs1QFg-nv6wDXPY1pdKUQWxlNAJ/view?usp=sharing)

## Table of Contents
1. [Introduction](#introduction)
2. [CLO2: Investigate Suspected Attacks and Policy Breaches](#clo2-investigate-suspected-attacks-and-policy-breaches)
   - [SIEM Event Collection and Processing](#siem-event-collection-and-processing)
   - [SIEM Vulnerability Data Collection](#siem-vulnerability-data-collection)
   - [SIEM Components and Data Flow](#siem-components-and-data-flow)
   - [Event Collector, Processor, and Console Architecture](#event-collector-processor-and-console-architecture)
   - [Detection and Offenses in the Management Console](#detection-and-offenses-in-the-management-console)
   - [Types of Offenses](#types-of-offenses)
3. [CLO4: Apply SIEM Tools to Customize Rules, Inspect Actions, and Responses](#clo4-apply-siem-tools-to-customize-rules-inspect-actions-and-responses)
   - [SIEM Dashboard](#siem-dashboard)
   - [Steps to Create a Dashboard](#steps-to-create-a-dashboard)
   - [SIEM Tabs Overview](#siem-tabs-overview)
   - [Menu Options in SIEM](#menu-options-in-siem)
   - [Context-Sensitive Help](#context-sensitive-help)
   - [Offense Rating and Analysis](#offense-rating-and-analysis)
4. [CLO5: Use SIEM to Create Reports, Charts, and Apply Filters](#clo5-use-siem-to-create-reports-charts-and-apply-filters)
   - [SIEM Reports Overview](#siem-reports-overview)
   - [Create and Schedule a Report](#create-and-schedule-a-report)
   - [Advanced Filtering](#advanced-filtering)
5. [Step-by-Step Installation and Configuration Process](#step-by-step-installation-and-configuration-process)
   - [Installation of Elastic Stack Components](#installation-of-elastic-stack-components)
   - [Configuration of Logstash](#configuration-of-logstash)
   - [Troubleshooting Errors Faced](#troubleshooting-errors-faced)
6. [Discussion and Reflection](#discussion-and-reflection)
7. [References](#references)

---

## 1. Introduction

Security Information and Event Management (SIEM) is a key component in monitoring, detecting, and responding to security incidents in real-time. The objective of this project was to set up a SIEM solution using Elastic Stack (Logstash, Elasticsearch, Kibana) to investigate suspected attacks, customize security rules, and create reports for examining security events. The input was simulated through terminal commands. This documentation will provide the details of the process, issues encountered, and solutions applied throughout the setup and testing.

---

## 2. CLO2: Investigate Suspected Attacks and Policy Breaches

### a. SIEM Event Collection and Processing
Elastic Stack collects events and logs using Logstash. Logstash reads log data from sources such as files, network traffic, or the terminal (in our case, standard input). Events are processed through filters such as `grok` and `date` to structure the logs and then forwarded to Elasticsearch, where they are stored and indexed.

### b. SIEM Vulnerability Data Collection
Elastic Stack can be extended to collect vulnerability data through plugins or integrations such as VulnWhisperer or OpenVAS, which feed vulnerability scan reports into the SIEM for analysis.

### c. SIEM Components and Data Flow
1. **Logstash (Collector)**: Reads raw logs and formats them for processing.
2. **Elasticsearch (Processor)**: Stores, indexes, and enables fast searching of log data.
3. **Kibana (Console)**: Provides a user interface for visualizing and analyzing logs.

### d. Event Collector, Processor, and Console Architecture
- **Event Collector**: Logstash collects events and filters the data.
- **Processor**: Elasticsearch indexes the logs for fast retrieval.
- **Console**: Kibana provides the interface for searching and visualizing data.

### e. Detection and Offenses in the Management Console
Offenses are detected based on patterns or rules defined in the SIEM, such as repeated failed login attempts triggering an alert for potential brute-force attacks.

### f. Types of Offenses
- **Policy Violation**: Actions that violate internal security policies.
- **Malicious Behavior**: Indicators of known attack vectors, such as brute-force or unauthorized access.
- **Anomaly Detection**: Unusual traffic patterns or deviations from normal behavior.

---

## 3. CLO4: Apply SIEM Tools to Customize Rules, Inspect Actions, and Responses

### a. SIEM Dashboard
The Kibana dashboard allows for real-time interaction and visualization of data using customizable visual elements such as charts, graphs, and maps.

### b. Steps to Create a Dashboard
1. Open Kibana.
2. Go to **Dashboard** and click **Create New Dashboard**.
3. Add visualizations by clicking **Add Visualization**.
4. Save the dashboard.

### c. SIEM Tabs Overview
- **Discover**: Search raw log data.
- **Visualize**: Create visual elements based on log queries.
- **Dashboard**: View a collection of visualizations.
- **SIEM App**: Show security-specific alerts and offenses.

### d. Menu Options in SIEM
Menu options allow switching between data sources, time filters, and advanced search queries (KQL).

### e. Context-Sensitive Help
Kibana offers tooltips and inline documentation to assist users with features.

### f. Offense Rating and Analysis
Offenses can be rated based on severity and log source (e.g., repeated failed login attempts).

---

## 4. CLO5: Use SIEM to Create Reports, Charts, and Apply Filters

### a. SIEM Reports Overview
Kibana generates reports on security events based on collected data.

### b. Create and Schedule a Report
1. Navigate to **Reporting** in Kibana.
2. Choose the visualization or dashboard for the report.
3. Click **Generate PDF**.

### c. Advanced Filtering
Advanced filtering in Kibana allows users to filter logs based on log sources, time ranges, and event types using Kibana Query Language (KQL).

---

## 5. Step-by-Step Installation and Configuration Process

### a. Installation of Elastic Stack Components

#### 1. **Install Elasticsearch:**
- Download the appropriate version for your architecture.
- Extract and start Elasticsearch:
    ```bash
    tar -xzf elasticsearch-7.17.4-linux-aarch64.tar.gz
    ./bin/elasticsearch
    ```

#### 2. **Install Kibana:**
- Download and extract Kibana.
- Modify the `.yml` configuration:
    ```yml
    server.host: "localhost"
    elasticsearch.hosts: ["http://localhost:9200"]
    ```
- Start Kibana:
    ```bash
    ./bin/kibana
    ```

#### 3. **Install Logstash:**
- Download and configure Logstash:
    ```bash
    input {
      stdin { }
    }

    output {
      elasticsearch {
        hosts => ["localhost:9200"]
      }
      stdout { codec => rubydebug }
    }
    ```
- Start Logstash:
    ```bash
    ./bin/logstash -f /path/to/logstash.conf
    ```

### b. Troubleshooting Errors Faced

#### 1. **Error: Path.data Conflict**
- **Solution**: Stop existing Logstash instances and remove the `.lock` file:
    ```bash
    sudo rm /path/to/logstash/data/.lock
    ```

#### 2. **Error: No Results in Kibana Discover Tab**
- **Solution**: Check `logstash.conf` and ensure correct time range in Kibana.

#### 3. **Error: Exit Code 203**
- **Solution**: Correct the `ExecStart` path in `logstash.service` and reload systemd:
    ```bash
    sudo systemctl daemon-reload
    ```

For more detailed troubleshooting, watch our [Tutorial Video](https://drive.google.com/file/d/10FyJsIs1QFg-nv6wDXPY1pdKUQWxlNAJ/view?usp=sharing).

---

## 6. Discussion and Reflection
Various challenges were faced, including Logstash configuration errors and Kibana not showing results. Through troubleshooting and adjustments, Elastic Stack was successfully configured to capture, process, and analyze logs from both terminal input and system logs.

---

## 7. References
- [Elastic Stack Documentation](https://www.elastic.co/guide/index.html)
- [Kibana Query Language (KQL)](https://www.elastic.co/guide/en/kibana/current/kuery-query.html)
- [Logstash Configuration](https://www.elastic.co/guide/en/logstash/current/configuration.html)

---

This README provides a detailed explanation of how to set up and configure a SIEM using the Elastic Stack. For further instructions, watch the tutorial video linked at the top of the page.
