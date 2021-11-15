# dockernizer
DOCKER Basic Command:

IMAGE:
Code	Sample	Desc
Docker build -t <image-name> .	 Docker run -t testing-image .	To build docker image before running container
Docker image ls	- 	To see image information
Docker image rm <image-id> -f	Docker image rm 128921 -f	To delete docker image, -f force mode


CONTAINER
- Container Running
Code	Sample	Desc
Docker run -p <port machine>:<port docker> -d —name <container-name> <docker-image-name>	Docker run -p 8080:3000 -d —name node-app testing-image	Run docker container, -p for expose port to external, -d to detached mode (running in background)
Docker ps	-	To see container running information
Docker rm <container-id/container-name> -f	Docker rm testing-node -f	To delete docker running container and delete in force
Docker ps -a	-	To see all container information
Docker logs <docker-name>	Docker logs testing-node	To see what happen in inside runtime docker container
		


- Docker Volume
Code	Sample	Desc
Docker volume ls	-	To see what volumes you had
Docker volume prune	-	To delete all volume docker
Docker rm <container-name> -fv	Docker rm testing-node -fv	Delete container and it’s related volume

- How to check file inside Container
Code	Sample	Desc
docker exec -it <container-name> bash	docker exec -it testing-container bash	just type “exit” if you want close 

Bind Mode:

- How to bind your code into changes into docker if you don’t want rebuild image
    - If you want rebuild in real-time without restart the docker container, just install nodemon for only dev dependency
Code	Sample	Desc
(With node_modules): Docker run —v <pathFolderLocation>:<pathFolderContainer> -p <port machine>:<port docker> -d —name <container-name> <docker-image-name>	Docker run -v $(pwd):/app -p 8080:3000 -d —-name node-app testing-image	Mac: $(pwd) to get reference folder
Real time in dev mode with node_modules copy from local	Script: { “dev”: “nodemon index.js”}	Only for dev mode, but make sure your node_modules installed in local not container. Because it will causing bug
Real time in dev mode without node_modules copy from local	Script: { “dev”: “nodemon index.js”}	Only for dev mode and need create new docker volume to handle node_modules not override from local file
(Without node_modules): Docker run —v <pathFolderLocation>:<pathFolderContainer> -v <pathFolderContainer>/node_modules -p <port machine>:<port docker> -d —name <container-name>	Docker run -v $(pwd):/app -v /app/node_modules -p 8080:3000 -d —-name node-app testing-image	

DOCKER COMPOSE
- To running multiple docker container
- Because docker compose is dummy, so when we have a new build image need run —build to make sure our docker compose using latest docker-image
- Always check command “docker compose —help” or “docker compose COMMAND —help”

Structure File	Sample	Desc
Version: <compose version>	Version: “3”	What version docker compose what will you use
service: <project-name>	Service: testing-node	To naming project what you wanted
Build: <path file dockerFile>	Build: .	Docker compose will try build based on your dockerFile location path
Ports: - “<portingToOut:docker expose port>”	Port: - “8080:3000”	
Volumes: <fileLocated:container file>	Volumes: ./:/app	Different explain path from local project in docker-compose & docker cmd
Environment: - <set .env here>	Environtment: - PORT=3000	Set environment config in here like, PORT, NODE_ENV etc
Env-file: <file path of your .env>	Env-file: ./.env	This is to inject your .env file if you not using “environment” setup in your docker compose


How To Differentiate Your Docker Compose Between Dev and Prod

	- in your .dockerignore add “docker-compose*” to prevent your docker-compose import in your container
Create file	Desc	How to Run
Docker-compose.dev.yml	Setup your specific dev config in here, especially when you using volume to get realtime changes	Docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d
Docker-compose.prod.yml	Setup your specific prod config in here	Docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d —build
Docker-compose.yml	Setup general config in here	-












