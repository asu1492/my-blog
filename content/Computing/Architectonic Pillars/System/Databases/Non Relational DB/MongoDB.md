---
source: https://btholt.github.io/complete-intro-to-databases/mongodb
---
MongoDB

1. ADV
2. Use cases
3. CRUD operations 
4. indexing 
5. aggregation framework 
6. Replication and Sharding 
7. Accessing mongoDB using Python 

**Characteristics of MongoDB**

1. based on NoSQL (It is queried using javascript). 
2. Non relational document database. 
3. Databases and Collections 
	1. Database is group of collection 
	2. Collection is grab bag of documents. e.g. in spreadsheet a row is document and table is collection. 
4. Query language for MD is javascript 
	1. Date.now()
	2. Math.max(1,5)
5. Own internal id system, other DB we have to create id
6. Capable of geospatial query. 
7. Bulk right -- bunch of things all at once. 

What is MongoDb 

1. Document and NOSQL database. 
2. prioritise availability over consistency. (CAP Theorem) 
	
3. ![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.22.png]]
4. ![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.10.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.21.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.16.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.30.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.20.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.18.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.32.png]]

* * *

Adv of MongoDB
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.28.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.25.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.17.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.23.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.19.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.24.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.9.png]]

* * *

![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.12.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.14.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.31.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.11.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.26.png]]

* MongoDB is a document and NoSQL database. 
* It is easy to access by indexing. 
* It supports various data types, including dates and numbers. 
* The database schema can be flexible when working with MongoDB.  
* You can change the database schema as needed without involving complex data definition language statements. 
* Complex data analysis can be done on the server using Aggregation Pipelines. 
* The scalability MongoDB provides makes it easier to work across the globe. 
* MongoDB enables you to perform real-time analysis on your data.

* CRUD operations consist of Create, Read, Update, and Delete. 
* The Mongo shell is an interactive command line tool provided by MongoDB to interact with your databases. 
* Indexes help quickly locate data without looking for it everywhere. 
* MongoDB stores data being indexed on the index entry and a location of the document on disk.  
* Using an aggregation framework, you can perform complex analysis on the data in MongoDB. 
* You can build your aggregation process in stages such as match, group, project, and sort. 
* Replication is the duplication of data and any changes made to the data. 
* Replication provides fault tolerance, redundancy, and high availability for your data. 
* For growing data sets, you can use sharding to scale horizontally. 
* MongoClient is a class that helps you interact with MongoDB

Indexes
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.52.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.39.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.45.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.44.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.49.png]]

* MongoDB stores indexes in tree form 

![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.35.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.37.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.41.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.47.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.53.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.48.png]]![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.51.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.36.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.43.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.33.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.46.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.54.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.40.png]]

![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.42.png]]![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.50.png]]![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.55.png]]![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.34.png]]![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.38.png]]

* * *

* * *

\# creating a mongo database called test-mongo
docker run --name test-mongo -dit -p 27017:27017 --rm mongo:4.4.1

docker exec \-it test-mongo mongo

* p -- port opening from docker container 27017(host computer) and 27017(inside, default port for mongo)
* \--rm : cleanup after done with it 
* mongo is name of container we are pulling from docker hub & 4.4.1 rep the version. 

* * *

\# Creating a new database

1. use name\_of\_database --> it wl create a new database , shows dynamic nature of MongoDB 
2. db.help()
3. db.stats()

* * *

\# Collection 

1. db.collectionName.help() 

**Create Operations** 

db.collectionName.insertOne() -- insertOne is function to add record or a doc in our database. ![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.png]]
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.27.png]]

db.collectionName.insertMany()
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.8.png]]

**Read Operations**

1. \_\_\_.findOne() : return first thing that matches query || findOne({type='dog"})
2. .find()
3. ![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.13.png]]
4. ![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.29.png]]

**Count** 

1. db.collectionName.count()
2. db.collectionName.count({"last\_name" : "Doe"} )

**Replace** 

**Updates** 

1. db.pets.updateOne(); ![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.2.png]]
2. db.pets.updateMany() 
3. Increment age by 1![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.1.png]]
4. Constructing Changes document for update 
	1. ![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.15.png]]
	2. 

