# Docker Quick-start Guide
To setup an instance to use docker, lets start with an Ubuntu server 


`sudo apt-get update`

`sudo apt-get -y install 
  apt-transport-https 
  ca-certificates 
  curl 
  gnupg-agent 
  software-properties-common`
  
  
  `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`


`sudo add-apt-repository 
  "deb [arch=amd64] https://download.docker.com/linux/ubuntu 
  $(lsb_release -cs) 
  stable"`
  
  
  ```sudo apt-get update```
  
  `sudo apt-get install -y docker-ce=5:18.09.5~3-0~ubuntu-bionic docker-ce-cli=5:18.09.5~3-0~ubuntu-bionic containerd.io`
  
  `sudo docker version`
  
  ## Give your own user access to run docker commands by adding it to docker group
  

`sudo usermod -a -G docker ubuntu`

Log out of the server, then log back in.

Once you are logged back on, you can verify cloud-user's access:

`docker version`

# Set the log driver to syslog
Edit daemon.json:

`sudo vi /etc/docker/daemon.json`
Add configuration to daemon.json to set the default logging driver.

`{
  "log-driver": "syslog"
}`

Restart docker.

`sudo systemctl restart docker`

Verify that the logging driver was set properly like so:

`docker info | grep Logging`

This command should return a line that says:

`Logging Driver: syslog`


# Configure the swarm manager.
On the swarm manager server, initialize the swarm. Be sure to replace <swarm manager private IP> in this command with the actual Private IP of the swarm manager (NOT the public IP).

`docker swarm init --advertise-addr <swarm manager private IP>;`


# Join the worker nodes to the cluster.
On the swarm manager, get a join command with a token:

`docker swarm join-token worker`
This should provide a command that begins docker swarm join .... Copy that command and run it on both worker servers.

Go back to the swarm manager and list the nodes.

`docker node ls`
Verify that you can see all three servers listed (including the manager). All three should have a status of READY. Once all three servers are ready, you have built your own Docker swarm cluster!



# Managing Images 

Download an image from a remote registry 
`docker image pull nginx:1.14.0`

To list your images 
`docker image ls`

# To get more details about each image 
`docker inspect nginx`

# When you delete an image, that is associated with a container you are left with a dangling 

`docker image prune`

# Docker Registries 

A docker registry is responsible for storing and distributing docker images. The default public registry for images is `Docker Hub`. But you can also create your own public/private registries and pull images from there. 


# Docker Services
Create a simple service running the nginx image.

`docker service create nginx`

Create an nginx service with a specified name, multiple replicas, and a published port.

`docker service create --name nginx --replicas 3 -p 8080:80 nginx`

Use a template to pass the node hostname to each container as an environment variable.

`docker service create --name node-hostname --replicas 3 --env NODE_HOSTNAME="{{.Node.Hostname}}" nginx`

Get the container running on the current machine, and print its environment variables to verify that the NODE_HOSTNAME variable is set properly.

```docker ps
docker exec <CONTAINER_ID> printenv
``` 
List the services in the cluster.

`docker service ls`

List the tasks for a service.

`docker service ps nginx`

Inspect a service.

`docker service inspect nginx`
`docker service inspect --pretty nginx`

Change a service.

`docker service update --replicas 2 nginx`

Delete a service.

`docker service rm nginx`

Create a global service.

`docker service create --name nginx --mode global nginx`
Two different ways to scale a service:

`docker service update --replicas 3 nginx`
OR
`docker service scale nginx=4`

# Docker Compose
Install Docker Compose.

`sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`

`sudo chmod +x /usr/local/bin/docker-compose`

`docker-compose version`

Set up a Docker Compose project.

```mkdir nginx-compose
cd nginx-compose
vi docker-compose.yml
```

Create your docker-compose.yml.

```
version: '3'
services:
  web:
    image: nginx
    ports:
    - "8080:80"
  redis:
    image: redis:alpine
  ```
  
Start your Compose app.

`docker-compose up -d`
List the Docker Compose containers, then stop the app.

`docker-compose ps`

`docker-compose down`
