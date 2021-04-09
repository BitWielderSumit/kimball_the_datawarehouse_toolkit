# kimball_the_datawarehouse_toolkit

## Chapter 1 : Data Warehousing, Business Intelligence, and Dimensional Modeling Primer

### Different Worlds of Data Capture and Data Analysis

* Users of operational system typically handeles one transaction at a time and perform actions like taking orders, launching customer complaints, monitoring the system etc, they turn the wheel of organization.
* Users of a DW/BI system, on the other hand, watch the wheels of the organization turn to evaluate performance.

### Goals of Data Warehousing and Business Intelligence

* The DW/BI system must make information easily accessible
* The DW/BI system must present information consistently.
* The DW/BI system must adapt to change.
* The DW/BI system must present information in a timely way.
* The DW/BI system must be a secure bastion that protects the information
assets
* The DW/BI system must serve as the authoritative and trustworthy foun-dation for improved decision making.
* The business community must accept the DW/BI system to deem it successful.

### Dimensional Modeling Introduction
Dimensional modeling is widely accepted as the preferred technique
for presenting analytic data because it addresses two simultaneous requirements:
1. Deliver data that’s understandable to the business users.
2. Deliver fast query performance.


### Star Schemas Versus OLAP Cubes
* A star schema hosted in a relational database is a good physical foundation for building an OLAP cube, and is generally regarded as a more stable basis for backup and recovery.
* OLAP cubes have traditionally been noted for extreme performance advantages over RDBMSs, but that distinction has become less important with advances in computer hardware, such as appliances and in-memory databases, and RDBMS software, such as columnar databases.
* It is typically more diffi cult to port BI applications between diff erent OLAP tools than to port BI applications across different relational databases.
* OLAP cubes off er signifi cantly richer analysis capabilities than RDBMSs, which are saddled by the constraints of SQL. This may be the main justification for using an OLAP product.

### Fact Tables for Measurements ( What is fact tables )
* The fact table in a dimensional model stores the performance measurements resulting from an organization’s business process events.
* The term fact represents a business measure. Imagine standing in the marketplace watching products being sold and writing down the unit quantity and dollar sales amount for each product in each sales transaction. These measurements are captured as products are scanned at the register
* Each row in a fact table corresponds to a measurement event. The data on each row is at a specific level of detail, referred to as the grain
* The most useful facts are numeric and additive, such as dollar sales amount.
* Additivity is crucial because BI applications rarely retrieve a single fact table row. Rather, they bring back hundreds, thousands, or even millions of fact rows at a time, and the most useful thing to do with so many rows is to add them up
* You will see that facts are sometimes semi-additive or even non-additive. Semi-additive facts, such as account balances, cannot be summed across the time dimension. Non-additive facts, such as unit prices, can never be added.
* You should not store redundant textual information in fact tables. Unless the text is unique for every row in the fact table, it belongs in the dimension table
* All fact table grains fall into one of three categories: transaction, periodic snapshot, and accumulating snapshot.

### Dimension Tables for Descriptive Context ( What is dimension tables )
* Dimension tables are integral companions to a fact table. The dimension tables contain the textual context associated with a business process measurement event.
* Dimension **attributes** serve as the primary source of query constraints, groupings, and report labels
* For example, when a user wants to see dollar sales by brand, brand must be available as a dimension attribute
* Attributes should consist of real words rather than cryptic abbreviations. You should strive to minimize the use of codes in dimension tables by replacing them with more verbose textual attributes
* In many ways, the data warehouse is only as good as the dimension attributes; the analytic power of the DW/BI environment is directly proportional to the quality and depth of the dimension attributes.
--------
* it is sometimes unclear whether a numeric data element is a fact or dimension attribute ( price of product )

Note : The designer’s dilemma of whether a numeric quantity is a fact or a dimension attribute is rarely a diffi cult decision. Continuously valued numeric observations are almost always facts; discrete numeric observations drawn from a small list are almost always dimension attributes.

* The hierarchical descriptive information is stored redundantly in the spirit of ease of use and query performance. You should resist the perhaps habitual urge to normalize data by storing only the brand code in the product dimension and creating a separate brand lookup table
* You should almost always trade off dimension table space for simplicity and accessibility.


## Kimball’s DW/BI Architecture
There are four separate and distinct components to
consider in the DW/BI environment: 
1. operational source systems
2. ETL system
3. data presentation area
4. business intelligence applications.

## Alternative DW/BI Architectures

