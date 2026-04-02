---
source: https://www.holistics.io/blog/the-rise-and-fall-of-the-olap-cube/
---
# The Rise and Fall of the OLAP Cube

#### by [Cedric Chin](https://www.holistics.io/blog/author/cedric/)

![[christian-fregnan-TAVKURx-xLw-unsplash.jpg]]

==One of the biggest shifts in data analytics over the past decade is the move== _==away==_ ==from building ‘data cubes’, or ‘OLAP cubes’, to running OLAP\* workloads directly on columnar databases.==

(\*OLAP means online analytical processing, but we’ll get into what that means in a bit).

The decline of the OLAP cube is a huge change, especially if you’ve built your career in data analytics over the past three decades.

This is a huge change, especially if you’ve built your career in data analytics over the past three decades. It may seem bizarre to you that OLAP cubes — which were so dominant over the past 50 years of business intelligence — are going away. And you might be rightly skeptical of this shift to columnar databases. What are the tradeoffs? What are the costs? Is this move really as good as all the new vendors say that it is? And of course, there’s that voice at the back of your head, asking: is this just another fad that will go away, like the NoSQL movement before it? Will it even last?

This essay seeks to be an exhaustive resource on the history and development of the OLAP cube, and the current shift away from it. We’ll start with definitions of the terminology (OLAP vs OLTP), cover the emergence of the OLAP cube, and then explore the emergence of columnar data warehouses as an alternative approach to OLAP workloads.

This piece is written with the novice in mind. If you’re a more experienced data analytics person, feel free to skip the first few sections, in order to get to the interesting parts at the end of this piece. Let’s dive in.

## What the Heck is OLAP?

