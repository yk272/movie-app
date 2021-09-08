# movies-app

# Introduction

This repository is the result of the tutorial to develop your first MERN application and you can find it [here](https://medium.com/@samarony.barros/how-to-create-your-first-mern-mongodb-express-js-react-js-and-node-js-stack-7e8b20463e66)

## What you should install?

For this project, I decided to use the MERN (MongoDB, Express.js, React.js, and Node.js) technology.
![mern](https://miro.medium.com/max/678/1*dqvlaszRLvoPmARpOlLN9A.png)

Firstly, you should install

-   [Mongo](https://www.mongodb.com/) 4.0.4+
-   [ExpressJS](https://expressjs.com/) 4.16.3+
-   [ReactJS](https://reactjs.org/) 16.5.0+
-   [Node](https://nodejs.org/en/) 11.4.0+ (It's recommended to use 10.15.1 LTS)

## Download

You can download the folder on my [GitHub](https://github.com/samaronybarros/) or you can do this directly on [this link](https://github.com/samaronybarros/movies-app).

If you have git installed on your PC, you just need do as follow:

```
$ git clone https://github.com/samaronybarros/movies-app.git
```

## Configuring App

If you have all the prerequisites installed you should verify if your MongoDB is up.

```
$ cd movies-app
$ cd server
$ yarn install
$ nodemon index.js
```

```
$ cd movies-app
$ cd client
$ yarn install
$ yarn start
```
## Steps to dockerize 

### Step 1 : Create the docker files for client and server

1. Go to client directory root and create

```
touch Dockerfile.client

```

2. Add the following content to the client dockerfile

```
FROM node:14

WORKDIR /client

ENV PATH /client/node_modules/.bin:$PATH

COPY package*.json ./

COPY yarn.lock ./

RUN yarn

COPY . ./

CMD ["yarn", "start"]
```

3. Create a .dockerignore file

```
touch .dockerignore
```

4. Add the following content to the .dockerignore file

```
node_modules
npm-debug.log
build
.dockerignore
**/.git
**/.DS_Store
**/node_modules
```

5. Go to server directory root and create a docker 

```
touch Dockerfile.server

```
6. Add the following content to the server dockerfile

```
FROM node:14

WORKDIR /server

COPY package*.json ./

RUN npm install

COPY . .

CMD [ "node", "index.js" ]
```

7. Create a .dockerignore file

```
touch .dockerignore
```

8. Add the following content to the .dockerignore file

```
node_modules
npm-debug.log
build
.dockerignore
**/.git
**/.DS_Store
**/node_modules
```
### Step 2 : Create docker-compose file 

1. Go to the project root ,create docker-compose file along with the server and client folder

```
touch docker-compose.yml
```

2. Add the following content to the docker-compose.yml file

```
version: "3.9" 
services:
  mongodb:
    network_mode: host
    image: mongo
    container_name: mongodb
    volumes:
      - mongodb-data:/data/db
  server:
    network_mode: host
    build:
      context: ./server
      dockerfile: Dockerfile.server
    container_name: movie-server
    depends_on:
      - mongodb
  client:
    network_mode: host
    build:
      context: ./client
      dockerfile: Dockerfile.client
    container_name: movie-client
    depends_on:
      - server
volumes:
  mongodb-data: {}
```

### Step 3 : Bringing up/down the containers

1. Bringing up containers

```
docker-compose up -d
```

2. Bringing down the containers

```
docker-compose down
```

3. Bringing down the containers along with the created images

```
docker-compose down --rmi all
```