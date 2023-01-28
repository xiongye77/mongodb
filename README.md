# mongodb


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


