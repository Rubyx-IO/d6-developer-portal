## Welcome to the Rubyx D6 data warehouse developer portal

This page provides the necessary documentation to query the data from the D6 Data Warehouse maintained by Rubyx. 

The document is divided into three parts. The first part informs about the structure of the data warehouse. The second section is about API authentication and the use of the BIgQuery API clients to make a query. The last one informs about the data model of the data warehouse tables and gives some examples of queries to be performed.

### D6 Data Warehouse architecture

The D6 Data Warehouse is hosted by Google Cloud Platform (GCP) and leverage its serverless, highly scalable, and cost-effective multi-cloud data warehouse called BigQuery. 

GCP is organized in projects. Each project encloses BigQuery datasets containing one or more tables where data is stored. The projects, dataset and table all have an associated ID. This combination `ProjectID.DatasetID.TableID` allows queries to be made on the right table.

Your `ProjectID` will have been given to you personally and usually corresponds to the name of your institution (e.g. `bank-of-demo`). The `datasetID` of the D6 Data Warehouse is `dwh`. Each table in the warehouse has a different ID. The list is given below.

| TableID        | Description           |
| ------------- |:-------------:| 
| customer     | Description | 
| dictionary     | Description      | 
| eligible | Description     | 
| event | Description      | 
| hierarchy  | Description     | 
| loan  | Description      | 
| loan_application_core  | Description     | 
| loan_balance | Description     | 
| objective | Description     | 
| schedule | Description      | 
| scoring | Description     | 
| WAD | Description     | 

For example to query the loan table, you must use the id `projectID.dwh.loan`.

### API request

The section below summarizes the elements described on the Google's webpage: [Quickstart: Using client libraries page](https://cloud.google.com/bigquery/docs/quickstarts/quickstart-client-libraries#client-libraries-install-python).

#### Authentication

Authentication to request the BigQuery API is done via a service account. A service account is a special kind of account used by an application or a virtual machine (VM) instance, not a person. Service accounts are associated with private/public RSA key-pairs that are used for authentication to Google.

In practice, a json file containing the private key of your account service will have been provided to you. Be careful, whoever is in possession of this key is able to request the data from the warehouse. Keep it safe. If there is any doubt about a security breach, please contact Rubyx support. We will generate a new key and make the previous one invalid.

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

#### Setup client libraries

While you can use Google Cloud APIs by making direct HTTP requests to the server (or RPC calls where available), Google provides client library code for all Cloud APIs that makes it easier to access them from your favorite languages. 

All Cloud APIs expose a simple traditional JSON/REST interface. If you need to write your own custom code to directly access the REST API using a third-party HTTP client library of your choice, you can find out more about how Cloud APIs work with different HTTP versions and implementations in [Google HTTP Guidelines](https://cloud.google.com/apis/docs/http) and the [REST reference for BigQuery](https://cloud.google.com/bigquery/docs/reference/rest).

However, Google Cloud Client Libraries are the recommended option for accessing Cloud APIs programmatically. Google BigQuery Client Libraries are available for the following languages: `C#`, `Go`, `Java`, `Node.js`, `PHP`, `Python`, `Ruby`.

It is possible to request the API via other languages for which ad hoc packages are available, e.g. [bigQueryR](https://code.markedmondson.me/bigQueryR/) or [bigrquery](https://bigrquery.r-dbi.org/) for `R`. 

In this documentation, we only present an example of a query using the Python Client. Tutorials for other languages are available [here](https://cloud.google.com/bigquery/docs/quickstarts/quickstart-client-libraries#client-libraries-install-python).

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

The following query returns the Id's of the ten customers with the largest due amount for the date of October 15, 2020 (do not forget to replace the value of `projectID`).

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
Here is the complete source code for the sample.

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

### Data Model


### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
