# Docker-K8s

This project is a hello world for Docker and K8s

## Docker
### Software Development
First we will pull a docker image. An **image** can be thought of as a file system that is not accessible. In order to make that file system accessible, you must run the image - running a docker image creates a **container**.   Most docker images are stored on [Docker Hub](https://hub.docker.com/search?q=), but many people create new docker images for their own purposes as well. Below is an example of a command to pull a docker image with C++ programming tools installed: </br> </br>
`docker pull eclipse/cpp_gcc` </br>

Now that we have a local instance of the image, we can change it programmatically using a dockerfile. Our dockerfile simply creates a folder titled john316 in the root directory. Many people will use this file to update/upgrade a docker images packages to ensure they are up to date. The resulting image is then saved on our local machine. To build the new docker image with the dockerfile, we will use the following command: </br> </br>
`docker build . -t "cpp:Dockerfile"` </br>

After that finishes is run, you can view the list of images you have by running the `docker images` command </br>

This next command will run an image such that we have an interactable container: </br> </br>

`docker run -it --rm <image id> /bin/bash` </br>

This will give you a shell inside the container! Pretty cool, but how does this help with software development? Well, if we `exit` from this container, we can create a new one that maps a folder from our host machine to the docker container. This will let us share files between the two filesystems. The following command will run the docker container mapping the current directory to the `/projects` folder in the docker container: </br> </br>
`docker run -it --rm -v /workspaces/Docker-K8s/Docker/project:/projects <image id> /bin/bash` </br>

Now, any file we make in our container's projects folder will propegate to our hosts project folder and vise versa! This means we can use any tools in the docker container (such as the g++ compiler) and the results will show up on our host machine! In your docker container, create a small C++ program named `main.cpp`. Then, compile it with the following command: </br> </br>
`sudo g++ -o main.exe main.cpp`
</br>

To list your docker containers use the following command: </br> </br>
`docker ps` </br>

To kill your docker containers use the following command: </br> </br>
`docker kill <container id>` </br>
### Organization
Docker can also be used to organize software based on functionality. For example, it would be nice to have all your website stuff not just in a folder, but in a container. Then, you might have your ftp server in another docker container. A third could contain your database and so on and so forth. To get an idea of how a docker container could be used for organization, run this command to setup a wordpress site: </br> </br>
`docker run -p 80:80 -d wordpress` </br> </br>
Give it a couple seconds to start and you'll have a fully operational wordpress site!
## Kubernetes
