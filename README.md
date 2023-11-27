# Step-by-Step Guide: Dockerizing REST API with MongoDB Atlas using Docker Compose

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
  ├── package.json
  ├── README.md
  ├── rest.http 
  └── ...
```
Run the code 
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

Create a `Dockerfile` in the root of your project:
- To create, run below code in your VScode terminal,
- for Windows
  ```
   type nul > Dockerfile
  ```
- for Linux
  ```
   touch Dockerfile
  ```

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

#### Step 4: Create Docker Compose and dockerignore Configuration

Create a `docker-compose.yml` file in the root of your project:

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
Create a `.dockerignore` file in the root of your project:
```.dockerignore
./node_modules
Dockerfile
.dockerignore
docker-compose.yml
```


#### Step 5: Build and Start Containers

Build and start the Docker containers defined in the `docker-compose.yml`:

```bash
docker-compose up -d
```

#### Step 6: Access Your Application

Access your Node.js application in a web browser at `http://localhost:3000`. MongoDB will be available at `mongodb://localhost:27017`.

#### Step 7: Stopping and Cleaning Up

To stop the application and remove the containers:

```bash
docker-compose down
```

### Additional Notes

- Customize ports, environment variables, or any other configurations in your `Dockerfile` or `docker-compose.yml` based on your application's requirements.
- Ensure sensitive information like database credentials is managed securely, perhaps using environment variables.

This guide should help a new user to Dockerize a Node.js application with MongoDB using Docker Compose. Feel free to modify and expand upon it for your GitHub README!


