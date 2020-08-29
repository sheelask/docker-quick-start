# Docker Quick-start Guide
To setup an instance to use docker, lets start with an Ubuntu server 


`sudo apt-get update`

`sudo apt-get -y install \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg-agent \
  software-properties-common`
  
  
  `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`


`sudo add-apt-repository \
  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) \
  stable"`
  
  
  `sudo apt-get update`
  
  `sudo apt-get install -y docker-ce=5:18.09.5~3-0~ubuntu-bionic docker-ce-cli=5:18.09.5~3-0~ubuntu-bionic containerd.io`
  
  `sudo docker version`
  
  ## Give your own user access to run docker commands by adding it to docker group
  

`sudo usermod -a -G docker ubuntu`

Log out of the server, then log back in.

Once you are logged back on, you can verify cloud-user's access:

`docker version`
