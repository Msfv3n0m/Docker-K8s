# Docker-K8s

This project is a hello world for Docker and K8s

## Docker

First we will pull a docker image. An **image** can be thought of as a file system that is not accessible. In order to make that file system accessible, you must run the image - running a docker image creates a **container**.   Most docker images are stored on [Docker Hub](https://hub.docker.com/search?q=), but many people create new docker images for their own purposes as well. Below is an example of a command to pull a docker image with C++ programming tools installed: </br> </br>
`docker pull eclipse/cpp_gcc` </br>

Now that we have a local instance of the image, we can change it programmatically using a dockerfile. Our dockerfile simply creates a folder titled john316 in the root directory. Many people will use this file to update/upgrade a docker images packages to ensure they are up to date. The resulting image is then saved on our local machine. To build the new docker image with the dockerfile, we will use the following command: </br> </br>
`docker build . -t "cpp:Dockerfile"` </br>

After that finishes is run, you can view the list of images you have by running the `docker images` command </br>

This next command will run an image such that we have an interactable container: </br>

`docker run -it --rm <image id> /bin/bash` </br>

This will give you a shell inside the container! Pretty cool, but how does this help with software development? Well, if we `exit` from this container, we can create a new one that maps a folder from our host machine to the docker container. This will let us share files between the two filesystems. The following command will run the docker container mapping the current directory to the `/projects` folder in the docker container </br> </br>
`docker run -it --rm -v /workspaces/Docker-K8s/Docker/project:/projects <image id> /bin/bash` </br>


## Kubernetes
