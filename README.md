
# Mongo DB
Mongo DB is a high-performance document-oriented database that is powered by a NoSQL structure. It makes use of collections (tables) each having multiple documents (records) & allows the user to store data in a non-relational format. 

MongoDB stores its data as objects which are commonly identified as documents. These documents are stored in collections, analogous to how tables work in relational databases. MongoDB is known for its scalability, ease of use, reliability & no compulsion for using a fixed schema among all stored documents, giving them the ability to have varying fields (columns).

Mongos = MongoDB Shard Utility, the controller and query router for sharded clusters. Sharding partitions the data-set into discrete parts.

Mongod = The primary daemon process for the MongoDB system. It handles data requests, manages data access, and performs background management operations.

The primary purpose of building MongoDB is:-

Scalability

Performance

High Availability

Scaling from single server deployments to large, complex multi-site architectures




Understanding basic terminology in mongo. MongoDB works on concept of collection and document.

Database is a physical container for collections. Each database gets its own set of files on the file system. A single MongoDB server typically has multiple databases.


Collection is a group of MongoDB documents. It is the equivalent of an RDBMS table. A collection exists within a single database. Collections do not enforce a schema. Documents within a collection can have different fields. Typically, all documents in a collection are of similar or related purpose.


Document is a set of key-value pairs, have dynamic schema which means that documents in the same collection do not need to have the same set of fields or structure, and common fields in a collection’s documents may hold different types of data.


Indexes exist primarily to improve the performance of MongoDB queries and other operations. Choosing the best set of indexes is perhaps the most important single thing you can do to improve the performance of your MongoDB applications.



Some other concepts one be dealing frquently in Mongo include:-

Aggregation,These are operations that process data records and return computed results. Aggregation operations group values from multiple documents together, and can perform a variety of operations on the grouped data to return a single result.

