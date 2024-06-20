### Story of an organization and Problem introduction

* _**Organization name**_ : Contoso
* _**Application**_ : ecommerce
* _**Architecture**_ :

![alt text](shots/1.PNG)

* _**Problems**_ :
    
 1. Different front-ends ( ios, Andriod )
    + Accesibility
    + Cross platform

  ![alt text](shots/2.PNG)

 2. Deployment of new features : typically are having more downtimes
    + Zero down time deployments
    + Faster deployments
 3. Contoso wants to run this application on any cloud or on-premises
    + Portability
 4. De Composing application into smaller services according to the business needs

 ![alt text](shots/3.PNG)

 5. Updating any microservice can be done independently
 6. Re using microservices is much easier
    + Scaling for microservice is much easier and cheaper compared to Scaling monolith

* _**Solution**_ :

 1. Container Technology ( _**Docker**_ )
 2. Container Orchestration ( _**Kubernetes**_ )

#### Application Deployments - Terms

 1. _**Capex (Capital Expenditure)**_ : Investment made by organisation for purchasing of servers 
 2. _**Opex (Operational Expenditure)**_ : Investment made by organisation to maintain servers
 3. _**Physical Server**_ : Racks have servers where we can directly install an application
 4. _**Hypervisor**_ : Software used to create virtual machines. They are of two types : `ONE`, which can be installed directly on servers and create virtual machines. `TWO`, where we install over the OS and then create virtual machines.
 5. _**Virtual Machine**_ : Isolated space to virtually create hardware and run applications individually
 6. _**Return on Investment (ROI)**_ : How much returns or results achieved in order to create hardware and running applications efficiently

### Evolution

1. _**Generation 1**_ : 

    * Directly run on physical servers
    * If your application is not utilizing hardware completely, ROI is very long
    
    ![alt text](shots/4.PNG)

2. _**Generation 2**_ : 
    
    * Hypervisors create virtual machines and applications installed in virtual machines
    * Hypervisors perform hardware virtualization and provide
      + virtual cpu
      + virtual ram
      + virtual disk
      + virtual network
    * In the isolated area created by hypervisor, we can install OS and necessary softwares
    * Application can be installed and used from here and also have better ROI

    ![alt text](shots/5.PNG)

3. _**Generation 3**_ - _**Containers**_ : 

    * These are isolated areas which look like vms but the container is an isolated area which has virtualized OS
    * Applications running in Containers will not feel the difference
    * We can run more applications on a single box

    ![alt text](shots/6.PNG)

### What is Docker ?

* _**Docker (Dock worker)**_ is used to create containers which is standard way of packaging any application
* Application can be any of the below but have a standard way of packaging i.e _**Docker image**_.
    + developed in any technology
    + developed on any server
* Packaging in docker image format helps us to run our application everywhere possible

![alt text](shots/7.PNG)

#### Expectations in terms of docker from you

* Containerize any application run by your organization
* Manage Data, Security and Networks for containerized applications
* To acheive the above, we need to use :
    + Docker and understand how it runs and creates containers
    + play with docker aroud networks, data and security
    + apply them to our application

#### Run apache server and nginx (VM)

* Create a VM (Ubuntu) and ssh into it
```
sudo apt update
sudo apt install apache2 -y
```
* Now navigate to `http://<public-ipaddress>`

![alt text](shots/8.PNG)

* Now let's try to also install nginx and navigate to `http://<public-ipaddress>`
```
sudo apt update
sudo apt install nginx -y
```
![alt text](shots/9.PNG)

### Docker Installation

* When we install docker we get two major components :

 1.  _**Docker client**_ : A command line to interact with docker engine

![alt text](shots/10.PNG)

 2. _**Docker engine**_ : This is collection of multiple components
        * Orchestration
        * Docker daemon
        * Runtime
* to play with docker commands
    * manual --help
    * cheatsheet

#### Installations

 1. _**Linux VM**_:

    * Create a VM with standard configuration
    ```
    sudo cat /etc/group
    sudo cat /etc/passwd
    ```
