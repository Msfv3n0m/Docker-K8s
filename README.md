# Docker-K8s

This project is a hello world for Docker and K8s

## Docker

First we will pull a docker image. An **image** can be thought of as a file system that is not accessible. In order to make that file system accessible, you must run the image - running a docker image creates a **container**.   Most docker images are stored on [Docker Hub](https://hub.docker.com/search?q=), but many people create new docker images for their own purposes as well. Below is an example of a command to pull a docker image with C++ programming tools installed: </br> </br>
`docker pull eclipse/cpp_gcc` </br>

Now that we have a local instance of the image, we can change it programmatically using a dockerfile. 

## Kubernetes