Replication,
![image](https://user-images.githubusercontent.com/36766101/215297847-14551d26-8ca2-4f9d-bbf7-35b9f9d70c90.png)

To monitor the overall replication status, use the rs.status()


rs.conf() : get the current configuration object

rs.printReplicationInfo() : check oplog size and time range


rs.printSecondaryReplicationInfo() : check replica set members and replication lag

Replica set is a group of two or more nodes (generally minimum 3 nodes are required). In a replica set, one node is primary node and remaining nodes are secondary. All data replicates from primary to secondary node. At the time of automatic fail-over or maintenance, election establishes for primary and a new primary node is elected. After the recovery of failed node, it again join the replica set and works as a secondary node.

Sharding,
![image](https://user-images.githubusercontent.com/36766101/215297944-37cd2c2e-deec-47ab-b136-a1182ed5f059.png)
![image](https://user-images.githubusercontent.com/36766101/215318961-81114880-913a-4e1d-9fae-a02656a528e8.png)

![image](https://user-images.githubusercontent.com/36766101/215319097-644af85c-4e69-426e-8777-f4e1d1ea982c.png)


![image](https://user-images.githubusercontent.com/36766101/215319137-999c58fe-2687-4ede-805d-b201165c2f1e.png)

It is the process of storing data records across multiple machines and it is MongoDB’s approach to meeting the demands of data growth. Sharding solves the problem with horizontal scaling. With sharding, you add more machines to support data growth and the demands of read and write operations. 

Sharding makes use of three components:

Shards: It is the location where the data is stored.
Config Server: These servers help map data from a cluster to a Shard, which is then used by query routers to perform operations specific to a particular Shard. 
Query Server: These servers allow users to access and perform operations on the desired MongoDB Shards.

![image](https://user-images.githubusercontent.com/36766101/215319189-b6dcb5e8-26ac-48d3-94f8-d56e66b88c75.png)

# When sharding and replication work together, they are referred to as a shared cluster. Each shard is replicated to preserve the same data availability

Backup/Restoure use mongodump and mongorestore


mongodump --host sample-cluster.node.us-east-1.docdb.amazonaws.com --port=27017 --username <username> --password <password> --out=<path where you want the backup>

mongorestore --host sample-cluster.node.us-east-1.docdb.amazonaws.com --port=27017 --username <username> --password <insertYourPassword> --drop <Restore path of DB>
   
  
mongodb://mongoadmin:<insertYourPassword>@sample-cluster.node.us-east-1.docdb.amazonaws.com:27017/?replicaSet=rs0&readPreference=secondaryPreferred&retryWrites=false
  
  
  
Monitor 
db.serverStatus
The db.serverStatus() method is used to return a document that provides an overview of the state of the database process.

db.stats()
  
db.serverStats()  

db.adminCommand( { getLog: "*" } )

  
db.adminCommand( { getLog : "global" } )
   
   
db.adminCommand( { getLog : "startupWarnings" } )
   
db.adminCommand({logRotate : 1 } )   

![image](https://user-images.githubusercontent.com/36766101/215297401-e9dac391-59ec-4cfc-aa2c-852e5f7e8955.png)
![image](https://user-images.githubusercontent.com/36766101/215297475-8dacd1ea-8af5-4f3b-aeee-aa39340f2676.png)

db.collectionname.stats()  
db.collectionname.dataSize()
db.collectionname.storageSize()
db.collectionname.totalSize()
db.collectionname.totalIndexSize()  

Create a New Collection and Load Data into It

mongoimport --db test --collection users --type csv --headerline --file users.csv


db.users.find()


db.users.find( { name: 'Betty' } )


db.users.updateOne( { name: 'Betty' }, { $set: { department: 'Engineering' } } )


db.users.find( { name: 'Betty' } )


db.users.find( { name: 'David' } )

db.users.deleteOne( { name:'David' } )

![image](https://user-images.githubusercontent.com/36766101/215250654-ea6f37b1-7b62-40e6-b3bb-1d7275a42f2c.png)




db.posts.aggregate([
  // Stage 1: Only find documents that have more than 1 like
  {
    $match: { likes: { $gt: 1 } }
  },
  // Stage 2: Group documents by category and sum each categories likes
  {
    $group: { _id: "$category", totalLikes: { $sum: "$likes" } }
  }
])

![image](https://user-images.githubusercontent.com/36766101/215256859-98598daa-5621-428d-ac6b-369f700845ed.png)


![image](https://user-images.githubusercontent.com/36766101/215257331-00780af2-211b-47dd-8027-e9e955ee46c1.png)



schema validation

![image](https://user-images.githubusercontent.com/36766101/215257883-8717b3e7-3466-4fab-9be3-ad3df8c96154.png)

![image](https://user-images.githubusercontent.com/36766101/215257909-9312b20a-91b8-4341-85d9-b5d7ecd7a633.png)

![image](https://user-images.githubusercontent.com/36766101/215257971-88cb2e37-7822-4988-9f96-4cac2415df36.png)




# Choose the Correct Data Model for MongoDB Application

Load Data Files into a New Collection
Check the contents of the current directory for the two CSV files needed for this lab:

ls
View the checkout.csv file to see more information on books being checked out:

cat checkout.CSV
View the inventory.csv file to see more information on the inventory of books:

cat inventory.CSV
Load the inventory.csv file into a new inventory collection in the library database:

mongoimport --db library --collection inventory --type csv --headerline --ignoreBlanks --file inventory.CSV
Load the checkout.csv file into a new checkout collection in the library database:

mongoimport --db library --collection checkout --type csv --headerline --ignoreBlanks --file checkout.CSV
Once the files are loaded, connect to our MongoDB instance:

mongosh library 
View the data files:

db.inventory.find()
db.checkout.find()
Normalize Publisher Information
To normalize the publisher information to a new collection, create a new publishers collection with the publisher name and contact using this aggregate function:

 db.inventory.aggregate([ { $project: { publisher_name: 1, publisher_contact: 1}}, {$out: 'publishers'} ]) 
View the new publishers collection:

db.publishers.find()
Remove the publisher contact information from the inventory collection:

db.inventory.updateMany( {}, {$unset: { publisher_contact: "" } } )
View the inventory collection to make sure the publisher contact information has been moved to a new collection:

db.inventory.find()
Denormalize the Checkout Information
To denormalize or embed the checkout information into our inventory collection, use the following aggregate function:

db.inventory.aggregate([
{
  $lookup: {
     from: "checkout",
     localField: "_id",    // field in the inventory collection
     foreignField: "book_id",  // field in the checkout collection
     as: "fromItems"
  }
},
{
  $replaceRoot: { newRoot: { $mergeObjects: [ { $arrayElemAt: [ "$fromItems", 0 ] }, "$$ROOT" ] } }
},
{ $set: { checkout: [ {by: "$name", date: "$date" } ] } },
{ $project: { fromItems: 0, name: 0, date: 0, book_id: 0 } },
{ $out: 'inventory' }
])   

View the inventory information to make sure the checkout information is included in the collection:

db.inventory.find()
   
# MongoDB Server Parameters   MongoDB provides a number of configuration options that you can set using
db.adminCommand( { setParameter: 1, <parameter>: <value>  } )
   
   
db.adminCommand({
   "setParameter": 1,
   "wiredTigerEngineRuntimeConfig": "<option>=<setting>,<option>=<setting>"
})

   
   
# Create index
![image](https://user-images.githubusercontent.com/36766101/215305171-fb92b03e-9a4a-4e81-a9e2-96c60498d567.png)
   
![image](https://user-images.githubusercontent.com/36766101/215305193-0261e390-a623-478d-bbdc-4f4f8265017b.png)

# Using mongostat and mongotop to Obtain Real-Time Database Statistics   
   
# MongoDB Profiler to use it to query a database by providing details about a result you want through a query, get query level insights by finding slow queries, setting filters to determine slow queries, specifying the threshold for slow queries, etc
   
Enabling and Configuring MongoDB Profiler
Level	Description
0	This is the default profiler level set off and it does not collect any data. mongod writes operations longer than the slowOpThresholdMs threshold to its log.
   
1	This level collects profiling data for slow operations only. Slow operations are regarded as operations that are slower than 100 milliseconds by default though, you can use the slowOpThresholdMs runtime option or the setParameter command to modify your definition of slow operations.
   
2	This level of the profiler collects profiling data for all the database operations.
   
db.setProfilingLevel(1, { slowms: 20 })
   
//To return operations for a particular collection, type the query similar to the one below and it will return operations found in the mydb database’s test collection:   
db.system.profile.find( { ns : 'mydb.test' } ).pretty()
   
   
// Find something that took more than 20ms
   
db.system.profile.find ({"millis": {$ gt: 20}})    
   
//Find all queries doing a COLLSCAN because there is no suitable index
// all queries that do a COLLSCAN
db.system.profile.find({"planSummary":{$eq:"COLLSCAN"},"op":{$eq:"query"}, "ns":{$eq: "mydb.test" }).sort({millis:-1})
![image](https://user-images.githubusercontent.com/36766101/215307477-b0cc4ae1-693f-4783-a9f1-31e6858f6fde.png)
   
db.system.profile.find({"planSummary":{$eq:"COLLSCAN"},"op":{$eq:"query"}}).sort({millis:-1})   

   
# MongoDB FTDC  (Full Time Diagnostic Data Capture) captures server behavior and stores it under a separate directory (data directory/diagnostic.data). This is mainly for MongoDB engineers, to process/analyze mongod and mongos when needed ( troubleshooting). FTDC data files are compressed and not human-readable. MongoDB Inc. engineers cannot access FTDC data without explicit permission and assistance from system owners or operators.


# mongodump 
![image](https://user-images.githubusercontent.com/36766101/215375328-8e66af28-2eea-45e1-bd71-e13135d71027.png)


![image](https://user-images.githubusercontent.com/36766101/215375524-6da7849e-bad2-4ecf-be16-d17bf299d0d6.png)

![image](https://user-images.githubusercontent.com/36766101/215375296-6c72b5f3-8220-4941-908c-78dd471df078.png)


   
   
![image](https://user-images.githubusercontent.com/36766101/215378831-a8fe65b2-5485-4753-b084-b5588224e84c.png)
![image](https://user-images.githubusercontent.com/36766101/215378872-a4637671-a284-49dd-9a90-1b689c8d00bc.png)
   

   
   
 ![image](https://user-images.githubusercontent.com/36766101/215469406-ab7bb70e-4809-4bd4-9603-c5abe07d670e.png)

# Azure Cosmos DB
relational data  schema on write 
![image](https://user-images.githubusercontent.com/36766101/226238137-67a48ce2-5727-47b5-8ec5-5c8087523658.png)

non-relational data  schema on read
![image](https://user-images.githubusercontent.com/36766101/226238592-9cc07e5c-176c-43af-a16d-3399b6ea37de.png)

![image](https://user-images.githubusercontent.com/36766101/226239978-59e5314e-d1c0-412a-8a0d-01e095988f4c.png)
![image](https://user-images.githubusercontent.com/36766101/226240251-3e8316ec-2405-4094-8b5c-d368b2daf9bd.png)
![image](https://user-images.githubusercontent.com/36766101/226241074-b7fd63a0-5d48-45a7-aee9-4dd05f6d9f6b.png)
![image](https://user-images.githubusercontent.com/36766101/226298413-10e0db7b-ebf0-4c86-8e6a-17abb9b04ce9.png)
![image](https://user-images.githubusercontent.com/36766101/226299070-15044dcf-805e-4eba-b7d1-98b339dd73c2.png)
![image](https://user-images.githubusercontent.com/36766101/226299338-18879649-6266-4ff2-87d6-73f6fce124c3.png)
![image](https://user-images.githubusercontent.com/36766101/226300127-fd0769fb-8b9a-4a76-8acd-f9a30c45fc1a.png)
![image](https://user-images.githubusercontent.com/36766101/226301075-56dc0e92-2797-4883-9782-142abfac0f85.png)
![image](https://user-images.githubusercontent.com/36766101/226303464-062317b2-6f53-4438-b23d-928207f95e00.png)
![image](https://user-images.githubusercontent.com/36766101/226322682-d974f91a-2d59-4b4d-85a2-97ccdc5a816b.png)
![image](https://user-images.githubusercontent.com/36766101/226329200-616e6283-9e72-4fbe-a34e-715c6179df6f.png)
![image](https://user-images.githubusercontent.com/36766101/226490214-a7968a3e-4b24-44ec-9c75-1e1df415373c.png)
![image](https://user-images.githubusercontent.com/36766101/226490954-04440d48-39f3-450a-aa9c-d4d0f1bc90d6.png)
