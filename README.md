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
