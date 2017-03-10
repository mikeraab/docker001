# Intro to Containers Hands On Lab (DRAFT IN PROGRESS)

This Hands on Lab (HOL) will take the particant through the basics of containerization, explore it's advantages and introduce Docker technology with begginer level excersises.  The topics to be covered in this 2 hour session are:

1.  [Intro to Basic Container Concepts](../master/Participant-Guide.md#intro-to-basic-container-concepts)
2.  [Verify Docker Engine Hands on Lab Environment](../master/Participant-Guide.md#verify-docker-engine-hands-on-lab-environment)
3.  [Hello Helloworld](../master/Participant-Guide.md#hello-helloworld)
5.  [Create a Dockerfile and Docker Image](../master/Participant-Guide.md#create-a-dockerfile-and-docker-image)
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

<img src=images/006-evolution.jpg />
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

## Hello Helloworld

Run Docker’s Hello-world example

	
```
$ docker run hello-world
```

Since the "hello-world" image is not available locally on the host, the command automatically pulls the hello-world image from the public Docker Hub image repository and runs the container in the foreground.

**Congratulations, you have just run your first Docker container!**

List all containers (- a = running **and** stopped)

```
$ docker ps -a
```

In this excercise you will explore another Docker image from Docker Hub

**Browse to another public image Helloworld example on Docker Hub.  Run it.**

Open a browser and go to this URL:

[https://hub.docker.com/r/karthequian/helloworld](https://hub.docker.com/r/karthequian/helloworld)

Pull the image from the Docker Hub Registry

>  *Note - observe how the layers are pulled individually*

```
$ docker pull karthequian/helloworld:latest
```

Copy/Paste the Docker Run command from the Docker Hub page and add a -d option so the container runs in "detached" mode (as opposed to the foreground mode in the last exercise).  This frees up your terminal window.

```
$ docker run -d -p 80:80/tcp "karthequian/helloworld:latest"
```

Explore this Helloworld app in the browser.  Navigate to the IP of the Docker Host where it is running and note the number of visits.  (The IP is the same as the Host that you are SSH’d into http://host_ip or on your localhost http://localhost )


<img src=images/004-hello-world.png />
***
You are now actually using an application that is in the Docker container.  Refresh the browser and observe how the visits counts increments.  This is a live application. A simple example, but an example of the experience of using an application running in a container, which is no different than if it was not running in a container.

> *Makes you wonder about how many apps that you are using on a day to day basis, may indeed be running in a Docker container?*

Now, look at the name that Docker has assigned the Helloworld container that is running.  List all running containers.

```
$ docker ps
```

Notice that Docker has assigned a container name, something like "stoic_wilson" in the above?  What name did Docker give your container?  Remember this name, as we will use it in a bit.

> *Note - unless you specify a container name, Docker will assign a similar 2 part name automatically*

**Stop and Re-run Your Container with a More Descriptive Name**

Now, go back to the terminal window, stop the container and give it a more descriptive name, so that we could find it easier if there were many containers running.

Stop the Running Container - Replace **your_container** below with an actual name that you want to call your running container.

```
$ docker stop your_container
```

Now, remove the container with the "rm" command

```
$ docker rm your_container
```

Check to be sure that the container has been removed

```
$ docker ps -a
```

> *Note - containers can be stopped and removed by using their name **(if there are no dependent image layers)**, their long id or their short id*

Now run the container with a more descriptive name, such as "helloworld_app"

```
$ docker run -d --name helloworld_app -p 80:80/tcp "karthequian/helloworld:latest"
```

List all running containers again

```
$ docker ps
```

> *Is the container easier to find now, especially that there is context to the name of the container?  Especially if there were many containers running?*

Stop and Remove the container

```
$ docker stop helloworld_app
$ docker rm helloworld_app
```

Now, remove the container with the "rm" command

We are done with this part of the HOL.

## Create a Dockerfile and Docker Image

In this excersise you will build your wwn image from a Dockerfile

About DockerFiles

A Dockerfile is a recipe that starts with a base image, typically a thin Linux OS distribution such as Alpine Linux, and then layers on an app and configuration.  [According to Docker](https://docs.docker.com/engine/reference/builder/): 

*"Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using docker build users can create an automated build that executes several command-line instructions in succession."*

**Build the Docker image**

Use the [Docker Whale](https://docs.docker.com/engine/getstarted/step_three/) example to build our first image.  

Follow Steps 1 and 2 from this exercise:

[https://docs.docker.com/engine/getstarted/step_four/](https://docs.docker.com/engine/getstarted/step_four/)

Here is a synopsis of the steps in the above URL:

Make a directory to store your Dockerfile

```
$ mkdir mydockerbuild
```

Change to the new directory

```
$ cd mydockerbuild
```

In Step 1.3, use VI or editor of your choice, like nano.  

To use VI, if you are on Oracle Linux

```
$ vi Dockerfile
```

Create a text file name Dockerfile with these 3 lines

```
FROM docker/whalesay:latest

RUN apt-get -y update && apt-get install -y fortunes

CMD /usr/games/fortune -a | cowsay
```

In Step 1.8, after you are done adding the 3 lines to your Dockerfile with VI, save the file by typing the Esc key - colon - w (for write) - q (for quit):

	
```
esc : w q 
```

> *Note - the docs for VI are here: [https://www.cs.colostate.edu/helpdocs/vi.html](https://www.cs.colostate.edu/helpdocs/vi.html)*

Then per section 2, build your Docker image, be sure to include the "." at the end of the command

```
$ docker build -t docker-whale .
```

Then per section 4, list the images on your host and run the docker-whale image as a container

```
$ docker images
```

```
$ docker run docker-whale
```

Notice the output in the terminal.  Re-run the image a couple of times, as the container will run once, then stop.

**Registries**

Registries store Docker images.  Using a registry is the first step towards moving Docker off the laptop.  The most widely used registry is the Docker Hub: [https://hub.docker.com](https://hub.docker.com) 

> *Note - in this exercise you will need a Docker Hub account.  If you do not have one already, you can signup for free, navigate to: [https://hub.docker.com/](https://hub.docker.com/)*

**Tag and Push your new image to the Docker Hub registry.  In this exercise username will be your Docker Hub account name.**

First, log into your Docker Hub account from the terminal

```
$ docker login
```

When prompted, enter your Docker account username (lowercase), password and email

Now, tag and push your new docker-whale image to your account on Docker Hub

Substitute your Docker username below

```
$ docker tag docker-whale:latest username/docker-whale:latest
```

Push the Docker image to your account.  This will create a new repository called "docker-whale" for this image.

```
$ docker push username/docker-whale:latest
```

Navigate to your account page in Docker Hub via this URL, substituting your username:

[https://hub/docker.com/r/username](https://hub.docker.com/r/username)

Do you see the image that you pushed?

<img src=images/010-docker-hub.png />
***

Now, remove the local image and run the image from the registry

To do this, you must first remove the stopped container by using its short id, not its name.  Find the short id.

```
$ docker ps -a 
```

Copy the short id for the appropriate container, it will be similar to this format: ee31fe1dd8f8 and use the "rm" command to remove the container 

```
$ docker rm short_id
```

Now that the container is removed, you can remove the image and force the container to be run from the image on the Docker Hub with the "rmi" command

Remove the image that you pushed to the Docker Hub

```
$ Docker rmi username/docker-whale
```

Verify the images are removed.  View all Docker images with this command.

```
$ docker images
```

Now, run the image directly from your repository on Dockerhub, and force a new pull of the image (because the image does not exist locally)

```
$ docker run username/docker-whale
```

> *Note - if no tag is used, the default tag is "latest"*

Push an Image to your Docker Hub Account
