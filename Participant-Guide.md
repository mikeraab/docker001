# Intro to Containers Hands On Lab (DRAFT IN PROGRESS)

This Hands on Lab (HOL) will take the particant through the basics of containerization, explore it's advantages and introduce Docker technology with begginer level excersises.  The topics to be covered in this 2 hour session are:

1.  [Intro to Basic Container Concepts](../master/Participant-Guide.md#intro-to-basic-container-concepts)
2.  Verify Docker Engine Hands on Lab Environment
3.  Hello Helloworld
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

History of Linux Containers

<img src=images/002-container-history.png />
***

VM vs Container

While containers may sound like a virtual machine (VM), the two are distinct technologies. With VMs each virtual machine includes the application, the necessary binaries and libraries and the **entire guest operating system.**

Whereas, Containers include the application, all of its dependencies, but share the kernel with other containers and are not tied to any specific infrastructure, other than having the Docker engine installed on it’s host – allowing containers to run on almost any computer, infrastructure and cloud.  

<img src=images/002-vm-vs-container.png />
***

> *Note - at this time, Windows and Linux containers require that they run on their respective kernel base, therefore, Windows containers cannot run on Linux hosts and vice versa.*

Docker images are a collection of files, which has everthing needed to run the software application inside the container.

<img src=images/004-docker-images.jpg />
***

