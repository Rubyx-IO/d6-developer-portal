## Welcome to the Rubyx D6 Warehouse developer portal

This page provides the documentation to query data tables from the D6 Warehouse powered by Rubyx. 

The document is divided into three parts. The first part informs about the structure of the data warehouse. The second section explains API authentication steps and how to use of the BigQuery API clients to make a query. The last one informs about the data model of the warehouse and provides some examples of queries to be performed.

### D6 Warehouse architecture

Rubyx D6 Warehouse is hosted by Google Cloud Platform (GCP) and leverage its serverless, highly scalable, and cost-effective multi-cloud data warehouse called BigQuery. 

GCP is organized in projects. Each project encloses BigQuery datasets containing one or more tables where data is stored. The projects, datasets and tables all have an associated ID. The combination `projectID.datasetID.tableID` allows queries to be made on the right table.

Your `projectID` will have been provided to you directly and usually corresponds to the name of your institution (e.g. `bank-of-demo`). The `datasetID` of the D6 Warehouse is `dwh`. Each table in the warehouse has a different ID. The list is given below for reference:

| `tableID`        | Description           |
| ---------- |-------------:| 
| customer     | list of all customers with their core information | 
| dictionary     | Description      | 
| eligible | Description     | 
| event | Description      | 
| hierarchy  | Description     | 
| loan  | Description      | 
| loan_application_core  | Description     | 
| loan_balance | Description     | 
| objective | Description     | 
| schedule |  schedules related to a payment of any kind, including its modification (reschedules)      | 
| scoring | Description     | 
| WAD | Description     | 

For example to query the loan table, you should use the id `projectID.dwh.loan`.

### API request

