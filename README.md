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