### 1. Independent Data Mart Architecture
* With this approach, analytic data is deployed on a departmental basis without concern to sharing and integrating information across the enterprise
* a single department identifies requirements for data from an operational source system, Working in isolation, this departmental datamart addresses the department’s analytic requirements.
* Meanwhile, another department is interested in the same source data. It’s extremely common for multiple departments to be interested in the same performance metrics resulting from an organization’s core business process events.
* It proceeds down a similar path on its own, obtaining resources and building a departmental solution that contains similar, but slightly different data.
* We strongly discourage the independent data mart approach. However, often these independent data marts have embraced dimensional modeling because they’re interested in delivering data that’s easy for the business to understand and highly responsive to queries.

### 2. Hub-and-Spoke Corporate Information Factory Inmon Architecture
* The final architecture warranting discussion is the marriage of the Kimball and Inmon CIF architectures. This architecture populates a CIF-centric EDW that is completely off limits to business users for analysis and reporting. It’s merely the source to populate a Kimball-esque presentation area in which the data is dimensional, atomic (complemented by aggregates), process centric, and conforms to the enterprise data warehouse bus architecture.

# Kimball DimensionalModeling Techniques Overview

### Collaborative Dimensional Modeling Workshops
* Dimensional models should be designed in collaboration with subject matter experts and data governance representatives from the business. The data modeler is in charge, but the model should unfold via a series of highly interactive workshops with business representatives

### Four-Step Dimensional Design Process
* Select the business process.
* Declare the grain.
* Identify the dimensions.
* Identify the facts

### Business Processes
* Business processes are the operational activities performed by your organization, such as taking an order, processing an insurance claim, registering students for a class, or snapshotting every account each month
* Business process events generate or capture performance metrics that translate into facts in a fact table.

### Grain
* The grain establishes exactly what a single fact table row represents.
* The grain must be declared before choosing dimensions or facts because every candidate dimension or fact must be consistent with the grain.
* Atomic grain refers to the lowest level at which data is captured by a given business process. We strongly encourage you to start by focusing on atomic-grained
* Different grains must not be mixed in the same fact table.

### Dimensions for Descriptive Context
* Dimension tables contain the descriptive attributes
used by BI applications for fi ltering and grouping the facts.

### Facts for Measurements
* Facts are the measurements that result from a business process event and are almost always numeric.

### Star Schemas and OLAP Cubes
* Star schemas are dimensional structures deployed in a relational database management system (RDBMS).
* An online analytical processing (OLAP) cube is a dimensional structure implemented in a multidimensional database; it can be equivalent in content to, or more often derived from, a relational star schema.

## Basic Fact Table Techniques:
* A fact table contains the numeric measures produced by an operational measurement event in the real world.
* In addition to numeric measures, a fact table always contains foreign keys for each of its associated dimensions, as well as optional degenerate dimension keys and date/time stamps.


### Additive, Semi-Additive, Non-Additive Facts
The numeric measures in a fact table fall into three categories. The most fl exible and
useful facts are fully additive;

* additive measures can be summed across any of the dimensions associated with the fact table.
* Semi-additive measures can be summed across some dimensions, but not all. balance amounts are common semi-additive facts because they are additive across all dimensions except time
* Finally, some measures are completely non-additive, such as ratios.
* A good approach for non-additive facts is, where possible, to store the fully additive components of the non-additive measure and sum these components into the fi nal answer set before calculating the fi nal non-additive fact
* This final calculation is often done in the BI layer or OLAP cube.


### Nulls in Fact Tables
* Null-valued measurements behave gracefully in fact tables
* The aggregate functions ( SUM , COUNT , MIN , MAX , and AVG ) all do the “right thing” with null facts.
* However, nulls must be avoided in the fact table’s foreign keys because these nulls would automatically cause a referential integrity violation.
* Rather than a null foreign key, the associated dimension table must have a default row (and surrogate key) representing the unknown or not applicable condition.

### Conformed Facts
* If the same measurement appears in separate fact tables, care must be taken to make sure the technical definitions of the facts are identical, If they are to be compared or computed together.

### Transaction Fact Tables
* A row in a transaction fact table corresponds to a measurement event at a point in space and time.
* Atomic transaction grain fact tables are the most dimensional and expressive fact tables;

### Periodic Snapshot Fact Tables
* A row in a periodic snapshot fact table summarizes many measurement events occurring over a standard period ( day, a week, or a month )
* The grain is the period, not the individual transaction
* These fact tables are uniformly dense in their foreign keys because even if no activity takes place during the period, a row is typically inserted in the fact table containing a zero or null for each fact.

### Accumulating Snapshot Fact Tables
* A row in an accumulating snapshot fact table summarizes the measurement events occurring at predictable steps between the beginning and the end of a process.
* such as order fulfi llment or claim processing, that have a defined start point, standard intermediate steps, and defined end point can be modeled with this type of fact table.
* An individual row in an accumulating snapshot fact table, corresponding for instance to a line on an order, is initially inserted when the order line is created.
* As pipeline progress occurs, the accumulating fact table row is revisited and updated.

