## demo app - developing with Docker

This demo app shows a simple user profile app set up using 
- index.html with pure js and css styles
- nodejs backend with express module
- mongodb for data storage

All components are docker-based


### With Docker


#### To start the application


Step 1: Create docker network

    $ docker network create mongo-network


Step 2: start mongodb

    $ docker run -d -p 27017:27017 \
        -e MONGO_INITDB_ROOT_USERNAME=admin \
        -e MONGO_INITDB_ROOT_PASSWORD=password \
        --name mongodb --net mongo-network \
        mongo


Step 3: start mongo-express

    $ docker run -d -p 8081:8081 \
        -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
        -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
        --net mongo-network --name mongo-express \
        -e ME_CONFIG_MONGODB_SERVER=mongodb \
        mongo-express


_NOTE: creating docker-network in optional. You can start both containers in a default network. In this case, just emit `--net` flag in `docker run` command_


Step 4: open mongo-express from browser

    http://localhost:8081


Step 5: create `user-account` _db_ and `users` _collection_ in mongo-express


Step 6: Start your nodejs application locally - go to `app` directory of project 

    $ npm install 
    $ node server.js


Step 7: Access you nodejs application UI from browser

    http://localhost:3000


### With Docker Compose


#### To start the application

Step 1: create the cocker compose file (docker-compose.yaml)

    version: '3'
    services:
    # my-app:
    # image: ${docker-registry}/my-app:1.0
    # ports:
    # - 3000:3000
    mongodb:
        image: mongo
        ports:
        - 27017:27017
        environment:
        - MONGO_INITDB_ROOT_USERNAME=admin
        - MONGO_INITDB_ROOT_PASSWORD=password
        volumes:
        - mongo-data:/data/db
    mongo-express:
        image: mongo-express
        ports:
        - 8080:8081
        environment:
        - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
        - ME_CONFIG_MONGODB_ADMINPASSWORD=password
        - ME_CONFIG_MONGODB_SERVER=mongodb
    volumes:
    mongo-data:
        driver: local


Step 2: start mongodb and mongo-express

    $ docker-compose -f docker-compose.yaml up

_You can access the mongo-express under localhost:8080 from your browser_


Step 3: in mongo-express UI - create a new database "my-db"


Step 4: in mongo-express UI - create a new collection "users" in the database "my-db"


Step 5: start node server

    $ npm install
    $ node server.js


Step 6: access the nodejs application from browser 

    http://localhost:3000


#### To build a docker image from the application

    $ docker build -t my-app:1.0 .

_The dot "." at the end of the command denotes location of the Dockerfile._


END.

