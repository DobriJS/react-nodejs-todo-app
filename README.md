# TODO App with ReactJS, NodeJS and MongoDB

### The TODO App is a simple app for adding tasks, and the primary goal is to Dockerize it !! 
The source code is provided in the repo, so hit in command line tool of your choice:
`git clone https://github.com/DobriJS/react-nodejs-todo-app.git`

### The application uses three services, React for frontend, NodeJS for backend and MongoDB for database.
It is required to create **separate Docker containers and connect them in two networks.**
![pic](https://www.pulumi.com/blog/fullstack-pulumi-mern-stack-digitalocean/tiers.png)


### Requirements
-  Name the three containers "frontend", "backend" and "mongo"
-  Build images from the provided Dockerfiles for the frontend and backend services
-  Use the latest image for MongoDB from Docker Hub
-  Expose the frontend service on port 3000 (see on which port the app works by yourself)
-  Mount the following host directories as volumes: for mongo service: **./data:/data/db**
-  Connect the frontend and backend services to the react-express network and the backend and mongo services to the express-mongo network

### Solution Example
Now we need to execute some commands to get the job done ;). Alright, let's get started !
1) Again, from you preffered console enter the following commands:
- `cd .\frontend` then build the backend image `docker build -t frontend . ` don't forget the dot at the end !!
2) Now, while you are waiting the image to be created, you can create docker networks, theu should be responsible to connect our backend service with the database and frontend to the backend:
- `docker network create react-express` then `docker network create express-mongo`
3) It's time to create our second image:
- `cd ..\backend\` then `docker build -t backend .`
4) Next step is to run a MongoDB container:
- `docker run -d -v .\data:/data/db --network express-mongo --name mongo mongo:latest`
5) And here comes the React container with ports forwarded:
- `docker run -d --name frontend --network react-express -p 3000:3000`
6) We need to start the backend container so far
- `docker run -d --name backend --network react-express backend`
7) Here we need to connect our backend container to express-mongo network that we created earlier:
- `docker network connect express-mongo backend`
8) Now, lets check that everything work correctly, type:
  `docker network inspect express-mongo`
  `docker network inspect react-express`
9) Run in the browser `localhost:3000` and add your daily tasks