### Factless Fact Tables
* Although most measurement events capture numerical results, it is possible that the event merely records a set of dimensional entities coming together at a moment in time. For example, an event of a student attending a class on a given day may not have a recorded numeric fact, but a fact row with foreign keys for calendar day,student, teacher, location, and class is well defined. 
* Likewise, customer communications are events, but there may be no associated metrics. Factless fact tables can also be used to analyze what didn’t happen. 
* These queries always have two parts: a factless coverage table that contains all the possibilities of events that might happen and an activity table that contains the events that did happen. When the activity is subtracted from the coverage, the result is the set of events that did not happen.

### Aggregate Fact Tables or OLAP Cubes
Aggregate fact tables are simple numeric rollups of atomic fact table data built solely to accelerate query performance.

### Consolidated Fact Tables
* It is often convenient to combine facts from multiple processes together into a single consolidated fact table if they can be expressed at the same grain.
* For example, sales actuals can be consolidated with sales forecasts in a single fact table to make the task of analyzing actuals versus forecasts simple and fast

## Basic Dimension Table Techniques
The techniques in this section apply to all dimension tables. Dimension tables are
discussed and illustrated in every chapter.

### Dimension Table Structure
* Dimension tables are usually wide, flat denormalized tables with many low-cardinality text attributes.
* Dimension table attributes are the primary target of constraints and grouping specifications from queries and BI applications. The descriptive labels on reports are typically dimension attribute domain values.

### Dimension Surrogate Keys
* A dimension table is designed with one column serving as a unique primary key.
* This primary key cannot be the operational system’s natural key because there will be multiple dimension rows for that natural key when changes are tracked over time.

### Natural, Durable, and Supernatural Keys
* Natural keys created by operational source systems are subject to business rules outside the control of the DW/BI system. For instance, an employee number (natural key) may be changed if the employee resigns and then is rehired.
* When the data warehouse wants to have a single key for that employee, a new durable key must be created that is persistent and does not change in this situation
* This key is sometimes referred to as a durable supernatural key.

### Degenerate Dimensions
* Sometimes a dimension is defi ned that has no content except for its primary key
* For example, when an invoice has multiple line items, the line item fact rows inherit all the descriptive dimension foreign keys of the invoice, and the invoice is left with no unique content.
* But the invoice number remains a valid dimension key for fact tables at the line item level.
* This degenerate dimension is placed in the fact table with the explicit acknowledgment that there is no associated dimension table.
* Degenerate dimensions are most common with transaction and accumulating snapshot fact tables.

### Multiple Hierarchies in Dimensions
* Many dimensions contain more than one natural hierarchy. For example, calendar date dimensions may have a day to week to fi scal period hierarchy, as well as a day to month to year hierarchy. Location intensive dimensions may have multiple geographic hierarchies. In all of these cases, the separate hierarchies can gracefully coexist in the same dimension table.

### Flags and Indicators as Textual Attributes
* Cryptic abbreviations, true/false fl ags, and operational indicators should be supplemented in dimension tables with full text words that have meaning when independently viewed.
* Operational codes with embedded meaning within the code value should be broken down with each part of the code expanded into its own separate descriptive dimension attribute.

### Null Attributes in Dimensions
Null-valued dimension attributes result when a given dimension row has not been fully populated, or when there are attributes that are not applicable to all the dimension’s rows. In both cases, we recommend substituting a descriptive string, such as
Unknown or Not Applicable in place of the null value. Nulls in dimension attributes should be avoided because diff erent databases handle grouping and constraining on nulls inconsistently.

### Calendar Date Dimensions
Calendar date dimensions are attached to virtually every fact table to allow navigation of the fact table through familiar dates, months, fiscal periods, and special days on the calendar. You would never want to compute Easter in SQL, but rather want to look it up in the calendar date dimension. The calendar date dimension typically
has many attributes describing characteristics such as week number, month name, fiscal period, and national holiday indicator.

### Role-Playing Dimensions
A single physical dimension can be referenced multiple times in a fact table, with
each reference linking to a logically distinct role for the dimension. For instance, a
fact table can have several dates, each of which is represented by a foreign key to the
date dimension. It is essential that each foreign key refers to a separate view of
the date dimension so that the references are independent. These separate dimen-
sion views (with unique attribute column names) are called roles.

### Outrigger Dimensions
A dimension can contain a reference to another dimension table. For instance, a bank account dimension can reference a separate dimension representing the date the account was opened. These secondary dimension references are called outrigger dimensions.


## Integration via Conformed Dimensions









