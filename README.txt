//setup mongoDB
sudo docker-compose up 

//MongoDB shell
sudo docker exec -it 319b32df0ad3 bash

//connect mongoDB
mongosh mongodb://localhost:27017 -u root -p rootpassword
