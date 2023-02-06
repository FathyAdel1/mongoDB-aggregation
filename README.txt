//setup mongoDB
sudo docker-compose up 

//MongoDB shell
sudo docker exec -it 319b32df0ad3 bash

//connect mongoDB
mongosh mongodb://localhost:27017 -u root -p rootpassword

//create database and name it graphicode
use graphicode

//to show all collections inside the db
show collections

//insert one document
db.playSchool.insertOne({ name: "stefan", age: 23 })

//insert many documents 
db.playSchool.insertMany([{ name: "Maxmillian", age: 25 }, { name: "Alex", age: 27 }])

//get all documents
db.playSchool.find({}).pretty()

//get the document that have name of stefan 
db.playSchool.findOne({ name: "stefan" })

//get all documents that age is equal to 27
db.playSchool.find({ age: 27 })

//get all documents that age is greater than or equal 23 
db.playSchool.find({ age: { $gte: 23 }})

//get all documents that the age is [23, 25]
db.playSchool.find({ age: { $in: [23, 25] }})

//get all documents that the age is gt 27 or name is stefan 
db.playSchool.find({ $or: [{ age:{ $gt: 27 }}, { name: "stefan" }]})

//update the age of the name of stefan 
db.playSchool.updateOne({ name: "stefan" }, { $set: { age: 24 }})

//delete the attribute age in the name of tony
db.playSchool.updateOne({name:"tony"}, {$unset:{age:1}})

//rename the attribute name in the name of stefan 
db.playSchool.updateOne({name:"stefan"}, {$rename:{name:"username"}})

//increment the age of stefan by one
db.playSchool.updateOne({name:"stefan"}, {$inc:{age:1}})

//mulibly the age of stefan to the number 2
db.playSchool.updateOne({name:"stefan"}, {$mul:{age:2}})

//insert complex nested data
db.playSchool.insertOne({name:"eliot", labs:[
    {info:{age:{number:28}}},
    {info:{age:{number:32}}},
    {info:{age:{number:50}}},
    {info:{age:{number:66}}},
    {info:{age:{number:80}}}
]})

//update the age property inside info object
db.playSchool.updateOne({"labs.info.age.number":50}, {$set:{"labs.$.info.age.number":100}})

//insert complex nested data 
db.playSchool.insertOne({
    name:"eliot",
    players:[
        {name:"stefan", age:23},
        {name:"Max", age:25},
    ]
})

//push new objects to the players array
db.playSchool.updateOne({name:"eliot"}, {$push:{players:[
    {name:"stefan", age:22},
    {name:"maxmillian", age:23},
    {name:"alex", age:25}
]}})

//remove players that thier age greater than 20
db.playSchool.updateOne({name:"eliot"}, {$pull:{players:{age:{$gt:20}}}})

//remove players array 
db.playSchool.updateOne({name:"eliot", {$set:{players:[]}}})

//insert many objects in the db
db.purchase.insertMany([
    {product:"toothbrush", total:4.75, customer:"mike"},
    {product:"guitar", total:199.99, customer:"tom"},
    {product:"milk", total:11.33, customer:"mike"},
    {product:"pizza", total:8.50, customer:"karen"},
    {product:"toothbrush", total:4.75, customer:"karen"},
    {product:"pizza", total:4.75, customer:"dave"},
    {product:"toothbrush", total:4.75, customer:"mike"},
])

//count the products that have name of toothbrush
db.purchase.countDocuments({product:"toothbrush"})

//print the names of products that sold in array
db.purchase.distinct("product")

//find the total amount of money spend by each product
db.purchase.aggregate([
    {$match:{total:{$gt:15}}},
    {$group:{_id:"$product", total:{$sum:"$total"}}},
    {$sort:{total:-1}}
])