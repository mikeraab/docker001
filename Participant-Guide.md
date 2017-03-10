# Intro to Containers Hands On Lab (DRAFT IN PROGRESS)

This Hands on Lab (HOL) will take the particant through the basics of containerization, explore it's advantages and introduce Docker technology with begginer level excersises.  The topics to be covered in this 2 hour session are:

1.  [Intro to Basic Container Concepts](../master/Participant-Guide.md#intro-to-basic-container-concepts)
2.  [Verify Docker Engine Hands on Lab Environment](../master/Participant-Guide.md#verify-docker-engine-hands-on-lab-environment)
3.  [Hello Helloworld](../master/Participant-Guide.md#hello-helloworld)
5.  Create a Dockerfile and Docker Image
6.  Push an Image to your Docker Hub Account
7.  Install Docker Compose (if needed)
8.  Create Wordpress "stack"
9.  Basics of Persistent storage
10. Use Github and Docker Hub together to build an Image and Run the Container
11. Demo ofContainer Cloud Service Showing Participant's Containers


### Requirements to Complete this HOL:

* A pre-configured Docker-Engine environment - See [Prerequisites](../master/Prerequisites.md)

* Access to the Internet

* A laptop

* A text editor to keep notes and snippets as you go along

* A Docker Hub account

* A Github account

## Intro to Basic Container Concepts

A container is a runtime instance of a docker image.

[https://docs.docker.com/engine/reference/glossary/#/container](https://docs.docker.com/engine/reference/glossary/#/container)

Docker is the company and containerization technology.

[https://docs.docker.com/engine/reference/glossary/#/docker](https://docs.docker.com/engine/reference/glossary/#/docker)  

Containers have been around for many years.  Docker created a technology that was usable by mere humans, and was much easier to understand than before.  Thus, has enjoyed a tremendous amount of support for creating a technology for packaging applications to be **portable and lightweight.**

However, there have been and still are variations on container technology.  Here are some of the technologies we have seen through the years.

History of Linux Containers

<img src=images/002-container-history.png />
***

Now, lets look at how a virtual machine (VM) is different from a container

While containers may sound like a VM, the two are distinct technologies. With VMs each virtual machine includes the application, the necessary binaries and libraries and the **entire guest operating system.**

Whereas, Containers include the application, all of its dependencies, but share the kernel with other containers and are not tied to any specific infrastructure, other than having the Docker engine installed on it’s host – allowing containers to run on almost any computer, infrastructure and cloud.  

<img src=images/002-vm-vs-container.png />
***

> *Note - at this time, Windows and Linux containers require that they run on their respective kernel base, therefore, Windows containers cannot run on Linux hosts and vice versa.*

Docker images are a collection of files, which has everthing needed to run the software application inside the container.  However, they are ephemeral, meaning that any data that is written inside the container, while it is running, will not be retained.  

If the container is stopped and restarted from its image, the container will run exactly the same as the first time, absent of any changes made during the last run cycle.  Changes to the container either have to be made during the image creation process, using the Dockerfile that become part of the image, or data can be retained by mounting a persistent storage volume, from inside the container to the outside.  This will be explored further in the HOL exercises below. 

<img src=images/004-docker-images.jpg />
***

Jumping back a bit, there is a new nomenclature that Docker introduces, here are terms that you will need to be familiar with.  

Each of these will be explored in this HOL.

<img src=images/005-docker-terms.jpg />
***

Additionally, the Docker Engine is the core piece of technology that allows you to run containers.  A in order for a container to run on any Linux host, at a minimum, the Docker Engine needs to be installed.

<img src=images/004-docker-engine.jpg />
***

Ok, so those are some of the mechanics of the technology, but why is Docker popular among all types of IT people?  Lets look at these proof points from Developers and IT Ops.

<img src=images/004-why-containers.jpg />
***

Additonally, in speaking with hundreds of organizations that are exploring and using Docker, these are the core advantages that Docker brings.  

<img src=images/005-docker-usage.jpg />
***

All of this is part of a transformation of technologies along a number of fronts, and is the basis for modern agile application development.

<img src=images/003-evolution.jpg />
***

##Verify Docker Engine Hands on Lab Environment

In this first section you are going to verfify that you are able to connect to your Docker Engine environment as requested in the [Prerequisites document](../master/Prerequisites.md).  Please access the environment now, and execute the following commands at the terminal.

First, run as root, so that you do not have to preface everthing with sudo.

```
$ sudo -s
```

Now, here is the Docker specific command to check what version is installed.

```
$ docker --version
```

If Docker is installed and running, you should see an output of something like this.

```
Docker version 1.1x.x, build 57bf6fd
```

##Hello Helloworld

