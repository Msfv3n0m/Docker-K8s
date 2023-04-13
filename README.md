# Docker-K8s

This project is a hello world for Docker and K8s

## Docker
### Software Development
First we will pull a docker image. An **image** can be thought of as a file system that is not accessible. In order to make that file system accessible, you must run the image - running a docker image creates a **container**.   Most docker images are stored on [Docker Hub](https://hub.docker.com/search?q=), but many people create new docker images for their own purposes as well. Below is an example of a command to pull a docker image with C++ programming tools installed: </br> </br>
`docker pull eclipse/cpp_gcc` </br>

Now that we have a local instance of the image, we can change it programmatically using a dockerfile. Our dockerfile simply creates a folder titled john316 in the root directory. Many people will use this file to update/upgrade a docker images packages to ensure they are up to date. The resulting image is then saved on our local machine. To build the new docker image with the dockerfile, we will use the following command: </br> </br>
`docker build ./Docker/. -t "cpp:Dockerfile"` </br>

After that finishes is run, you can view the list of images you have by running the `docker images` command </br>

This next command will run an image such that we have an shell inside the container: </br> </br>

`docker run -it --rm <image id> /bin/bash` </br>

Now we have a cli inside our container! Now we can explore to see what's here. `ps auxf` will display the processes that are running inside the container. Kinda like Task Manager in Windows, but there's not many. This is because the container is not a full computer. It only runs the processes that it has to; in this way, it differs from a VM or a normal computer. Because images/containers are created to be small, they are lightweight, easily portable, and have a relatively small footprint. `sudo cat /etc/shadow` will show you the list of users inside the container. These users are not the same as the ones on your host machine.  </br>
In summary, containers have their own file systems, users, processes, and networks. All of these are exclusive to the container(s) on the host system, and do not exist on the host system. </br>
Now you can use the `exit` command to return to your host system </br> </br>

Now, a shell in a container is pretty cool, but how does this help with software development? Well, we can create a new one that maps a folder from our host machine to the docker container. This will let us share files between the two filesystems. The following command will run the docker container mapping the current directory to the `/projects` folder in the docker container: </br> </br>
`docker run -it --rm -v /workspaces/Docker-K8s/Docker/project:/projects <image id> /bin/bash` </br>

Now, any file we make in our container's projects folder will propegate to our hosts project folder and vise versa! This means we can use any tools in the docker container (such as the g++ compiler) and the results will show up on our host machine! In your docker container, create a small C++ program named `main.cpp`. Then, compile it with the following command: </br> </br>
`sudo g++ -o main.exe main.cpp`
</br>

### Organization
Docker can also be used to organize software based on functionality. For example, it would be nice to have all your website stuff not just in a folder, but in a container. Then, you might have your ftp server in another docker container. A third could contain your database and so on and so forth. To get an idea of how a docker container could be used for organization, run this command to setup a wordpress site: </br> </br>
`docker run -p 80:80 -d httpd` </br> </br>
Give it a couple seconds to start and you'll have a fully operational wordpress site! the -p option is used to allow inbound traffic from the host system's port 80 to the container's port 80. In this way, container's have a similar firewall to a regular computer. The default configuration for host-based firewalls is to allow outbound traffic and deny inbound traffic. Likewise, our container will allow outbound traffic and deny inbound traffic unless otherwise specified. The -p option is allowing inbound traffic to our docker container. You will see a notification at the bottom-right of your browser after executing the command above; click on it and it will take you to your website! </br>

`docker ps` will show your running container(s) along with their container IDs. You can kill the container with `docker kill <container id>` </br> </br>

Now, we can also declaratively assembly a network of containers using a docker-compose file. In this file, we build a new image using our dockerfile and deploy 2 docker containers based on that template. To do this, first `cd Docker` into the docker folder and run `docker compose up -d` to execute the docker-compose file. Then, you can run `docker ps` to see both of the docker containers running on your host machine. After that, you can `cd ..` to return to the parent directory. But again, the problem with this is that docker compose will not guarentee that the container is running in a healthy state. </br> Kubernetes: enter stage right

## Kubernetes
Minikube is k8s running as both master and worker node on a single machine. So, for the purposes of this demonstration we must start minikube with `minikube start` </br> </br>

We already have a configuration file to create a kubernetes pod in the K8s folder. The pod contains a single container that runs the httpd image; this mirrors the manual configuration we had in docker. </br> </br>
`kubectl apply -f K8s/httpd-pod.yml` </br>

`kubectl get pods` will show you the pod we just created. </br></br>

The pod is created, but we still cant access the website. Why? Because the website is only available on the inside of the kubernetes cluster. There are a two easy ways we can access it: </br>
1. VPN into the k8s cluster </br>
2. Forward a port from the host to the httpd-pod pod  </br>

The easiest solution in this case is just to forward a port so that it can be accessed from outside of our K8s cluster </br>
`kubectl port-forward httpd-pod 8080:80` </br> </br>

To see the service running on the pod, click on the ports tab above your terminal and add port 80 to the list. Now, if you click the globe icon that says "open in browser" you will see that the website is up! Now, kubernetes will use its built-in autoscaling mechanism to scale up or down depending on the demand of your web browser. </br> </br>