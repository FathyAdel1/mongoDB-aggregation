//setup mongoDB
sudo docker-compose up 

//MongoDB shell
sudo docker exec -it 319b32df0ad3 bash

//connect mongoDB
mongosh mongodb://localhost:27017 -u root -p rootpassword

//create database
use fathy;

//show collections 
show collections

//insert document
db.inventory.inserOne({})

//insert documents 
db.inventory.insertMany([
    {product:"toothbrush", total:4.75, customer:"Mike"},
    {product:"guitar", total:199.99, customer:"Tom"}, 
    {product:"milk", total:11.33, customer:"Mike"},
    {product:"pizza", total:8.50, customer:"Karen"}, 
    {product:"toothbrush", total:4.75, customer:"Karen"}, 
    {product:"pizza", total:4.75, customer:"Dave"},
    {product:"toothbrush", total:4.75, customer:"Mike"}
])

//show documents
db.inventory.find({})

//count product [toothbrush]
db.inventory.countDocuments({product:"toothbrush"})

//find total of [11.33, 8.5]
db.inventory.find({total:{$in:[11.33, 8.5]}})

//find product [pizza] and customer [Dave]
db.inventory.find({product:"pizza", customer:"Dave"})

//find prodcut [toy] or total [199.99]
db.inventory.find({$or:[{product:"toy"}, {total:199.99}]})

//find product [guitar] and update total [200]
db.inventory.updateOne({product:'guitar'}, {$set:{total:200}})

//delete all documents
db.inventory.deleteMany({})

//find all products
db.inventory.distinct("product")

//find the total amount of money spent to each product 
by the customer name Mike

db.inventory.aggregate([
    {$match:{customer:"Mike"}}, 
    {$group:{_id:"$product", total:{$sum:"$total"}}},
    {$sort:{total:-1}}
])