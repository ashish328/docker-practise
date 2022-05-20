# Docker Notes

### Docker Images

Docker images are a layer of another docker images.

1. Building docker image
    ```sh
    docker build -t myApp:v1 .
    ```
    * -t —> tag.
    * myApp —> imageName.
    * v1 —> version (optional).
    * . —> location to the Docker file. ( If the command is run in the folder where docker file exists.
  
2. listing docker images
    ```sh
    docker images
    ```
  
3. Deleting docker images
    ```sh
    docker image rm myApp:v1
    ```
    * myApp —> image name. (can specify the Image Id also).
    * v1 —> version. (optional if not give while building the images & should be given if you have multiple images with same name but different versions).
    ```sh
    docker image rm myApp:v1 -f
    ```
    * -f —> force. (if the image is used by any container it gives you warning saying image is in use need to delete all the containers using that particular image or remove it forcefully).


### Docker Containers

1. Create & Run docker container
    ```sh
    docker run --name myapp_c -p 4000:4000 myApp:v1
    ```
    * --name —> name tag to specify the name for container.
    * myApp_c —> name of the container. 
    * -p —> port mapping. (export the port of the container to outer system).
    * 4000:4000 —> port of the container you wanna expose. :  port you wanna map to. (port to which app is listening to).
    * myApp:v1 —> image name:version.

2. List docker containers
    ```sh
    docker ps
    ```
    * ps —> processes, will list out all the running docker containers 
	
    ```sh
    docker ps -a
    ```
    * -a -> all, will list out all the docker containers which are running & idle.

3. Delete docker container
    ```sh
    docker container rm myapp_c1 myapp_c2
    ```
    * myapp_c1 —> container name. (name1 name2 name3) can be used to delete multiple containers at a time.

4. Start docker container (only user id container is available)
    ```sh
    docker start myapp_c1
    ```
    * myapp_c1 —> container name. (Port mapping is not needed as its done while creating the container) 
  
5. Stop docker process
    ```sh
    docker stop myapp_c1
    ```
    * myapp_c1 —> container name.


### Docker file
    FROM node:17-alpine
    WORKDIR /app
    COPY scr_folder dist_folder
    RUN npm install
    EXPOSE 4000
    CMD [“node”, “app.js”] || CMD ["node app.js"]

* `FROM node:17-alpine` —> Parent image. Node:17-alpine can get these parent images from docker hub.
* `WORKDIR /app` —> sets up the working directory for the image to run all the commands.
* `COPY scr_folder dist_folder` —> copies files to the image. 
    * scr_folder —> path to files to be copied
    * dist_folder —> destination path to where to copy the files.
* `RUN npm install` —> RUN is use to run commands while building the app. The working directory to run the commands on the root directory of the image. If the we want to shift the command to run on any other directory WORKDIR id recommended to use to change working directory of the command. 
* `EXPOSE 4000` —> will expose the port where the app is running.
* `CMD [“node”, “app.js”] || CMD ["node app.js"]` —> is used to run the command while runtime of the project. It takes the commands in form of array of strings.


### Docker compose

Docker-compose.yaml file take the data in the form of an JSON but  which is a bit modified. Indentation for the objects & — for the arrays.

    version: '2.2'
      services:
        api:
          build: ./api
          container_name: myapp_c
          ports:
            - "4000:4000"
          volumes:
            - ./api:/app
            - /app/node_modules
  

* Version —> version of docker-compose (docker-compose version)
* services —>  has multiple nester values how to build the docker images and how to run the docker containers. Services have sub values  equal to number of projects
* api —> project name (can take any name but the folder names are preffered for letter understanding to us)
* build —> path to the docker file of the project.
* container_name: —> container name
* ports —> port mapping to the containers. Takes multiple values start with -
* volumes —> volumes of the container. Takes multiple values start with -

1. #### run compose

  this will create and image and start the container.
    
    docker-compose up
   
2. #### Stop docker-compose

  this will stop the container and delete the container
    
    docker-compose down
    
  this will settle the images and volumes and delete the containers
     
     docker-compose down —rmi all -v
    

