
# Mongo DB
Mongo DB is a document-oriented database. It is an open source product which is developed and supported by a company named 10gen



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

Replication,Replica set is a group of two or more nodes (generally minimum 3 nodes are required). In a replica set, one node is primary node and remaining nodes are secondary. All data replicates from primary to secondary node. At the time of automatic fail-over or maintenance, election establishes for primary and a new primary node is elected. After the recovery of failed node, it again join the replica set and works as a secondary node.

Sharding,It is the process of storing data records across multiple machines and it is MongoDB’s approach to meeting the demands of data growth. Sharding solves the problem with horizontal scaling. With sharding, you add more machines to support data growth and the demands of read and write operations. 



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
