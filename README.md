# Step-by-Step Guide: Dockerize a Node.js application with MongoDB using Docker Compose
This guide should help a new user to Dockerize a Node.js application with MongoDB using Docker Compose.

#### Prerequisites

Before starting, ensure you have the following installed:

- [Docker Desktop](https://docs.docker.com/get-docker/): Install the latest version of Docker Desktop.
- [Git client](https://git-scm.com/downloads): Install a Git client for version control.
- IDE or Text Editor: Docker recommends using [Visual Studio Code](https://code.visualstudio.com/) for editing files.

#### Step 1: Clone the Repository

Clone the repository to your local machine:

```bash
git clone https://github.com/Sivarajah-Mouley/Docker-with-REST-Api-MongoDB.git
cd Docker-with-REST-Api-MongoDB
```

#### Step 2: Set Up Your Node.js Application

Ensure your Node.js application has the necessary structure:

```
your-nodejs-app/
  ├──Contorllers
      └──Product.Controller.js
  ├──Models
      └──Porducts.route.js
  ├──Routes
      └──Porducts.model.js
  ├──.env
  ├──app.js         
  ├──package.json   
  ├──package.json
  ├──README.md
  ├──rest.http 
  └── ...
```
Run the code to install node modules and dependencies
```bash
npm install
```
Put your credentials in the .env file.

```bash
PORT=3000
MONGODB_URI=YOUR MONGODB URI
DB_NAME=DATABASE NAME OF YOUR CHOICE
DB_USER=DATABASE USER
DB_PASS=DATABASE USER PASSWORD 
```


#### Step 3: Create Dockerfile

Create a `Dockerfile` in the root of your project where the ```package.json``` file is located :
- To create, run below code in your VScode terminal,
- for Windows
  ```
   type nul > Dockerfile
  ```
- for Linux
  ```
   touch Dockerfile
  ```
Copy the following code into it:
```Dockerfile
# Use Node.js as the base image
FROM node:alpine

# Create and set the working directory in the container
WORKDIR /usr/src/app


# Copy package.json and package-lock.json to the working directory
COPY package*.json .

# Install Node.js dependencies
RUN npm ci

# Copy the application files to the working directory
COPY . .


# Expose the port your app runs on (e.g., 3000)
EXPOSE 3000

# Start the Node.js application
CMD ["npm", "start"]
```

#### Step 4: Create Docker Compose and Dockerignore Configuration

Create a `docker-compose.yml` file in the root of your project:
- for Windows
  ```
   type nul > docker-compose.yml
  ```
- for Linux
  ```
   touch docker-compose.yml
  ```
- You can use the ```.ymal``` extention also instead of ```.yml```.
  
Copy the following code inside it:
```yaml
version: '3.9'
services:
  mongo_db:
    image: mongo:latest
    container_name: m1
    restart: always
    volumes:
      - mongo_db:/data/db

  # NodeJS Services
  api: 
      build: .
      container_name: api
      restart: always
      ports:
        - 4000:3000 
      depends_on:
        - mongo_db

      environment:
        PORT: 3000
        MONGODB_URI: mongodb+srv://<user name>:<password>@cluster0.zwbrtkl.mongodb.net/<DB_NAME>?retryWrites=true&w=majority
        DB_NAME: <your prefered name>
        # DB_USER:
        # DB_PASS:
volumes:
  mongo_db: {}
```
- Do not forget to change the environment part inside the services here, as you changed in the ```.env``` file.
  
Create a `.dockerignore` file in the root of your project:
- for Windows
  ```
   type nul > .dockerignore
  ```
- for Linux
  ```
   touch .dockerignore
  ```
Copy the following code into it:
```.dockerignore
./node_modules
Dockerfile
.dockerignore
docker-compose.yml
```


#### Step 5: Build and Start Containers

Build and start the Docker containers defined in the `docker-compose.yml` :
- Make sure your Mongo Services is in running status.
- Open the docker Desktop application inorder to initiate the Docker Engine. And you have to pull mongo base image into your Docker desktop.
Run the following command to build the image:
```bash
docker build -t mongodb-con .
```
Here, ```mongodb-con``` is image name and ```.``` shows the relative path to create it. Change the image name as you wish.
- Here, you can use the above same command with having a Tag name for container to identify it easily.
```
docker build -t mongodb-con:mongocontainor
 ```
Here, ```mongocontainor``` is tag name.

Run the below command to initiate the container in detached mode:
```bash
docker compose up -d
```
- Here, if you want to see the background running details in terminal then ignore ```-d``` and run the above code.
Once you have run this code you will have running container for this application. 
- In order to see those containers, run the below code in terminal:
```bash
docker ps
```

#### Step 6: Access Your Application

Access your Node.js application in a web browser at `http://localhost:4000/products`. MongoDB Atlas will be available at `mongodb://localhost:27017`.
Now inorder to do the REST api function you have to enable one more extention in the VSCode.
- Open the VScode and go to the extentions and search for ```REST client``` and install the extention.
- Open the ```rest.http``` file and click the ```Send Request``` above each function.
- You can find your results in the 'REST cilent' window and also in the `http://localhost:4000/products`.

#### Step 7: Stopping and Cleaning Up

To stop the application and remove the containers:

```bash
docker compose down
```

### Additional Notes

To check your Docker version, open cmd in your machine and run the below command: 
```bash
docker --version
```
- If you have version before V20.10.8 then you have to use ```docker-compose``` to up or down the container.
- If you have after V20.10.8 then you have to use ```docker compose``` to up or down the container.

In order to get stopped but not deleted containers details, run the below code in terminal:
```bash
docker ps -a
```
- From the above code's output you can get the Name,containerId, Tagname,Port,etc. 

To delete only a specific containor, after copy the tagname or ContainerId Run the below code :
```bash
docker rm <ContainorID>
```
- Here, replcae the ```<ContainorID>``` with your actual value.

To stop only one running containor, run the below code:
```bash
docker stop <ContainorName>
```
To run a containor that was already stopped, run the below code:
```bash
docker start <ContainorName>
```
- Here, replace the ```<ContainorName>``` with your actual value.

To view all the images:
```bash
docker images
```
To run a new image for create containor with port,containorName:
```bash
docker run -d -p 4000:3000 --name my-app mongoapp:v1
```
Here,
- ```-d```       => To run in detached mode
- ```-p```       => Define the port configuration
- ```4000```     => App running port address in your local machine
- ```3000```     => App running port address in Docker Containor
- ```my-app```   => Name for the containor ( change as you wish)
- ```mongoapp``` => Excisting image name
- ```v1```       => Tag Name (Version of your app)


- Customize ports, environment variables, or any other configurations in your `Dockerfile` or `docker-compose.yml` based on your application's requirements.
- Ensure sensitive information like database credentials is managed securely, perhaps using environment variables.
