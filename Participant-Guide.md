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

Now, you explore Docker images and the Docker Hub

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

## Stop and Re-run Your Container with a More Descriptive Name

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
