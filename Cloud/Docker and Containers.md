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

**Installing Liberty server in the container**

1. Download latest WebSphere liberty server from the following location:<br/>
https://developer.ibm.com/wasdev/downloads/#asset/runtimes-wlp-webProfile8 <br/>

2. Copy the liberty zip, wlp-webProfile8-19.0.0.9.zip into the base centos system.<br/>

3. Alternately, the liberty zip can be downloaded directly into the base system using the following curl command: <br/>
`curl https://public.dhe.ibm.com/ibmdl/export/pub/software/websphere/wasdev/downloads/wlp/20.0.0.7/wlp-webProfile8-20.0.0.7.zip --output wlp-webProfile8-20.0.0.7.zip`

4. Copy the zip from the base OS to the container OS. Execute the following command:<br/>
`docker cp ./wlp-webProfile8-19.0.0.9.zip 336c881a0ec6:/wlp.zip`

5. Enter into the container by once again executing:<br/>
`docker exec -it <container-id> bash` <br/>
Execute `ls` command. You will see wlp.tar file present.

6. unzip the tar using the following command:<br/>
`unzip wlp.zip`<br/>
You will see wlp folder present.

7. Navigate to wlp/bin directory using cd command. `cd wlp/bin`

8. Execute the following command to create a server instance in liberty.<br/>
`./server create AppServer`

9. navigate to /wlp/usr/servers/AppServer.

10. Edit the server.xml using the vi editor.<br/>
Add host attribute to httpEndpoint element. Example:<br/>
`<httpEndpoint id="defaultHttpEndpoint" host=”*”`<br/>
Save and exit vi editor.

11. Exit the container by typing `exit` and save the container by committing.<br/>
`docker commit <container-id>`

12. Download sample application from the following location:<br/>
https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/<br/>
`curl https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/sample.war --output sample.war`

13. Copy the sample.war from the base ubuntu to the container OS’s liberty server’s dropin directory.<br/>
`docker cp ./sample.war 336c881a0ec6:/wlp/usr/servers/AppServer/dropins`

14. Login to the container again. 

15. Navigate to /wlp/usr/servers/AppServer/dropins
Execute `ls` command, you should see sample.war present. 

16. Navigate to /wlp/bin and execute the following command to start the server:<br/>
`./server start AppServer`<br/>
When successfully started you should see the following message in the console:<br/>
Server AppServer started with process ID 5267.

17. Exit and saved the container.

18. Open the browser and hit the following URL:
http:// 9.199.144.167:9080/sample/hello.jsp

Following output will be seen:

<img width="300" alt="image" src="https://user-images.githubusercontent.com/93929892/185535311-1a243716-ced6-44d7-ac43-ffe1f5eeb4d6.png">
