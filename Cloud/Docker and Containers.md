## Docker and Container

**Preparing the system**

1. Uninstall docker if it already exists in the system.<br/>
`sudo yum remove docker-ce docker-ce-cli containerd.io`<br/>
This is not a mandatory step. User can run the above command in case it is required to remove the existing docker installation.<br/>
2. Install yum utils.<br/>
`sudo yum install -y yum-utils`<br/>

**Installation of Docker**

Steps:<br/>
1. Login to the operating system and execute the following command to add the docker repository:<br/>
`sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo`<br/>

2. Install docker
`sudo yum install docker-ce docker-ce-cli containerd.io`<br/>

The installation will prompt for user input at a couple of places before the installation completes. Once the above command is executed, check if the installation process completes successfully.<br/>

3. Execute the following commands to start docker service:<br/>
`sudo systemctl start docker`<br/>

4. Test the installation by executing the following commands:<br/>
To check for the list of images available in the local repository:<br/>
`docker images`<br/>
To check if there are any active container:<br/>
`docker ps`<br/>
To check if there are any exited containers:<br/>
`docker ps -a`<br/>
For now both the above commands would return no values.<br/>

