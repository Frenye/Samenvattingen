# 1. What is big data?

## 1.1 Information Management

Information Management includes

* Data storing
* Data processing
* Data visualisation

![Decision Support Systems](images/1.1.1.png)

### Hierarchial structure of information management systems 

#### OLTP

* Online Transaction Processing
* Works with transactions
* Transaction = logical unit of work
* Working with transactions ensures data consistency and guarantees there is no information loss
* Intended to support operational activities of a company
* Keeps most recent data up-to-date

#### OLAP

* Online Analytical Processing
* Used for advanced analysis
* Central to an OLAP system is a data warehouse or data marts
* Data warehouses collect data from different sources and thus records the history of the data
* Allows user to do historical analysis on the data
* Midterm and longterm decision support

#### Data Mining

* Examining the data to try to find previously unknown knowledge
* Found by searching for correlations within the available data collections
* These correlations are not visible to users of data warehouse or database and cannot be found via SQL queries or OLAP operations 

#### Data Presentation

* Not all correlations found yield relevant knowledge
* They must be critically analysed by business analysts 
* Major challenge to build efficient that are able to visualise adequately the available data and the analysis results

#### Making Decisions

* Data can be entered into decision support systems
* Allows the user to select option that best meets the preferences of the users
* If there is more relevant data, the results of the analysis are more accurate 

### Traditional Systems

![Traditional Systems](images/1.1.2.png)