Most of the following section is taken from Google's documentation described on the Google's webpage [Quickstart: Using client libraries page](https://cloud.google.com/bigquery/docs/quickstarts/quickstart-client-libraries).

All Google Cloud APIs expose a simple traditional JSON/REST interface. If you need to write your own custom code to directly access the REST API using a third-party HTTP client library of your choice, you can find out more about how Cloud APIs work with different HTTP versions and implementations in [Google HTTP Guidelines](https://cloud.google.com/apis/docs/http) and the [REST reference for BigQuery](https://cloud.google.com/bigquery/docs/reference/rest).

While you can use Google Cloud APIs by making direct HTTP requests to the server (or RPC calls where available), Google Cloud Client Libraries are the recommended option for accessing Cloud APIs programmatically from your favorite languages. Google BigQuery Client Libraries are available for the following languages: `C#`, `Go`, `Java`, `Node.js`, `PHP`, `Python`, `Ruby`.

Note that it is still possible to request the API through other languages for which ad hoc packages are available, e.g. [bigQueryR](https://code.markedmondson.me/bigQueryR/) or [bigrquery](https://bigrquery.r-dbi.org/) for `R`. 

In this documentation, we only present the steps to request the API using the Python Client (full Reference API for Python available [here](https://googleapis.dev/python/bigquery/latest/reference.html)). But tutorials for other languages are available [here](https://cloud.google.com/bigquery/docs/quickstarts/quickstart-client-libraries#client-libraries-install-python).

#### Authentication

Authentication to request the BigQuery API is done via a service account. A service account is a special kind of account used by an application or a virtual machine (VM) instance, not a person. Service accounts are associated with private/public RSA key-pairs that are used for authentication to Google.

In practice, a json file containing the private key of your account service will have been provided to you. Be careful, whoever is in possession of this key is able to request the data from your data warehouse. It is therefore essential that you keep it safe. If there is any doubt about a security breach, please contact Rubyx support. We will generate a new key and make the previous one invalid.

Note that this service account only has permission to request `dwh` tables in your project. It does not have access to any other tables in any other projects and can only read data, not write.

To authenticate yourself, set the environment variable `GOOGLE_APPLICATION_CREDENTIALS` to the path of the JSON file that contains your service account key. This variable only applies to your current shell session, so if you open a new session, set the variable again. 

*Example: Linux or macOS*

Replace `[PATH]` with the path of the JSON file that contains your service account key. 

```
export GOOGLE_APPLICATION_CREDENTIALS="[PATH]" 
```

*Example: Windows*

Replace `[PATH]` with the path of the JSON file that contains your service account key. 

```
set GOOGLE_APPLICATION_CREDENTIALS=[PATH]
```

*For HTTP request: get bearer token*

To make HTTP requests, you will need a bearer token for authentication which can be obtained, after performing the previous commands, using the following Google Cloud SDK command (Google Cloud SDK can be installed following the instructions detailed [here](https://cloud.google.com/sdk/docs/install)):

```
gcloud auth application-default print-access-token
```

#### Setup client libraries

*Install the client library*

```python
pip install --upgrade google-cloud-bigquery
```

*Import the libraries*

```python
from google.cloud import bigquery
```

*Initialize a BigQuery client*

```python
client = bigquery.Client()
```

#### SQL query

Queries are written in SQL using standard SQL syntax, which is described in the [query reference guide](https://cloud.google.com/bigquery/docs/reference/standard-sql/enabling-standard-sql).

The following query returns, from the schedule table, the ID's of the 10 customers with the largest due amount for the date of October 15, 2020.

```sql
SELECT Customer_ID, Amount_Due 
FROM `projectID.dwh.schedule` 
WHERE Payment_Due_Date = '2020-10-15T00:00:00' 
ORDER BY Amount_Due DESC 
LIMIT 10
```

*Running the query*

```python
query_job = client.query(
    """
    SELECT Customer_ID, Amount_Due 
    FROM `projectID.dwh.schedule` 
    WHERE Payment_Due_Date = '2020-10-15T00:00:00' 
    ORDER BY Amount_Due DESC 
    LIMIT 10"""
)

results = query_job.result()  # Waits for job to complete.
```

*Displaying the query result*

```python
for row in results:
    print("{} : {} views".format(row.Customer_ID, row.Amount_Due))
```

#### Complete source code
Here is the complete source code for the sample. Replace `projectID` with the ID of your project to be able to test the function.

```python
from google.cloud import bigquery

def query_d6_dwh():
    client = bigquery.Client()
    query_job = client.query(
        """
        SELECT Customer_ID, Amount_Due 
        FROM `projectID.dwh.schedule` 
        WHERE Payment_Due_Date = '2020-10-15T00:00:00' 
        ORDER BY Amount_Due DESC 
        LIMIT 10"""
    )

    results = query_job.result()  # Waits for job to complete.

    for row in results:
        print("{} : {} views".format(row.Customer_ID, row.Amount_Due))

if __name__ == "__main__":
    query_d6_dwh()
```

### Data model

The data model, including the name of the variables for each table in the warehouse, their description and standard values is available [here](https://docs.google.com/spreadsheets/d/1xF58QDPaE-rKDAicDCf12dMqi8x1RhhGAItdx7Nk_QA/edit?usp=sharing).

You can also query the schema of a table directly from the API as follows:

```python
from google.cloud import bigquery

def table_schema():
    client = bigquery.Client('projectID')
    table_ref = client.dataset('dwh').table('schedule')
    table = client.get_table(table_ref)

    result = ["{0}: {1}".format(schema.name,schema.field_type) for schema in table.schema]
    print(result)

if __name__ == "__main__":
    table_schema()
```    
Table schema supply column's name and datatype and optionaly column's description and mode (specifying if NULL values are allowed or not).

<!--- Get table schema

https://googleapis.dev/python/bigquery/latest/reference.html#table

```
curl --location --request GET 'https://bigquery.googleapis.com/bigquery/v2/projects/[projectID]/datasets/dwh/tables/loan' \
--header 'Authorization: Bearer [BEARER TOKEN]'
```
 -->

### Support

Having trouble with using the API? Contact support and we’ll help you sort it out.
