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

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Rubyx-IO/d6-developer-portal/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
