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
* Install
    * _**Linux VM**_:
        * Docker can be installed by following instructions over here 
        
        [ Refer Here : https://docs.docker.com/engine/install/ ]

        * script based installation 
        
        [ Refer Here : https://get.docker.com/ ]
       ```
       curl -fsSL https://get.docker.com -o get-docker.sh
       sh get-docker.sh
       ```
       9th picture
       * Docker allows communication to the unix socket for the users who belong to docker group. so lets add current user to docker group `sudo usermod -aG docker <username>`. logout and login
* Now execute `docker container run hello-world`

10 th picture

#### Windows 10/11 (Non Home editions)

* [ Refer Here : https://docs.docker.com/desktop/install/windows-install/ ]
* I will not recommend installing docker on windows

#### Docker Playground

* Create a Docker Hub account 

    [ Refer Here : https://hub.docker.com/ ]

* This playground gives a linux machine with docker installed for 4 hours for free

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

 * Applications bring revenue to enterprises and to run applications we need servers, os etc

#### Let's create a linux server and install tomcat in it
* Create a linux server (AWS/Azure/GCP) 

![alt text](shots/11.PNG)

* We have experimented in the linux vm
    * network interface gives network connectivity
    * cpu, ram and disk are available
    * to install softwares we have used package manager apt
* Commands
```
sudo apt update
sudo apt install net-tools openjdk-11-jdk tomcat9 -y
# to check network
ifconfig
# to check java
java -version
# to check tomcat
sudo systemctl status tomcat9
```
* We were able to exactly the similar operations inside container as well.

#### Let me take a application

* This is spring pet clinic application
* Let's try to run this application on linux

    [ Refer Here : https://github.com/spring-projects/spring-petclinic ]

* To build the code
```
sudo apt update
sudo apt install openjdk-17-jdk maven -y
git clone https://github.com/spring-projects/spring-petclinic.git
cd spring-petclinic
# java package
mvn package
java -jar target/spring-petclinic-3.0.0-SNAPSHOT.jar
```
* Docker way of working
  * We create a docker image (docker packaging format)
    * We need to create Dockerfile
    * push the image to registry (docker hub)
    * create the container using the image anywhere
* Dockerfile
```
FROM amazoncorretto:17-alpine-jdk
LABEL author=khaja
ADD target/spring-petclinic-3.0.0-SNAPSHOT.jar /springpetclinic.jar
EXPOSE 8080
CMD ["java", "-jar", "/springpetclinic.jar"]
```
* Create docker image
```
docker image build -t spc:1.0 .
```
* to create container
```
docker container run -d -P spc:1.0
docker container run -d -P spc:1.0
docker container run -d -P spc:1.0
```
#### Next Steps

* How is container able to give ip address/storage/process/cpu/ram etc
* Container architectures (3 versions)
* understanding image and container
* docker container life cycle
* containerization
* networking , storage aspects

#### How Isolations are created or How Containers Work

* Each container is getting a
    * new process tree
    * disk mounts
    * network (nic)
    * cpu/memory
    * users
* Docker Internals

    [ Refre here : https://directdevops.blog/2019/01/31/docker-internals/ ]

![alt text](shots/12.PNG)

### Docker Architecture

_**Generation 1:**_

* This was first gen, Where docker daemon used lxc (a linux kernel feature) to create containers

![alt text](shots/13.PNG)

_**Generation2:**_

* Since docker was relying on lxc which was kernel feature, updates to kernel frequently used to break containers created by docker
* So docker has created its own component called libcontainer (libc) to create containers
* Docker wanted containers to be multi os and lxc was definetly not the way forward

![alt text](shots/14.PNG)

* Adoption of docker was drastically increased as it was stable

_**Generation 3:**_

* In this generation, docker engine was revamped from monolith to multi component architecture and the images and containers were according to OCI (open container initiative) image spec and runtime spec.
* In the latest architecture
* docker daemon exposes api’s to listen requests from docker client.
* Passes the requests to containerd. This manages the lifecylcle of container
* containerd forks a runc process which creates container. once the container is created the parent of the container will be docker shim

![alt text](shots/15.PNG)

#### Creating our first docker container

 _**docker container creation:**_

* To create container we need some image in this case lets take `hello-world`
* The command `docker container run hello-world` executed
* What happens
    * docker client will forward the request to docker daemon
    * docker daemon will check if the image exists locally. if yes creates the container by using image
    * if the image doesnot exist, then docker daemon tries to download the image from docker registry connected. The default docker registry is docker hub.
    * Downloading image into local repo from registy is called as pull.
    * Once the image is pulled the container is created.

![alt text](shots/16.PNG)

* Registry is collection of docker images hosted for reuse.
* Docker hub 

    [ Refer Here : https://hub.docker.com/search?q= ]

#### Playing with containers

* Create a new linux vm and install docker in it

17th picture

18th picture

* Open all ports
    * AWS

    19th picture

    20th picture

#### Check docker images in the host

21st picture

22nd picture

23rd picture

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
24th picture

* Let's pull the jenkins image with latest version

25th picture

* Let's find the alpine and pull the image

26th picture

#### Remove images from local

* Every image will have unique image id and image name
* We can delete individually `docker image rm alpine:3.17`
* if I have to delete all the images `docker image rm $(docker image ls -q)`

27th picture

28th picture

#### Create a container with nginx

* To create and start the container we use run command

29th picture

* _**note:**_ i will be using -d for some time and we will discuss importance of this in next session
* every container gets an id and a name. name can be passed while creating container, if not docker will give random name

30th picture

* Remove all the running containers `docker container rm -f $(docker container ls -q )`

31st picture

32nd picture

* Remove specific container

33rd picture

* Remove all containers `docker container rm -f $(docker container ls -a -q )`

34th picture

* _**Exercise:**_ Start and stop containers



