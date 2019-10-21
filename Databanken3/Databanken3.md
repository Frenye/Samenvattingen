
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

## 1.2 What are Big Data?

Data of very large size, typically to the extent that its manipulation and management present significant logistical challenges.

An all-encompassing term for any collection of data sets so large and complex that it becomes difficult to process using on-hand data management tools or traditional data processing applications.

## 1.3 The origin of Big Data

* New data characteristics 
* New data challenges

This data has it's own characteristics and the efficient processing of this data brings new challenges along.

## 1.4 The four V's of Big Data

### Volume: BIG data

The amount of data, also reffered to the data at rest 

Big data can be the result of a large data volume. This feature is most common. There is a great need to be able to work with ever-increasing data collections.

### Variety: Varied data

The range of data types and sources that are used, data in its many forms

In the conventional information systems we assume the data can be structured in a fixed database schema the only rarely needs to be adjusted. If data does not fit into a fixed database schema ut must first be converted, which is time consuming. In practice data is often available in various shapes and sizes.

Big data can be characterised by a lack of uniformity in the structure of the data, making it very difficult to set up a fixed database schema for it.

### Velocity: Fast data

The speed at which data comes in and goes out, data in motion, streaming data

Quickly and efficiently processing is necessary in order not to loose any information. Limitaions on the acquisition speed and timelines of the data can be another reason for characterising data as big data.

### Veracity: Bad data

The uncertainty of the data, data in doubt

* Imprecise data
* Vague data
* Uncertain data
* Incomplete data
* Inconsistent data

This indicates the quality or trustworthiness of the data. Large volumes with varied data that must be processed very quickly are very sensitive to poor data quality.

To be valuable an information system should offer adequate guarantees that the user at least knows how reliable the processing of the data was done -> truthfulness.

As the quality of your data can not be checked with a conventional information the data is described as big data.

Poor quality data is often due to inaccuracy, vagueness, uncertainty, incompleteness, inconsistency and can be caused by incorrect user input, redundant data, corrupt data.

## 1.5 Some other V's

### Virality 

How long do we need to keep the data, when does it become outdated? 

### Viscosity

Do we have enough data to perform relevant to analysis?

### Visualisation

Can the results easily be presented?

### Value

Can we create additional value based upon the data?

## 1.6 Bottlenecks

### Most common pitfalls

* Believing that one has huge volumes of data, hence requiring big data setup, even trough modern RDBMs are perfectly capable of handling these.
* Big data technologies are not easy to query, analyse, or derive insights from.# 1. What is big data?

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

## 1.2 What are Big Data?

Data of very large size, typically to the extent that its manipulation and management present significant logistical challenges.

An all-encompassing term for any collection of data sets so large and complex that it becomes difficult to process using on-hand data management tools or traditional data processing applications.

## 1.3 The origin of Big Data

* New data characteristics 
* New data challenges

This data has it's own characteristics and the efficient processing of this data brings new challenges along.

## 1.4 The four V's of Big Data

### Volume: BIG data

The amount of data, also reffered to the data at rest 

Big data can be the result of a large data volume. This feature is most common. There is a great need to be able to work with ever-increasing data collections.

### Variety: Varied data

The range of data types and sources that are used, data in its many forms

In the conventional information systems we assume the data can be structured in a fixed database schema the only rarely needs to be adjusted. If data does not fit into a fixed database schema ut must first be converted, which is time consuming. In practice data is often available in various shapes and sizes.

Big data can be characterised by a lack of uniformity in the structure of the data, making it very difficult to set up a fixed database schema for it.

### Velocity: Fast data

The speed at which data comes in and goes out, data in motion, streaming data

Quickly and efficiently processing is necessary in order not to loose any information. Limitaions on the acquisition speed and timelines of the data can be another reason for characterising data as big data.

### Veracity: Bad data

The uncertainty of the data, data in doubt

* Imprecise data
* Vague data
* Uncertain data
* Incomplete data
* Inconsistent data

This indicates the quality or trustworthiness of the data. Large volumes with varied data that must be processed very quickly are very sensitive to poor data quality.

To be valuable an information system should offer adequate guarantees that the user at least knows how reliable the processing of the data was done -> truthfulness.

As the quality of your data can not be checked with a conventional information the data is described as big data.

Poor quality data is often due to inaccuracy, vagueness, uncertainty, incompleteness, inconsistency and can be caused by incorrect user input, redundant data, corrupt data.

## 1.5 Some other V's

### Virality 

How long do we need to keep the data, when does it become outdated? 

### Viscosity

Do we have enough data to perform relevant to analysis?

### Visualisation

Can the results easily be presented?

### Value

Can we create additional value based upon the data?

## 1.6 Bottlenecks

### Most common pitfalls

* Believing that one has huge volumes of data, hence requiring big data setup, even trough modern RDBMs are perfectly capable of handling these.
* Big data technologies are not easy to query, analyse, or derive insights from.

Big data is first about managing and storing huge, high-speed, and/or unstructured datasets, but this does not automatically mean one can analyze them or easily leverage them to obtain insights.

The use of big data must be justified by the potential to create additional value in the activities of a company.