* Docker can be installed in many ways :  

 +  Using following instructions
        
    [ Refer Here : https://docs.docker.com/engine/install/ ]

 + Script based installation 
        
    [ Refer Here : https://get.docker.com/ ]

```
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
docker info
```
![alt text](shots/11.PNG)

* Docker allows communication to the unix socket for the users who belong to docker group `sudo cat /etc/group`
* So let's add current user `ubuntu (whoami)` to docker group `sudo usermod -aG <groupname> <username>` _**sudo usermod -aG docker ubuntu**_
* Then exit and relogin `docker info`

![alt text](shots/12.PNG)

* Now execute `docker container run hello-world`

![alt text](shots/13.PNG)

2. _**Windows 10/11 (Non Home editions)**_


    [ Refer Here : https://docs.docker.com/desktop/install/windows-install/ ]

* _**Note**_ : Installing docker on windows is not recommended

3. _**Mac**_

    [ Refer here : https://docs.docker.com/desktop/install/mac-install/ ]

#### Docker Playground

* Create a Docker Hub account 

    [ Refer Here : https://hub.docker.com/ ]

* This playground gives a linux machine with docker installed ( 4 hours for free ) 

    [ Refer Here : https://labs.play-with-docker.com/ ]

#### Terms to be aware of

* containerd
* runc
* libcontainer
* oci
* docker shim
* appc
* grpc
* rkt containers

 * Applications bring revenue to enterprises and to run applications we need servers, OS, etc

#### Let's create a linux server and install tomcat in it
* Create a linux server ( AWS / Azure / GCP ) 

![alt text](shots/14.PNG)

* We have experimented in the _**Linux VM ( Ubuntu )**_
    * network interface gives network connectivity
    * cpu, ram and disk are available
    * to install softwares we have used package manager `apt`
* Commands
```
htop
sudo apt update
sudo apt install net-tools -y
if config
sudo apt install openjdk-11-jdk -y
java -version
sudo apt install tomcat9 -y
sudo systemctl status tomcat9
```
* _**NOTE**_ :

+ to check network => `ifconfig`
+ to check java => `java -version`
+ to check tomcat => `sudo systemctl status tomcat9`

* We were able to exactly perform similar operations inside the container as well

#### Let me take a application

* This is spring pet clinic application
* Let's try to run this application on linux ( ubuntu )

    [ Refer Here : https://github.com/spring-projects/spring-petclinic ]

* To build the code
```
sudo apt update
sudo apt install openjdk-17-jdk maven -y
git clone https://github.com/spring-projects/spring-petclinic.git
cd spring-petclinic/
# java package
mvn package
java -jar target/spring-petclinic-3.0.0-SNAPSHOT.jar
```
![alt text](shots/15.PNG)

* Docker way of working
  * We create a _**Docker image (docker packaging format)**_
    * We need to create _**Dockerfile**_
    * Push the image to _**registry (docker hub, public registry)**_
    * Create the _**container**_ using the image anywhere

* Install docker on the VM using script and add to docker group
```
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
sudo usermod -aG docker ubuntu
exit
relogin
cd spring-petclinic/
```
* _**Dockerfile**_ for spring pet clinic
```
FROM amazoncorretto:17-alpine-jdk
LABEL author=Harikag
ADD target/spring-petclinic-3.0.0-SNAPSHOT.jar /springpetclinic.jar
EXPOSE 8080
CMD ["java", "-jar", "/springpetclinic.jar"]
```
* Add the Dockerfile and create docker image
```
vi Dockerfile
docker image build -t spc:1.0 .
```
* to create container at the original port `-P`
```
docker container run -d -P spc:1.0
```
* to create container at the different ports `-p` ( remember to allow open the required ports )
```
docker container run -d -p 8081:8080 spc:1.0
docker container run -d -p 8082:8080 spc:1.0
docker container run -d -p 8083:8080 spc:1.0
```
* expose over the browser with the desired port

#### How Isolations are created ? ( OR ) How Containers Work ?

* Each container is getting a
    * new process tree
    * disk mounts
    * network (nic)
    * cpu/memory
    * users
* Docker Internals

    [ Refre here : https://directdevops.blog/2019/01/31/docker-internals/ ]



### Docker Architecture

_**Generation 1:**_

* This was first gen, Where docker daemon used lxc (a linux kernel feature) to create containers



_**Generation2:**_

* Since docker was relying on lxc which was kernel feature, updates to kernel frequently used to break containers created by docker
* So docker has created its own component called libcontainer (libc) to create containers
* Docker wanted containers to be multi os and lxc was definetly not the way forward



* Adoption of docker was drastically increased as it was stable

_**Generation 3:**_

* In this generation, docker engine was revamped from monolith to multi component architecture and the images and containers were according to OCI (open container initiative) image spec and runtime spec
* In the latest architecture
* docker daemon exposes api’s to listen requests from docker client
* Passes the requests to containerd. This manages the lifecylcle of container
* containerd forks a runc process which creates container. once the container is created the parent of the container will be docker shim



#### Creating our first docker container

 _**docker container creation:**_

* To create container we need some image in this case lets take `hello-world`
* The command `docker container run hello-world` executed
* What happens
    * docker client will forward the request to docker daemon
    * docker daemon will check if the image exists locally. if yes creates the container by using image
    * if the image doesnot exist, then docker daemon tries to download the image from docker registry connected. The default docker registry is docker hub
    * Downloading image into local repo from registy is called as pull
    * Once the image is pulled the container is created



* Registry is collection of docker images hosted for reuse
* Docker hub 

    [ Refer Here : https://hub.docker.com/search?q= ]

#### Playing with containers

* Create a new linux vm and install docker in it





* Open all ports
    * AWS

 



#### Check docker images in the host







#### Pull the images from docker hub

* image naming convention
```
[username]/[repository]:[<tag>]
shaikkhajaibrahim/myspc:1.0.1
username => shaikkhajaibrahi
repository => what image => myspc
tag => version => 1.0.1
```

* default tag is latest 
```
nginx
nginx:latest
```

* Official images don't have username
```
nginx
ubuntu
alpine
shaikkhajaibrahim/myspc
```

* Lets pull the image nginx with tag 1.23
```
docker image pull nginx:1.23
docker image ls
```


* Let's pull the jenkins image with latest version



* Let's find the alpine and pull the image



#### Remove images from local

* Every image will have unique image id and image name
* We can delete individually `docker image rm alpine:3.17`
* if I have to delete all the images `docker image rm $(docker image ls -q)`





#### Create a container with nginx

* To create and start the container we use run command



* _**note:**_ i will be using -d for some time and we will discuss importance of this in next session
* every container gets an id and a name. name can be passed while creating container, if not docker will give random name



* Remove all the running containers `docker container rm -f $(docker container ls -q )`





* Remove specific container



* Remove all containers `docker container rm -f $(docker container ls -a -q )`



* _**Exercise:**_ Start and stop containers


### Docker container lifecycle

* Docker lifecycle states :
 1. Created
 2. Running
 3. Paused
 4. Stopped
 5. Deleted



#### Accessing the applications inside docker containers

* From now the machine where we have installed docker will referred as `host` and the docker container will be referred as `container`
* We have access to host network & as of now containers are created in private container network, so to access applications inside containers we use `port-forwording`



* _**Command**_ : `docker container run -d -p <host-port>:<container-port> <image>`
* Create a _**nginx container**_ and expose on _**port 30000**_ we use , `docker container run -d -p 30000:80 --name nginx1 nginx`



* Create a _**jenkins container**_ & _**expose 8080 port on 30001**_ of host we use , `docker container run -d -p 30001:8080 --name jenkins1 jenkins/jenkins`



* To assing any random free port on host to container port `docker container run -d -P image`
* Let's create three nginx containers




#### Exercise

1. Install docker on a linux vm
2. Run 1 httpd containers (apache container) which runs on 80 port
3. try accessing any application
4. stop the containers
5. try accessing
6. start the continers and access this should work
7. pause the containers, access the application
8. unpause the containers, access the application
9. delete the container

#### Containerizing spring petclinic

* I have spring petclinic version 2.4.2 which requires java 11 and runs on port 8080
* To start the application `java -jar spring-petclinic-2.4.2.jar`
* What is required:
    + jdk 11
    + jar file
* How to access the application
    + http over port 8080
* Let's start the amazoncorretoo based container with port 8080 exposed 

    [ Refer Here : https://hub.docker.com/_/amazoncorretto ]

```
docker container run -it -p 30000:8080 amazoncorretto:11 /bin/bash
```


* now lets download the spring petclinic 

[ Refer Here : ]



*  Run the application `java -jar spring-petclinic-2.4.2.jar`



*  Now to create a image from a running container, lets login into linux vm, so let's use `docker container commit`



* remove all the containers and run the myspc image based container
```
docker container run -d -p 30001:8080 --name spc1 myspc:latest java -jar spring-petclinic-2.4.2.jar
```
* This is not a useful approach as we are creating images manually
* Docker has a better way i.e. _**Dockerfile**_

### Dockerfile based Image building

* Workflow



* Dockerfile is a text file with instructions Refer Here
* The basic syntax `INSTRUCTION arguments`
* In Docker we have concept of base image i.e. to run your application using some existing image
* We can use a base image called as scratch which has nothing in it
* In majority of the cases we take what is required to run our application as base image

#### Basic instructions to write a Dockerfile

* _**FROM**_ : use tag all the time (donot use latest)

    [ Refer Here : https://docs.docker.com/reference/dockerfile/#from ]

* _**RUN**_ : The commands to be executed while building the image to install/configure your appliation 

    [ Refer Here : https://docs.docker.com/reference/dockerfile/#run ]

* _**CMD**_ : This command will be executed while starting the container

    [ Refer Here : https://docs.docker.com/reference/dockerfile/#cmd ]

* _**EXPOSE**_ : This adds ports to be exposed while starting the container 

    [ Refer Here : https://docs.docker.com/reference/dockerfile/#expose ]

#### Spring petclinic Dockerfile

* Let's do two ways
 1. use any image with java11 already as base image `amazoncorretto:11`
 2. use any image with slim os as base image `alpine:3`

* Dockerfile- based on amazoncorreto:11 
```
FROM amazoncorretto:11
RUN curl https://referenceapplicationskhaja.s3.us-west-2.amazonaws.com/spring-petclinic-2.4.2.jar -o spring-petclinic-2.4.2.jar
EXPOSE 8080
CMD ["java", "-jar", "spring-petclinic-2.4.2.jar"]
```
* Let
s build the image based on amazoncorreto




* Now let's create a container `docker container run -d -P --name spc1 myspc:corretto11`



* Approach 2: Start from some os
```
FROM alpine:3
RUN apk add openjdk11
RUN wget https://referenceapplicationskhaja.s3.us-west-2.amazonaws.com/spring-petclinic-2.4.2.jar
EXPOSE 8080
CMD ["java", "-jar", "spring-petclinic-2.4.2.jar"]
```
* Build the image




* Let's run the container `docker container run -d -P --name myspc2 myspc:alpine`



### Immutable Infrastructure

* Any infra changes will not be done on infra directly rather we create some infra as code option and change the configuration

### Additional instructions for Dockerfile

 1. _**LABEL**_ : This instruction adds metadata


    [ Refer Here : https://docs.docker.com/reference/dockerfile/#label ]

    + Refer Here for the changes done for spc image
    + Let's inspect the image `docker image inspect spc:1.0.0.1` and observe the labels section



 2. _**ADD , COPY**_ : 

* ADD instruction can add the files into docker image from local file system as well as from http(s)
* ADD instruction can have sources
    + local file system
    + git repo
    + url
* COPY supports only local file system
* Let's use ADD to download springpetclinic into docker image from url `ADD https://referenceapplicationskhaja.s3.us-west-2.amazonaws.com/spring-petclinic-2.4.2.jar /spring-petclinic-2.4.2.jar`

* For the changes

    [ Refer Here : https://github.com/asquarezone/DockerZone/commit/6800c2af1ee665f335cd6e250848e95a0611b976 ]

* copy the springpetclinic jar file into some local path on docker host. For the changes 

    [ Refer Here : https://github.com/asquarezone/DockerZone/commit/d7e1e440f0151ef0b10a2ba01af11a8fed3ec199 ]

#### What do we mean by running container in detached mode ?

* Let's try to start the docker container jenkins `jenkins/jenkins`




* docker container’s STDOUT and STDERR will be attached to your terminal and if we execute ctrl+c the container exits
* Running container normally will take us to attached mode
* In detached mode container executes and gives us back the access to terminal



* Once we start the container in detached mode we can still view the STDOUT and STDERR by executing `docker container attach <container-name-or-id>`
* To exit from attach mode `Ctrl+PQ`

#### Docker container will be in running state as long as command in cmd is running

* Consider the following _**Dockerfile**_
```
FROM amazoncorretto:11
LABEL author="shaikkhajaibrahim"
LABEL organization="qt"
LABEL project="learning"
# Copy from local file on Docker host into docker image
COPY spring-petclinic-2.4.2.jar  /spring-petclinic-2.4.2.jar
EXPOSE 8080
CMD ["sleep", "10s"]
```
* We have sleep 10s i.e. this will run for 10s and finish
* Docker container will move to exited stated once the command in CMD has finished executing



#### Exercise:

* Create a ubuntu vm
* install apache2 and note the ExecStart command for apache2
* install tomcat9 and note the ExecStart command for tomcat9
* stop the services (systemcl stop servicename)
* become a root user (sudo -i)
* try executing the ExecStart command directly and see if the application is running

#### .net application manual process

* For manual steps

    [ Refer Here : https://docs.nopcommerce.com/en/installation-and-upgrading/installing-nopcommerce/installing-on-linux.html ]

* This  application requires
    + mysql  server (lets ignore this)
    + . dotnet runtime 7.0
    + it runs on port 5000
* Steps:
    + Ensure  dotnet 7 is installed
    + Download  application from Refer Here
    + unzip the application into some folder
    + create two directories bin and logs
    + Run the application using command `dotnet --urls "http://0.0.0.0:5000" Nop.Web.dll`

* We have created the following `Dockerfile`
```
FROM mcr.microsoft.com/dotnet/sdk:7.0
LABEL author="khaja" organization="qt" project="learning"
ADD https://github.com/nopSolutions/nopCommerce/releases/download/release-4.60.2/nopCommerce_4.60.2_NoSource_linux_x64.zip /nop/nopCommerce_4.60.2_NoSource_linux_x64.zip
WORKDIR /nop
RUN apt update && apt install unzip -y && \
    unzip /nop/nopCommerce_4.60.2_NoSource_linux_x64.zip && \
    mkdir /nop/bin && mkdir /nop/logs
EXPOSE 5000
CMD [ "dotnet", "--urls", "http://0.0.0.0:5000", "Nop.Web.dll" ]
```
* This is not working



* Try fixing
* Try using alpine version of dotnet 7 for the same application

#### Dockerfile instructions

 1. _**WORKDIR**_ : This instruction sets the working directory 
 
    [ Refer Here : https://docs.docker.com/reference/dockerfile/#workdir ]

#### Fixing issue with startup of .net application

* For the Dockerfile

    [ Refer Here : https://github.com/asquarezone/DockerZone/commit/14555855fddd6adb5da3d07c1833e09b5639f438 ]



* The document to host the .net application on `0.0.0.0`

    [ Refer here : https://andrewlock.net/5-ways-to-set-the-urls-for-an-aspnetcore-app/ ]

#### Setting Environment Variables in the container

* ENV instruction 

    [ Refer Here : https://docs.docker.com/reference/dockerfile/#env ]

* This instruction adds environmental variable in the container and it also allows us to change environmental variables while creating containers
* For the changes done to include environmental varibles

    [ Refer Here : https://github.com/asquarezone/DockerZone/commit/07243fd3a974b947c407f12570a9979cbc592025 ]

* docker container exec will allow us to execute commands in the container



* `docker container exec -it <c-name> <shell>` will allow us to login into container



* ARG instruction allows us to set the values while building the image 

    [ Refer Here : https://docs.docker.com/reference/dockerfile/#arg ]

* For the BUILD ARGS added

    [ Refer Here : https://github.com/asquarezone/DockerZone/commit/60a624985b34fe5b9a82c3fa87204738fd139cd8 ]

* Build args can be set while creating images. BUILD ARG can be used by using `${ARG_NAME}`
* We have build two images by changing the HOME_DIR and DOWNLOAD_URL Build args
```
docker image build --build-arg DOWNLOAD_URL=nopCommerce_4.60.2_NoSource_linux_x64.zip -t nop:1.0.2 .
docker image build --build-arg HOME_DIR=/publish -t nop:1.0.0 . 
```
* USER: 

    [ Refer Here : https://docs.docker.com/reference/dockerfile/#user ]

* For the changes done to add a non root user to run the nop commerce application

    [ Refer Here : https://github.com/asquarezone/DockerZone/commit/9e69fbecc61c60df83697c69d03867e4679a976d ]



#### Exercise:

* Gameof life application:
    + This requires java 8
    + this requires tomcat 8 or 9
    + copy the gameoflife.war application into webapps folder of tomcat Refer Here
    + This runs on port 8080

