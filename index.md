## Welcome to the Rubyx D6 data warehouse developer portal.

This page contains all the information needed to query the databases in the D6 Data Warehouse maintained by Rubyx. These queries are done through the use of the Google BigQuery API. 

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

Replace [PATH] with the path of the JSON file that contains your service account key. 

```
export GOOGLE_APPLICATION_CREDENTIALS="[PATH]" 
```

*Example: Linux or Windows*

Replace [PATH] with the path of the JSON file that contains your service account key. 

```
set GOOGLE_APPLICATION_CREDENTIALS=[PATH]
```
 

#### SQL query
#### Example



### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
