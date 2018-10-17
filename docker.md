# Docker


Docker allows our code to be built, distributed and executed in any environment. A container is the fundamental work unit of a docker similar to a virtual machine that emulates either an operating system, an application, a service or a set of the previous ones.

### 1.- Execute containers

```
docker run hello-world // Run a example container
```

### 2.- Basic commands

```
docker ps // Command used to see containers running
```

```
docker ps -a // Check containers that have been executed at some time
```

```
docker rm name_of_container // Remove the container
```

```
docker rm name_of_container -f // Remove the container even if it is running
```

```
docker run --name name_of_container hello-world // Name the container
```

```
docker logs name_of_container // Check the output of the specific container
```

```
docker run ubuntu // Run a container with ubuntu (turns on and off)
```

```
docker run -it ubuntu // Enter a container with ubuntu interactively
```

```
docker run --name db -d mongo // Run the mongo container in background
```

```
docker exec -it name_of_container bash // Execute the container bash command
```

```
docker stop name_of_container // Stops a created container
```

```
docker start name_of_container // Start the container
```

### 3.- Docker Volumes

```
// Create a volume for a mongo database
docker run --name name_of_container -d -v path_of_my_computer:data/db mongo
```

```
docker exec -it name_of_container bash // Enter the bash process of name_of_container
```

```
docker rm -f name_of_container // Delete the container but not the filesystem / volume
```
### 4.- Binding

```
// Bind machine port / container port with the flag -p
docker run -d --name name_of_container -p 27017:27017 mongo
```

```
docker run -d --name name_of_container -p 27017:27017 mongo:3.0 // Binde ports and a specific version of the container
```

### 5.- Docker images

```
docker images // Check installed images
```

```
docker rmi mongo:3.0 // Remove an installed image
```

### 6.- Layouts

```
docker run -it --name changes ubuntu bash // Create an ubuntu image called "changes"
```

If we make a change for example, we create a file in that image and create a new image from it with the following command:

```
docker commit change newimage // We have an image from the previous one with the included change called "newimage"
```

```
docker run --name trynew -it --rm new image bash // An image is created and deleted once it finishes its execution with the flag --rm
```

### 7.- Download docker images

```
docker pull node // Download a node image (the latest available version) without running it
```

```
docker pull node:4.4.7 // Download a node image with the specific version
```

### 8.- Docker build & docker files

Use Docker file and make a build

*Dockerfile*

```
FROM node:4.4.7 // Image from where the container will be created
COPY ["package.json", "/ usr / src /"] // If there are changes in the package.json run the npm install of new account
WORKDIR / usr / src / // Change the path where it is positioned (Path)
RUN npm install // Execute the command according to the path mentioned in the above line
COPY [".", "/ Usr / src /"] // Copy files from where (host machine) / to where (container)
EXPOSE 3000 // Declares the port exhibition
CMD ["node", "index.js"] // Command by default
```

*Dockerignore*

```
node_modules // Ignore the node_modules folder
```

To create the build, the following commands are executed in the same directory:

```
docker build -t test. // Make a build to create the image
```

```
docker images // We can see that we create the image test
```

```
docker run -it --rm bash test // Create a container from the created image
```

```
docker run -p 3000:3000 test // Bin the port and run the server in local
```

### 9.- Docker push and wrap

You need to create your account in the docker hub and then login to the console:

```
docker login
```

```
docker tag test user_of_docker/docker-testing_project
```

```
docker push user_of_docker/docker-testing_project
```


### 10.- Linking containers

Link the container db that was created from mongo with the container of the application using the docker image user_of_docker test:

```
docker run -d --name db mongo // We create the container db
```

```
// Linkeamos db with the container of the application
docker run --name app -d --link db:mongoalias -e MONGO_URL=//mongoalias/test -p 3000:3000 user_of_docker/docker-testing
```

### 11.- Docker Compose

Docker compose describes and executes the architecture of an application.

*docker-compose.yml*

```
version: '2'

services:
  app:
    image: user_of_docker/docker-testing
    environment:
      MONGO_URL: "mongodb://db/test"
    depends_on:
      - db
    ports:
     - "3000:3000"
db:
     image: mongo
```

```
docker-compose up // Raise and execute the architecture of our application
```

Instead of using an image of docker hub for the app we use an image built from the dockerfile, changing image by build in the docker-compose:

```
version: '2'

services:
  app:
    build:.
    # image: user_of_docker/docker-testing
    environment:
      MONGO_URL: "mongodb://db/test"
    depends_on:
      - db
    ports:
      - "3000:3000"
  db:
     image: mongo
```

```
docker compose build // The image will be built from what we passed in the app section of the docker-compose.yml file
```

```
docker-compose up -d // Raise the infrastructure of our project in background or background
```

```
docker-compose ps // Display the containers that are running docker-compose in a friendly way
```

```
docker-compose db logs // Show docker-compose db container logs
```

```
docker-compose logs app // Show the logs of the docker-compose app container
```

```
docker-compose start app // Raise container docker-compose
```

```
docker-compose stop app // Stops docker-compose container app
```

```
docker-compose down // Turn off/Remove the docker compose infrastructure
```