1. <https://docs.mongodb.com/manual/reference/operator/update/>

1. "it" - iterate -- records at a time -- by default around 20 records at a time.. 
2. db.collectionNam.limit(40) - limits record to 40. 
3. For better visualization, can append .toarray() -- db.collectionName.limit(5).toArray()
4. Age greater than 12 --- $gt
5. Query Operator : $gt | $gte | $lt | $lte | $eq | $ne -- can also be used with strings 
	1. ![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.4.png]]
6. Logical operators 
	1. db.pets.count({type: "bird", **$and:** \[{age: {$gte: 4}}, {age: {$lte: 8}}\]})
7. Sorting
	1. db.pets.find({type: "dog"}).sort({age: 1}).limit(5) -- ascending 
	2. db.pets.find({type: "dog"}).sort({age: -1}).limit(5) -- descending 
	3. db.pets.find({type: "dog"}).sort({age: 1, breed: 1}).limit(5) -- secondary sorting by breed 
8. Projections 
	1. db.pets.find({type: "dog"}, **{name: 1}**).limit(5) -- it will only pull name from database
	2. lesser bandwidth + added security 
	3. can disallow even id through : db.pets.find({type: "dog"}, {name: 1, \_id: 0}).limit(5)
	4. ![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.3.png]]

+++++++
upsert
+++++++

1. update if exist, else insert ![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.5.png]]

+++++++
delete
+++++++
![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.6.png]]

* * *

Indexes

1. Except Reddis, all databases have index 
2. enable faster querying over million of records. 
3. db.pets.createIndex({name: 1});
4. db.pets.getIndexes()

**db.pets.find({name: "Fido"}).explain("executionStats")**
{
        "queryPlanner" : {
                "plannerVersion" : 1,
                "namespace" : "adoption.pets",
                "indexFilterSet" : false,
                "parsedQuery" : {
                        "name" : {
                                "$eq" : "Fido"
                        }
                },
                "winningPlan" : {
                        "stage" : "**COLLSCAN**", _(collscan actually looks at each entry/record)_ 
                        "filter" : {
                                "name" : {
                                        "$eq" : "Fido"
                                }
                        },
                        "direction" : "forward"
                },
                "rejectedPlans" : \[ \]
        },
        "executionStats" : {
                "executionSuccess" : true,
                "nReturned" : 1067,
                "executionTimeMillis" : 11,
                "totalKeysExamined" : 0,
                "totalDocsExamined" : 9643,
                "executionStages" : {
                        "stage" : "COLLSCAN",
                        "filter" : {
                                "name" : {
                                        "$eq" : "Fido"
                                }
                        },
                        "nReturned" : 1067,
                        "executionTimeMillisEstimate" : 0,
                        "works" : 9645,
                        "advanced" : 1067,
                        "needTime" : 8577,
                        "needYield" : 0,
                        "saveState" : 9,
                        "restoreState" : 9,
                        "isEOF" : 1,
                        "direction" : "forward",
                        "docsExamined" : 9643
                }
        },
        "serverInfo" : {
                "host" : "ed9a41d9b63d",
                "port" : 27017,
                "version" : "4.4.1",
                "gitVersion" : "ad91a93a5a31e175f5cbf8c69561e788bbc55ce1"
        },
        "ok" : 1

![[Computing/Architectonic Pillars/System/Databases/_resources/MongoDB.resources/unknown_filename.7.png]]

* * *

* * *

**Primaries, Secondaries and Replica Sets** 

1. Group of primaries and secondaries are called replica sets.  
2. Secondaries have copies of primaries but are not consistent (Remember ACID), rather eventually consistent. 
3. All the running things are called MongoDaemon process or Mongos 

**Arbiters and Elections**

1. If primary goes down, MongoDB holds an election and elects a new primary based on factors like 
	1. lowest latency
	2. most up to date
2. Once old primary up and running, it becomes secondary. 
3. Entire purpose of Arbiters is to vote. They are not secondaries. 

**Sharding** 

1. Enable to handle large amount of data. 
2.
