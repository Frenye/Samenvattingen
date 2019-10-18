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

The general purpose of an information management system is to support users when taking decisions. 

#### Common Tools:

* Querying the database 
* BI-tools
* Data mining applications 
* Dashboard and decision support systems

#### What do users want 

Users want a single logic database for their analyses. It looks like all data is coming from one single database.

#### What do we usually have: In Theory

![What we have in theory](images/1.1.3.png)

The theoretical process off:

* Extracting data from different source systems
* Transforming and cleaning it to a suitable format
* Loading it into a Datawarehouse for easy BI

The ETL process guarantees that the correct data is selected from the sources and that it is checked for errors, and if necessary is corrected and converted to current data standards and that this transformed data is loaded in the data warehouse.

Back flushing the error correction is important to insure that the data sources contain correct data. Otherwise users will start using the data warehouse for OLTP purposes.

#### What do we usually have: In Practice

![What we have in practice](images/1.1.4.png)

#### How does Big Data fit in these pictures?

Traditional databases, ETL processes, Data warehouses, Data Mining, ... are not suited for processing data with specific characteristics. Such data is called **Big Data**.