Online Analytical Processing (or OLAP) is a fancy term used to describe a certain class of database applications. The term was invented by database legend Edgar F. Codd, in a [1993 paper](https://www.semanticscholar.org/paper/Providing-OLAP-to-User-Analysts%3A-An-IT-Mandate-Salley-Codd/a0bd1491a54a4de428c5eef9b836ef6ee2915fe7) titled _Providing OLAP to User-Analysts: An IT Mandate_.

Codd’s creation of the term wasn’t without controversy. A year before he published the paper, Arbor Software had released a software product called Essbase, and — surprise, surprise! — Codd’s paper defined properties that happened to fit Essbase’s feature set _perfectly_.

Computerworld magazine soon discovered that Arbor had paid Codd to ‘invent’ OLAP as a new category of database applications, in order to better sell its product. Codd got called out for his conflict of interest and was forced to retract his paper … but without much fallout, it seems: today, Codd is still regarded as ‘the father of the relational database’, and OLAP has stuck around as a category ever since.

‘So what _is_ OLAP?’ you might ask. The easiest way to explain this is to describe the two types of business application usage. Let’s say that you run a car dealership. There are two kinds of database-backed operations that you need to do:

1. **You need to use a database as part of some business process.** For instance, your salesperson sells the latest Honda Civic to a customer, and you need to record this transaction in a business application. You do this for operational reasons: you need a way to keep track of the deal, you need a way to contact the customer when the car loan or insurance is finally approved, and you need it to calculate sales bonuses for your salesperson at the end of the month.

* **You use a database as part of analysis.** Periodically, you will need to collate numbers to understand how your overall business is doing. In his 1993 paper, Codd called this activity ‘decision-making-support’. These queries are things like ‘how many Honda Civics were sold in London the last 3 months?’ and ‘who are the most productive salespeople?’ and ‘are sedans or SUVs selling better overall?’ These are questions you ask at the end of a month or a quarter to guide your business planning for the near future.

The first category of database usage is known as ‘Online Transaction Processing’, or ‘OLTP’. The second category of database usage is known as ‘Online Analytical Processing’, or ‘OLAP’.

Or, as I like to think of it:
* OLTP: using a database to run your business
* OLAP: using a database to understand your business

Why do we treat these two categories differently? As it turns out, the two usage types have vastly different data-access patterns.

With OLTP, you run things like ‘record a sales transaction: one Honda Civic by Jane Doe in the London branch on the 1st of January, 2020’.

With OLAP, your queries can become incredibly complex: ‘give me the total sales of green Honda Civics in the UK for the past 6 months’ or ‘tell me how many cars Jane Doe sold last month’, and ‘tell me how well did Honda cars do this quarter in comparison to the previous quarter’? The queries in the latter category aggregate data across many more elements when compared to the queries for the former.

In our example of a car dealership, you might be able to get a way with running both OLTP and OLAP query-types on a normal relational database. But if you deal with huge amounts of data — if you're querying a global database of car sales over the past decade, for instance — it becomes important to structure your data for analysis _separately_ from the business application. Not doing so would result in severe performance problems.

## The Performance Challenges of OLAP

To give you an intuition for the kinds of performance difficulties we’re talking about, think about the queries you must ask when you’re analyzing car sales at a car dealership.

* give me the total sales of green Honda Civics in the UK for the past 6 months

* tell me how many cars Jane Doe sold last month
* how many Honda cars did we sell this quarter in comparison to the previous quarter?

These queries can be reduced to a number of _dimensions_ — properties that we want to filter by. For instance, you might want to retrieve data that is aggregated by:

* date (including month, year, and day)

* car model
* car manufacturer
* salesperson
* car colour
* transaction amount

If you were to store these pieces of information in a typical relational database, you would be forced to write something like this to retrieve a 3 dimensional summary table:

    SELECT Model, ALL, ALL, SUM(Sales)
    FROM Sales
    WHERE Model = 'Civic' 
    GROUP BY Model
    UNION
    SELECT Model, Month, ALL, SUM(Sales)
    FROM Sales
    WHERE Model = 'Civic' 
    GROUP BY Model, Year
    UNION
    SELECT Model, Year, Salesperson, SUM(Sales)
    FROM Sales
    WHERE Model = 'Civic' 
    GROUP BY Model, Year, Salesperson;
    

A 3-dimensional roll-up requires 3 such unions. This is generalisable: it turns out that aggregating over N dimensions require N such unions.

You might think that this is already pretty bad, but this isn’t the worst example there is. Let’s say that you want to do a cross-tabulation, or what Excel power users call a ‘pivot table’. An example of a cross-tabulation looks like this:

![[Screenshot-2019-12-18-at-3.47.26-PM.png]]

Cross-tabulations require an even more complicated combination of unions and `GROUP BY` clauses. A six dimensional cross-tabulation, for instance, requires a 64-way union of 64 different `GROUP BY` operators to build the underlying representation. In most relational databases, this results in 64 scans of the data, 64 sorts or hashes, and a terribly long wait.

Business intelligence practitioners realised pretty early that it was a bad idea to use SQL databases for large OLAP workloads. This was made worse by the fact that computers weren’t particularly powerful back in the day: in 1995, for instance, 1GB of RAM cost $32,300 — an insane price for an amount of memory we take for granted today! This meant that the vast majority of business users had to use relatively small memories to run BI workloads. Early BI practitioners thus settled on a general approach: grab _only_ the data you need out of your relational database, and then shove it into an efficient in-memory data structure for manipulation.

## The Rise of the OLAP Cube

Enter the OLAP cube, otherwise known as the data cube.

The OLAP cube grew out of a simple idea in computer programming

The OLAP cube grew out of a simple idea in programming: take data and put it into what is known as a ‘2-dimensional array’ — that is, a list of lists. The natural progression here is that the more dimensions you wanted to analyze, the more nested arrays you would use: a 3-dimensional array is a list of list of lists, a 4-dimensional array is a list of list of list of lists, and so on. Because nested arrays exist in all the major programming languages, the idea of loading data into such a data structure was an obvious one for the designers of early BI systems.

But what if you want to run analyses on datasets that are far larger than your computer’s available memory? Early BI systems decided to do the next logical thing: they aggregated and then cached subsets of data within the nested array — and occasionally persisted parts of the nested array to disk. Today, ‘OLAP cubes’ refer specifically to contexts in which these data structures far outstrip the size of the hosting computer’s main memory — examples include multi-terabyte datasets and time-series of image data.

![[olap-3d-cube.png]]
An illustration of an OLAP cube

The impact of the OLAP cube was profound — and changed the practice of business intelligence to this very day. For starters, nearly all analysis began to be done within such cubes. This in turn meant that new cubes often had to be created whenever a new report or a new analysis was required.

Say you want to run a report on car sales by province. If your currently available set of cubes don’t include province information, you’d have to ask a data engineer to create a new OLAP cube for you, or request that she modify an existing cube to include such province data.

OLAP cube usage also meant that data teams had to manage complicated pipelines to transform data from an SQL database into these cubes. If you were working with a large amount of data, such transformation tasks could take a long time to complete, so a common practice would be to run all ETL (extract-transform-load) pipelines before the analysts came in to work. This way, the analysts wouldn’t need to wait for their cubes to be loaded with the latest data — they could have the computers do the data heavy-lifting at night, and start work immediately in the mornings. This approach, of course, became more problematic as companies globalized, and opened offices in multiple timezones that demanded access to the same analytical systems. (How do you run your pipelines at ‘night’ when your night is another office’s morning?)

Using OLAP cubes in this manner also meant that SQL databases and data warehouses had to be organized in away that made for easier cube creation. If you became a data analyst in the previous two decades, for instance, it was highly likely that you were trained in the arcane arts of [Kimball dimensional modeling](https://en.wikipedia.org/wiki/Dimensional_modeling), [Inmon-style](https://en.wikipedia.org/wiki/Bill_Inmon) entity-relationship modeling, or [data vault modeling](https://en.wikipedia.org/wiki/Data_vault_modeling). These fancy names are simply methods for organizing the data in your data warehouse to match your businesses’s analytical requirements.

Kimball, Inmon and their peers observed that certain access patterns occured in every business. They also observed that a slap-dash approach to data organization was a terrible idea, given the amount of time data teams spent creating new cubes for reporting. Eventually, these early practitioners developed repeatable methods to turn business reporting requirements into data warehouse designs — designs that would make it easier for teams to extract the data they need in the formats they need for their OLAP cubes.

These constraints have shaped the form and function of data teams for the better part of four decades. It is important to understand that very real technological constraints lead to the creation of the OLAP cube, and the demands of the OLAP cube led to the emergence of data team practices that we take for granted today. For instance, we:

* Maintain complex ETL pipelines in order to model our data.
* Hire a large team of data engineers in order to maintain these complicated pipelines.
* Model data according to Kimball or Inmon or Data Vault frameworks in order to make it easier to extract and load data into cubes. (And even when we’ve moved away from cubes, we still keep these practices in order to load data _into_ analytical and visualization tools — regardless of whether they’re built on top of cubes.)
* Have the large team of data engineers also maintain these second set of pipelines (from modelled data warehouse to cube).

Today, however, many of the constraints that lead to the creation of the data cube have loosened somewhat. Computers are faster. Memory is cheap. The cloud works. And data practitioners are beginning to see that OLAP cubes come with a number of problems of their own.

## Wither the OLAP Cube

Let’s pretend for a moment that we live in a world where ==memory is cheap and computing power is readily available====. Let’s also pretend that in this world, SQL databases are powerful enough to come in both OLTP and OLAP flavours.== What would this world look like?

==Why bother going through an extra step of building and generating new cubes when you can simply write queries in an existing SQL database?==

For starters, we’d probably stop using OLAP cubes. This is stupidly obvious: why bother going through an extra step of building and generating new cubes when you can simply write queries in an existing SQL database? Why bother maintaining a complex tapestry of pipelines if the data you need for reporting could be copied blindly from your OLTP database into your OLAP database? And why bother training your analysts in anything other than SQL?

This sounds like a tiny thing, but it isn’t: we’ve heard multiple horror stories from analysts who’ve had to depend on data engineers to build cubes and set up pipelines for every new reporting requirement. If you were an analyst in this situation, you would feel powerless to meet your deadlines. Your business users would block on you; you would block on your data engineers; and your data engineer would most likely be grappling with the complexity of your data infrastructure. This is bad for everyone. Better to avoid the complexity completely.

Second, if we lived in an alternate world where compute was cheap and memory was plentiful … well, we would ditch serious data modeling efforts.

This sounds ridiculous until you think about it from a first principles perspective. We model data according to rigorous frameworks like Kimball or Inmon because we must regularly construct OLAP cubes for our analyses. Historically, this meant a period of serious schema design. It also meant a constant amount of busy-work in order to maintain that design in our warehouses as business requirements and data sources change.

But if you no longer use OLAP cubes for your analysis, then you no longer have to extract data as regularly from your data warehouse. And if you no longer have to extract data regularly from your data warehouse, then there’s no reason to treat your data warehouse schema as a precious thing. Why do all that busy work, after all, if you could just ‘model’ data by creating new ‘modelled’ tables or materialized views within your data warehouse … _as and when you need it_? This approach has all the functional benefits of traditional data modeling without the ceremony or the complexity that goes with designing and maintaining a Kimball-style schema.

Let’s discuss a concrete example: let’s say that you think you’ve got your schema wrong. What do you do? In our alternate universe, this problem is simple to solve: you can simply dump the table (or toss the view) and create new ones. And since your reporting tools connect directly to your analytical database, this leads to little disruption: you don’t have to rewrite a complex set of pipelines or change the way your cubes are created — because you _don’t have any cubes to create in the first place._

## A New Paradigm Emerges

The good news is that this alternate universe isn’t an alternate universe. It is the world that we live in today.

How did we get here? As far as I can tell, there have been three breakthroughs over the past two decades — two of which are easy to understand. We’ll deal with those two first, before exploring the third in some detail.

The first breakthrough is a simple consequence of Moore’s law: compute and memory have become _real_ commodities, and are now both ridiculously cheap and easily available via the cloud. Today, anyone with a working credit card can go to [AWS](https://aws.amazon.com/) or [Google Cloud](https://cloud.google.com/) and have an arbitrarily powerful server spun up for them within minutes. This fact also applies to cloud-based data warehouses — companies are able to store and analyze huge data sets with practically zero fixed costs.

The ==second breakthrough is that most modern, cloud-based data warehouses have what is known as a== _==massively parallel processing==_ ==(MPP) architecture==. The central insight behind the development of MPP databases is actually really easy to understand: instead of being limited by the computational power and memory of a single computer, you can ==radically boost the performance of your query if you spread that query across hundreds if not thousands of machines. These machines will then process their slice of the query, and pass the results up the line for aggregation into a final result.== The upshot of all this work is that you get ridiculous performance improvements: Google’s BigQuery, for instance, is able to perform a full regex match on 314 million rows with no indices, and return a result within 10 seconds ([source](https://cloud.google.com/files/BigQueryTechnicalWP.pdf)).

![[Screenshot-2020-01-30-at-12.21.17-PM.png]]
The underlying infrastructure of BigQuery, an MPP database

It’s easy to sort of say “ahh MPP databases are a thing”, but there has been at least four decades of work into making it into the reality it is today. In 1985, for instance, ==database legend Michael Stonebraker published a paper titled== _[==The Case for Shared Nothing==](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.58.5370&rep=rep1&type=pdf)_ ==— an argument that the best architecture for an MPP data warehouse== ==is one where processors don’t share anything between each other.== Several researchers pushed back against this view, with [_Much Ado About Shared Nothing_](https://dl.acm.org/doi/10.1145/234889.234892) in 1996; my point here isn’t to say Stonebraker was right and his critics wrong; it is to point out that at the beginning, even simple questions like “should a distributed data warehouse have its computers share memory or storage or not?” was an open question in need of investigation. (If you’re curious: the answer to that question is ‘mostly no’).

Th==e third breakthrough was the development and spread of columnar data warehouses==. This conceptual breakthrough is really the more important of the three, and it explains why OLAP workloads can shift away from cubes and back into databases. We should understand why this is, if we want to understand the future of business intelligence.

A t==ypical relational database stores its data in row form.== A single row for a transaction, for instance, would contain the fields `date`, `customer`, `price`, `product_sku` and so on. A ==columnar database, however, stores each of these fields in separate columns. As the illustration below shows (taken from this== [==2009 presentation by Harizopoulos, Abadi and Boncz==](http://www.cs.umd.edu/~abadi/talks/Column_Store_Tutorial_VLDB09.pdf)==):==

![[Screenshot-2020-01-30-at-11.24.49-AM.png]]
Difference between a row-based RDBMS and a columnar database

While OLAP cubes demand that you load a subset of the dimensions you’re interested in into the cube, ==columnar databases allow you to perform similar OLAP-type workloads at equally good performance levels without the requirement to extract and build new cubes==. In other words, this is an SQL database that is _perfect_ for OLAP workloads.

How do columnar databases accomplish such feats of performance? As it turns out, there are ==three main benefits of storing your data in columns:==

1. **==Columnar databases have higher read efficienc==y.** If you’re running a query like “give me the average price of all transactions over the past 5 years”, a ==relational database would have to load all the rows from the previous 5 years even though it merely wants to aggregate the price field; a c====olumnar database would only have to examine one column — the price column==. This means that a columnar database only has to sift through a fraction of the total dataset size.
2. **Columnar databases also compress better than row-based relational databases.** It turns out that when you’re storing similar pieces of data together, you can compress it far better than if you’re storing very different pieces of information. (In information theory, this is what is known as ‘low entropy’). ==A====s a reminder, columnar databases store== _==columns==_ ==of data — meaning values with identical types and similar values.== This is far easier to compress compared to row data, even if it comes at the cost of some compute (for decompression during certain operations) when you’re reading values. But overall, this compression means more data may be loaded into memory when you’re running an aggregation query, which in turn results in faster overall queries.
3. The ==final benefit is that compression and dense-packing in columnar databases free up space — space that may be used to sort and index data within the columns. In other words,== **==columnar databases have higher sorting and indexing efficienc==y**, which comes more as a side benefit of having some leftover space from strong compression. It is also, in fact, mutually beneficial: researchers who study columnar databases point out that sorted data compress better than unsorted data, because sorting lowers entropy.

The ==net result of all these properties is that columnar databases give you OLAP cube-like performance without the pain of explicitly designing (and building!) cubes.== It means you can perform _everything you need within your data warehouse_, and skip the laborious busy-work that comes with cube maintenance.

If ==there’s a downside, however, it is that update performance in a columnar database is abysmal (you’ll have to go to every column in order to update one ‘row’);== ==as a result, many modern columnar databases limit your ability to update data after you’ve stored it.== ~~BigQuery, for instance, doesn’t allow you to update data at all — you can only write new data to the warehouse, never edit old bits~~. (Update: the original Dremel paper explained that BigQuery had an append-only structure; this is no longer true as of 2016).

## Conclusion

What is the result of all these developments? The two predictions I made earlier when I was writing about the alternate universe are slowly coming true: smaller companies are less likely to consider data-cube-oriented tools or workloads, and strict dimensional modeling has become less important over time==. More importantly, tech-savvy companies like Amazon, Airbnb, Uber and Google have rejected the data cube paradigm entirely;== these events and more tell me that we are going to see both trends spread into the enterprise over the next decade.

We are only at the start of this change, however. The future may be here, but it is unevenly distributed. This is only to be expected: ==MPP columnar databases have only been around for a decade or so (BigQuery launched in 2010, Redshift in 2012) and we’ve only seen tools that take advantage of this new paradigm (like== [==Looker==](https://looker.com/)==,== [==dbt==](https://www.getdbt.com/) ==and== [==Holistics==](https://www.holistics.io/)==) emerge in the middle of the previous decade.== Things are still really early — and we have a long way to go before large enterprises toss out their legacy, cube-influenced systems and move to the new ones.

These trends may be interesting to you if you are a BI service provider, but let’s focus a little on the implications of these trends on your career. What does it mean for you if you are a data professional and OLAP cubes are on the wane?

As I see it, you’ll have to skate to where the puck is:

* ==Master SQL; the majority of MPP columnar databases have settled on SQL as the de-facto query standard. (More on this== [==here==](https://www.holistics.io/blog/the-rise-of-sql-based-data-modeling-and-dataops/)==).==
* Be suspicious of companies that are heavily locked into the OLAP cube workflow. (Learn how to do this [here](https://www.holistics.io/blog/how-we-combine-data-sources-at-holistics/)).
* F==amiliarise yourself with modeling techniques for the columnar database age== (Chartio is probably the first company I’ve seen with a guide that attempts this; read that [here](https://dataschool.com/data-governance/) … but understand that this is relatively new and the best practices may still be changing).
* ==Use ELT whenever possible (as opposed to ETL); this is a== [==result of the new paradigm==](https://www.holistics.io/blog/etl-vs-elt-how-elt-is-changing-the-bi-landscape/)==.==
* ==Study Kimball, Inmon and data vault methodologies, but with an eye to application in the new paradigm.== Understand the limitations of each of their approaches.

I started this piece with a focus on the OLAP cube as a way to understand a core technology in the history of business intelligence. But it turns out that the invention of the cube has been influential on nearly everything we’ve seen in our industry. Pay attention to its decline: if there’s nothing else you take away from this piece, let it be this: the rise and fall of the OLAP cube is more important to your career than you might originally think.

_**Follow up:** We've published a follow up post titled: [OLAP != OLAP Cube](https://www.holistics.io/blog/olap-is-not-olap-cube/)._

## Sources
