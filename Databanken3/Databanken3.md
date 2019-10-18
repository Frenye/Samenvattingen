---
extensions:
  preset: gfm

---

<h1 id="what-is-big-data">1. What is big data?</h1>
<h2 id="information-management">1.1 Information Management</h2>
<p>Information Management includes</p>
<ul>
<li>Data storing</li>
<li>Data processing</li>
<li>Data visualisation</li>
</ul>
<p><img src="https://raw.githubusercontent.com/Frenye/Samenvattingen/master/Databanken3/images/1.1.1.png" alt="Decision Support Systems"></p>
<h3 id="hierarchial-structure-of-information-management-systems">Hierarchial structure of information management systems</h3>
<h4 id="oltp">OLTP</h4>
<ul>
<li>Online Transaction Processing</li>
<li>Works with transactions</li>
<li>Transaction = logical unit of work</li>
<li>Working with transactions ensures data consistency and guarantees there is no information loss</li>
<li>Intended to support operational activities of a company</li>
<li>Keeps most recent data up-to-date</li>
</ul>
<h4 id="olap">OLAP</h4>
<ul>
<li>Online Analytical Processing</li>
<li>Used for advanced analysis</li>
<li>Central to an OLAP system is a data warehouse or data marts</li>
<li>Data warehouses collect data from different sources and thus records the history of the data</li>
<li>Allows user to do historical analysis on the data</li>
<li>Midterm and longterm decision support</li>
</ul>
<h4 id="data-mining">Data Mining</h4>
<ul>
<li>Examining the data to try to find previously unknown knowledge</li>
<li>Found by searching for correlations within the available data collections</li>
<li>These correlations are not visible to users of data warehouse or database and cannot be found via SQL queries or OLAP operations</li>
</ul>
<h4 id="data-presentation">Data Presentation</h4>
<ul>
<li>Not all correlations found yield relevant knowledge</li>
<li>They must be critically analysed by business analysts</li>
<li>Major challenge to build efficient that are able to visualise adequately the available data and the analysis results</li>
</ul>
<h4 id="making-decisions">Making Decisions</h4>
<ul>
<li>Data can be entered into decision support systems</li>
<li>Allows the user to select option that best meets the preferences of the users</li>
<li>If there is more relevant data, the results of the analysis are more accurate</li>
</ul>
<h3 id="traditional-systems">Traditional Systems</h3>

