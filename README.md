FROM node:alpine – 
Uses a lightweight Node.js image based on Alpine Linux to minimize image size and increase build efficiency.
LABEL author="Dan Wahlin"- 
Improves documentation by adding metadata, such as the author's name.

Environment Configuration:
       ARG PACKAGES=nano
      ENV TERM xterm
    RUN apk update && apk add $PACKAGES
ARG defines a build-time variable for optional packages (such as nano).
	ENV defines environment variables (TERM stands for terminal compatibility).
RUN refreshes the Alpine package management and installs the required tools.
Set Working Directory: 
WORKDIR /var/www
Defines the working directory inside the container where application files are stored.

Copy Dependency Files:
COPY . ./
Copies all application files into the container.

Define Entrypoint:
 ENTRYPOINT ["npm", "start"]
Specifies the command to run when the container starts (launches the application).

Explanation of the docker-compose.yml
Version 3.7- The Docker Compose file format version is specified here. 

Services: 
Node.js Application (node) container_name: Gives the container a specific name (nodeapp).

Image  :Specify the image name (nodeapp) for the container.

build:
Context: Defines the directory (.) that contains the Dockerfile and application code.
dockerfile: Specifies which Dockerfile (node.dockerfile) to use.
args: Adds build arguments such as additional tools (nano, wget, curl) to the Dockerfile.
Port mapping: Redirects container port 3000 to host port 3000.

Connects the service to a customized network (nodeapp-network).
Volumes: For persistent logging, mount the local./logs directory to /var/www/logs in the container.

Passes environment variables.
NODE_ENV specifies the runtime environment (production).
APP_VERSION: Custom variable for defining the application version (1.0)
depends_on:
Ensures that the mongodb container starts before nodeapp.



 MongoDB Service (mongodb) container_name: Specifies a custom container name (mongodb).
Use the official mongo image to launch a MongoDB instance.

Connects the service to the same custom  network (nodeapp-network) as the nodes.

 Networks: 
NodeApp-Network: Driver: Bridge
Nodeapp-network creates a   bridge network for container communication.


Final Setup:
Run the application with:
docker-compose up –build
