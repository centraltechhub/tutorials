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

<img width="1000" alt="image" src="https://user-images.githubusercontent.com/93929892/184942280-a9f7f562-8a0b-48ed-bd4a-d4af1f663154.png">

**Creating a CentOS based Docker container**

1. Create a directory called LabSession1 </br>

2. If vi editor is not available, install using the following command:</br>
`yum -y install vim`</br>

3. create a file called Dockerfile using vi command under LabSession1 directory. Command to launch vi editor:</br>
`vi Dockerfile`</br>
Type i to enter into edit mode.</br>
Enter the following:</br>
`FROM centos` </br>
Save the file by entering Esc and then type  :wq </br>
Now the file Dockerfile contain one line - `FROM centos`. </br>

4. Execute the following command to build an image.</br>
`docker build .` </br>
If successful, you will see the following output in the terminal.</br>
`Successfully built a2a15febcdf3` </br>

5. Execute the following command to check the image availability in the local repository. </br>
`docker images` </br>

6. Execute the following command to run the images into a container: </br>
`docker run -idt <image-id>` </br>
If the container is up and running, you will see a similar status showing “Up” for the image.

<img width="524" alt="image" src="https://user-images.githubusercontent.com/93929892/185033791-09075bdf-2c6d-4aac-aff8-2c7229cc2255.png">


**Installing Java in the container**

1. Login into the docker container using the following command:</br>
`docker exec -it <container-id> bash`</br>
If successful, you will be directed into the container console. </br>

2. Type pwd to check the present working directory. Output should be root – “/”.</br>

3. Install jdk on the local container.</br>
First command to execute:</br>
`yum install -y yum-utils`</br>
Followed by:</br>
`yum install java-1.8.0-openjdk`
run `java -version`. If installed successfully, you will see:
<img width="694" alt="image" src="https://user-images.githubusercontent.com/93929892/185534463-7305def8-a941-4b51-8197-8d8b0d29ade5.png">

4. Install unzip tool using the following command:<br/>
`yum install zip`

5. Exit from the container by entering exit command.<br/>

6. Save the docker container by executing the following command:<br/>
`docker commit <container-id>`


