---
source: https://en.wikipedia.org/wiki/Fact_table
---
# Fact table

From Wikipedia, the free encyclopedia
[Jump to navigation](https://en.wikipedia.org/wiki/Fact_table#mw-head) [Jump to search](https://en.wikipedia.org/wiki/Fact_table#searchInput)

In [data warehousing](https://en.wikipedia.org/wiki/Data_warehousing), a **==fact table==** ==consists of the measurements, metrics or== [==facts==](https://en.wikipedia.org/wiki/Fact_(data_warehouse)) ==of a== [==business process==](https://en.wikipedia.org/wiki/Business_process)==. I==t is ==located at the center of a== [==star schema==](https://en.wikipedia.org/wiki/Star_schema) ==or a== [==snowflake schema==](https://en.wikipedia.org/wiki/Snowflake_schema) ==surrounded by== [==dimension tables==](https://en.wikipedia.org/wiki/Dimension_table). ==Where multiple fact tables are used, these are arranged as a== [==fact constellation schema==](https://en.wikipedia.org/wiki/Fact_constellation_schema)==.== A f==act table typically has two types of columns: those that contain facts and those that are a== [==foreign key==](https://en.wikipedia.org/wiki/Foreign_key) ==to dimension tables==. The primary key of a fact table is usually a composite key that is made up of all of its foreign keys. Fact tables contain the content of the data warehouse and store different types of measures like additive, non additive, and semi additive measures.

Fact tables provide the (usually) additive values that act as independent variables by which dimensional attributes are analyzed. Fact tables are often defined by their _grain_. The grain of a fact table represents the most atomic level by which the facts may be defined. The grain of a sales fact table might be stated as "sales volume by day by product by store". Each record in this fact table is therefore uniquely defined by a day, product and store. Other dimensions might be members of this fact table (such as location/region) but these add nothing to the uniqueness of the fact records. These "affiliate dimensions" allow for additional slices of the independent facts but generally provide insights at a higher level of aggregation (a region contains many stores).

## Example\[\]

If the [business process](https://en.wikipedia.org/wiki/Business_process) is sales, then the corresponding fact table will typically contain columns representing both [raw facts](https://en.wikipedia.org/w/index.php?title=Raw_fact&action=edit&redlink=1) and [aggregations](https://en.wikipedia.org/wiki/Aggregate_(data_warehouse)) in rows such as:

* _$12,000_, being "sales for New York store for 15-Jan-2005".

* _$34,000_, being "sales for Los Angeles store for 15-Jan-2005"
* _$22,000_, being "sales for New York store for 16-Jan-2005"
* _$21,000_, being "average daily sales for Los Angeles Store for Jan-2005"
* _$65,000_, being "average daily sales for Los Angeles Store for Feb-2005"
* _$33,000_, being "average daily sales for Los Angeles Store for year 2005"

_"Average daily sales"_ is a measurement that is stored in the fact table. The fact table also contains [foreign keys](https://en.wikipedia.org/wiki/Foreign_key) from the [dimension tables](https://en.wikipedia.org/wiki/Dimension_table), where [time series](https://en.wikipedia.org/wiki/Time_series) (e.g. dates) and other [dimensions](https://en.wikipedia.org/wiki/Dimension_(data_warehouse)) (e.g. store location, salesperson, product) are stored.

All [foreign keys](https://en.wikipedia.org/wiki/Foreign_key) between fact and dimension tables should be [surrogate keys](https://en.wikipedia.org/wiki/Surrogate_key), not reused keys from operational data.

## Measure types\[\]

* Additive - measures that can be added across any dimension.

* Non-additive - measures that cannot be added across any dimension.
* Semi-additive - measures that can be added across some dimensions.

A fact table might contain either detail level facts or facts that have been aggregated (fact tables that contain aggregated facts are often instead called summary tables).

Special care must be taken when handling ratios and percentage. One good design rule[1](https://en.wikipedia.org/wiki/Fact_table#cite_note-Kimball_DWT-1) is to never store percentages or ratios in fact tables but only calculate these in the data access tool. Thus only store the numerator and denominator in the fact table, which then can be aggregated and the aggregated stored values can then be used for calculating the ratio or percentage in the data access tool.

In the real world, it is possible to have a fact table that contains no measures or facts. These tables are called "factless fact tables", or "[junction tables](https://en.wikipedia.org/wiki/Junction_table)".

The _factless fact tables_ may be used for modeling many-to-many relationships or for capturing [timestamps](https://en.wikipedia.org/wiki/Timestamps) of events.[1](https://en.wikipedia.org/wiki/Fact_table#cite_note-Kimball_DWT-1)

## Types of fact tables\[\]

There are four fundamental measurement events, which characterize all fact tables.[2](https://en.wikipedia.org/wiki/Fact_table#cite_note-Kimball-2)

Transactional
A transactional table is the most basic and fundamental. The grain associated with a transactional fact table is usually specified as "one row per line in a transaction", e.g., every line on a receipt. Typically a transactional fact table holds data of the most detailed level, causing it to have a great number of [dimensions](https://en.wikipedia.org/wiki/Dimension_(data_warehouse)) associated with it.
Periodic snapshots
The periodic snapshot, as the name implies, takes a "picture of the moment", where the moment could be any defined period of time, e.g. a performance summary of a salesman over the previous month. A periodic snapshot table is dependent on the transactional table, as it needs the detailed data held in the transactional fact table in order to deliver the chosen performance output.
Accumulating snapshots
This type of fact table is used to show the activity of a process that has a well-defined beginning and end, e.g., the processing of an order. An order moves through specific steps until it is fully processed. As steps towards fulfilling the order are completed, the associated row in the fact table is updated. An accumulating snapshot table often has multiple date columns, each representing a milestone in the process. Therefore, it's important to have an entry in the associated date dimension that represents an unknown date, as many of the milestone dates are unknown at the time of the creation of the row.
Temporal snapshots
By applying [temporal database](https://en.wikipedia.org/wiki/Temporal_database) theory and modeling techniques the _temporal snapshot fact table_ [3](https://en.wikipedia.org/wiki/Fact_table#cite_note-3) allows to have the equivalent of daily snapshots without really having daily snapshots. It introduces the concept of time Intervals into a fact table, allowing to save a lot of space, optimizing performances while allowing the end user to have the logical equivalent of the "picture of the moment" they are interested in.

## Steps in designing a fact table\[\]

* Identify a business process for analysis (like sales).

* Identify measures of facts (sales dollar), by asking questions like 'what number of X are relevant for the business process?', replacing the X with various options that make sense within the context of the business.
* Identify dimensions for facts (product dimension, location dimension, time dimension, organization dimension), by asking questions that make sense within the context of the business, like 'analyse by X', where X is replaced with the subject to test.
* List the columns that describe each dimension (region name, branch name, business unit name).
* Determine the lowest level (granularity) of summary in a fact table (e.g. sales dollars).

An alternative approach is the four step design process described in Kimball:[1](https://en.wikipedia.org/wiki/Fact_table#cite_note-Kimball_DWT-1) select the business process, declare the grain, identify the dimensions, identify the facts.

## References\[\]

1. ^ [_**a**_](https://en.wikipedia.org/wiki/Fact_table#cite_ref-Kimball_DWT_1-0) [_**b**_](https://en.wikipedia.org/wiki/Fact_table#cite_ref-Kimball_DWT_1-1) [_**c**_](https://en.wikipedia.org/wiki/Fact_table#cite_ref-Kimball_DWT_1-2) Kimball & Ross - The Data Warehouse Toolkit, 2nd Ed \[Wiley 2002\]

* **[^](https://en.wikipedia.org/wiki/Fact_table#cite_ref-Kimball_2-0)** Kimball, Ralph (2008). _The Data Warehouse Lifecycle Toolkit, 2. edition_. Wiley. [ISBN](https://en.wikipedia.org/wiki/ISBN_(identifier)) [978-0-470-14977-5](https://en.wikipedia.org/wiki/Special:BookSources/978-0-470-14977-5).
* **[^](https://en.wikipedia.org/wiki/Fact_table#cite_ref-3)** Davide, Mauri. ["Temporal Snapshot Fact Table"](http://www.slideshare.net/davidemauri/temporal-snapshot-fact-tables).
