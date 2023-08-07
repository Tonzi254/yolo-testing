OBJECTIVES
# 1.Git Work Flow
After cloning the supplied repository to my local machine I did a git fetch and then set the remote origin to my repo url.
I changed directory to client and then created a Dockerfile for the frontend of the app and pushed it to the repo
After that I changed directory to backend and created another Dockerfile for the backend of the app and pushed it to the repo
After that I changed directory to the root of the app and create a docker-compose file and created the microservices and pushed the final file to the repo.

# 2.Image Selection
The three images below were created and but only the first two were pushed to Docker Hub
1. Yolo-Client
2. Yolo-Backend
3. Mongo

The total image sizes for the two images was less than 100MB which was achieved by creating a dockerignore file to prevent uploading of node_modules files to Docker Hub

The third image of Mongo was pulled directly from MongoDB. Unfortunately the size is over 600MB but this is due to it being pre-defined and not easy to customize or would take too much time to do so.

# 3.Image Versioning
The two images Yolo-Client and Yolo-Backend were versioned to 1:0:0 following the smever conventions. The default latest image tag was not used so as to be able to distinguish between versions of the front end and backend apps during development. Mongo image was not versioned since it was not user-defined rather it was directly pulled from Docker Hub. 

# 4.Image Deployment
The Yolo-Client 1.0.0 image and Yolo-Backend image were tagged with my username to enable upload to my Docker Hub repository which prevented unnecessary builds of the images everytime the app would be started using docker-compose. Mongo image was not tagged with my username and pushed to Docker Hub since the docker-compose file pulls it direclty from Docker Hub during running of the application.

# 5.Service Orchestration
Three containers below were successfully created and run using docker-compose file
1. Yolo Client
2. Yolo Backend
3. Mongo

To prevent the frontend from starting up first before the backend has started I added a frontend dependency on backend during execution and subsequently I also added a backend dependency on database to prevent the backend from starting before the database. 

Once the docker-compose file is run it starts the database container which in turn triggers the backend container to start which in turn triggers the frontend container to run. The backend app waits for the database to start and then connects to it before allowing the frontend app to connect to the backend which connects it to the database.

I created a custom bridge yolo-network and put the three containers all in the same subnet (172.20.0.0/16) to facilitate the communication between frontend, backend and database which is crucial for proper functioning of the application. The front end is able to connect to the backend via the bridge and the backend is also able to connect to the database using the same bridge.
