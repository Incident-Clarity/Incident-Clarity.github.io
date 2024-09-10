---
title: "[THM] Investigating with ELK 101"
date: 2024-05-25T18:36:09+05:30
url: /Investigating_with_ELK_101/
categories: 
tags: 
draft: false
author: aayan
showauthor: false
showDateOnlyInArticle: false
showDateUpdated: true
showHeadingAnchors: false
showPagination: false
showReadingTime: true
showTableOfContents: true
showTaxonomies: true
showWordCount: true
sharingLinks: false
showEdit: false
showViews: false
showLikes: false
showSummary: false
summary: ""
---


> Aayan Ta | 25th May, 2024 | 18:37 | [Investigating with ELK 101](https://tryhackme.com/r/room/investigatingwithelk101)
## Investigating with ELK 101

**Learning Objective**

- How to perform searches, apply a filter, save search.
- How to create visualizations.
- Investigate VPN logs to identify anomalies.
- To create a dashboard using saved searches and visualizations.


### Task 1: Incident Handling Scenario

- **CyberT**, a US-based company, detected anomalies in VPN activities through monitoring logs.
- SOC Analysts need to examine VPN logs for January 2022 to identify anomalies.
- VPN logs are ingested into the index **`vpn_connections`**.
- The index contains logs specifically for January 2022.
- User Johny Brown was terminated on 1st January 2022.
- There are observed failed connection attempts against some users that require investigation.

### Task 2: ElasticStack Overview

- Elastic Stack is a collection of open-source components for **ingesting, searching, analyzing, and visualizing data** in real-time.

- **Elasticsearch**
  - Full-text search and analytics engine for storing JSON-formatted documents.
  - Used for storing, analyzing, and performing correlation on data.
  - Supports RESTful API for data interaction.

- **Logstash**
  - Data processing engine for ingesting data from various sources, filtering/normalizing it, and sending it to destinations like Kibana or a listening port.
  - Configuration file has three parts:
    - **Input**: Defines the data source. Supports various input plugins.
    - **Filter**: Specifies filter options for normalization. Supports various filter plugins.
    - **Output**: Defines the destination for filtered data. Supports various output plugins.

- **Beats**
  - Host-based agents known as Data-shippers for transferring data from endpoints to Elasticsearch.
  - Each beat is a single-purpose agent sending specific data to Elasticsearch.

- **Kibana**
  - Web-based data visualization tool for analyzing, investigating, and visualizing data streams in real-time.
  - Allows creation of multiple visualizations and dashboards for better visibility.

- **How They Work Together**
  - Beats collect data from various sources (e.g., Winlogbeat for Windows event logs, Packetbeat for network traffic).
  - Logstash collects data from Beats, ports, or files, parses/normalizes it, and stores it in Elasticsearch.
  - Elasticsearch acts as a database for searching and analyzing data.
  - Kibana visualizes the data stored in Elasticsearch, enabling the creation of visualizations, time charts, and infographics.

**Questions**

1. Logstash is used to visualize the data. (yay / nay)
	- `nay`

2. Elasticstash supports all data formats apart from JSON. (yay / nay)
	- `nay`
### Task 3: Kibana Overview

- Kibana is used to display, visualize, and search logs in the Elastic Stack.
- Key features to explore:
	- **Discover Tab**: Search and filter logs.
	- **Visualization**: Create visual representations of data.
	- **Dashboard**: Combine visualizations into interactive dashboards.

**Username: `Analyst`**

**Password: `analyst123`**
### Task 4: Discover Tab

- Kibana Discover tab is where analysts spend most of their time.
- Shows ingested logs (documents), search bar, normalized fields, etc.
- Key tasks:
  - Search logs
  - Investigate anomalies
  - Apply filters based on search term or time period

![Discover Tab](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/9635453d465f7625f5dfda21966aa6a6.png)


- **Discover Tab Features**
  - Logs (documents): Contains event information with fields and values.
  - Fields Pane: Left panel shows parsed fields from logs, allowing adding/removing fields to/from filters.
  - Index Pattern: Select index patterns from available list to access Elasticsearch data.
  - Search Bar: Add search queries or apply filters.
  - Time Filter: Narrow down results based on time duration with various options.
  - Time Interval Chart: Shows event counts over time.
  - Top Bar: Options to save/open searches, share or save searches.

- **Key Elements Explained**
  - **Time Filter**: Apply log filters based on time.
  - **Quick Select**: Multiple options for time selection; auto-refresh logs at set intervals.
  - **Timeline**: Overview of event counts over time; select bars to filter logs by time period.
  - **Index Pattern**: Defines properties of fields for data exploration; corresponds to multiple indices.
  - **Left Panel - Fields**: List of normalized fields; shows top values and occurrence percentage; allows adding/removing filters.
  - **Add Filter Option**: Apply filters to fields.
  - **Create Table**: Select important fields to create a table, reducing noise and making data more presentable; save table format for consistency.

**Questions:**

1. Select the index **vpn_connections** and filter from 31st December 2021 to 2nd Feb 2022. How many hits are returned?
	- `2861`

2. Which IP address has the max number of connections?
	- `238.163.231.224`

3. Which user is responsible for max traffic?
	- `James`

4. Apply Filter on UserName Emanda; which SourceIP has max hits?
	- `107.14.1.247`

```
EventTime

is between

2022-01-11T00:00:00.000+05:30 -> 2022-01-11T23:59:59.000+05:30
```

5. On 11th Jan, which IP caused the spike observed in the time chart?
	- `172.201.60.191`

6. How many connections were observed from IP **238.163.231.224**, excluding the **New York** state?
	- `48` hits
		- 2 Filters

### Task 5: KQL Overview

- KQL (Kibana Query Language) is used for searching ingested logs/documents in Elasticsearch.
- Kibana also supports Lucene Query Language.
- KQL can be disabled if needed.

- **KQL Syntax**
  - **Free Text Search**: Searches logs based on text only. Example: Searching "security" returns all documents containing the term.
  - **Field-based Search**: Uses a specific syntax `FIELD : VALUE` to search logs by field and value.

- **Free Text Search Examples**
  - Searching "United States" returns all logs with this term.
  - Searching "United" returns no results because KQL looks for the whole term.
  - Using wildcard `*` (e.g., `United*`) matches parts of the term.

- **Logical Operators**
  - **OR**: ` "United States" OR "England"` - Returns logs containing either term.
  - **AND**: `"United States" AND "Virginia"` - Returns logs containing both terms.
  - **NOT**: `"United States" AND NOT ("Florida")` - Returns logs from the United States excluding Florida.

- **Field-based Search Example**
  - `Source_ip : 238.163.231.224 AND UserName : Suleman`
  - Displays documents where `Source_ip` is `238.163.231.224` and `UserName` is `Suleman`.
  
- Clicking on the search bar shows available fields for use in search queries.
- For more KQL options, refer to the [official documentation](https://www.elastic.co/guide/en/kibana/7.17/kuery-query.html).

**Questions:**
1. Create a search query to filter out the logs from Source_Country as the **United States** and show logs from User James or Albert. How many records were returned?
	- `161`
		- `Source_Country:United States AND UserName:Albert or UserName:James`

2. As User **Johny Brown** was terminated on 1st January 2022, create a search query to determine how many times a VPN connection was observed after his termination.
	- `1`
		- `UserName : Johny Brown AND _index:vpn_connections`

### Task 6: Creating Visualizations

- The Visualization tab allows data visualization in different forms like tables, pie charts, bar charts, etc.
- Multiple navigation methods to the Visualization tab, including clicking a field in the Discover tab.

- **Creating Visualizations**
  - Select visualization type: tables, pie charts, etc.
  - **Correlation Option**: Dragging fields to create correlations between them.
    - Example: Select `Source_Country` to correlate with `Source_IP`.
  - **Pie Chart**: Create pie charts for top values, e.g., top 5 `Source_Country`.
  - **Table**: Show values of selected fields as columns, e.g., IPs vs. country count.

- **Saving Visualizations**
  - Click the Save button.
  - Add title and description.
  - Add to an existing dashboard or create a new one.
  - Click Save and add to the library.

- **Example Task: Failed Connection Attempts**
  - Create a table to display users and IP addresses involved in failed attempts.

**Questions:**

1. Which user was observed with the greatest number of failed attempts?
	- `Simon`

2. How many wrong VPN connection attempts were observed in January?
	- `274`

### Task 7: Creating Dashboards

- Dashboards provide visibility into log collections and can be tailored for specific needs.
- Combine saved searches and visualizations to create custom dashboards for VPN log visibility.

- **Creating Custom Dashboard**
  - Go to the Dashboard tab and click "Create dashboard".
  - Click "Add from Library".
  - Select visualizations and saved searches to add to the dashboard.
  - Adjust the layout of the added items.
  - Save the dashboard after completing it.

