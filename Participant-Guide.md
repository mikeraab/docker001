# Intro to Containers Hands On Lab (DRAFT IN PROGRESS)

This Hands on Lab (HOL) will take the particant through the basics of containerization, explore it's advantages and introduce Docker technology with entry level excersises.  The topics to be covered in this 2 hour session are:

1.  [Intro to Basic Container Concepts](../master/Participant-Guide.md#intro-to-basic-container-concepts)
2.  [Verify Docker Engine Hands on Lab Environment](../master/Participant-Guide.md#verify-docker-engine-hands-on-lab-environment)
3.  [Hello Helloworld](../master/Participant-Guide.md#hello-helloworld)
5.  [Create a Dockerfile and Docker Image](../master/Participant-Guide.md#create-a-dockerfile-and-docker-image)
6.  [Push an Image to your Docker Hub Account](../master/Participant-Guide.md#push-an-image-to-your-docker-hub-account)
7.  [Install Docker Compose](../master/Participant-Guide.md#install-docker-compose)
8.  [Create Wordpress "stack"](../master/Participant-Guide.md#create-a-wordpress-stack)
9.  [Basics of Persistent storage](../master/Participant-Guide.md#basics-of-persistent-storage)
10. [Use Github and Docker Hub together to build an Image and Run the Container](../master/Participant-Guide.md#use-github-and-docker-hub-together-to-build-an-image-and-run-the-container)
11. [Demo of Oracle Container Cloud Service Showing Participant's Containers](../master/Participant-Guide.md#demo-of-oracle-container-cloud-service-showing-participants-containers)


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

## Push an Image to your Docker Hub Account

**Registries**

Registries store Docker images.  Using a registry is the first step towards moving Docker off the laptop.  The most widely used registry is the Docker Hub: [https://hub.docker.com](https://hub.docker.com) 

> *Note - in this exercise you will need a Docker Hub account to use the public Docker registry.  If you do not have one already, you can signup for free, navigate to: [https://hub.docker.com/](https://hub.docker.com/)*

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

## Install Docker Compose

> *Note -if the Docker Engine setup you used, installed direcly on Windows 10 or your Mac, most likely, Docker Compose is installed.

**Introduction to Docker Compose**

What is Docker Compose, why use it?

According to Docker: Docker Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a Compose file to configure your application’s services. Then, using a single command, you create and start all the services from your configuration.
Install Docker compose

> *Note - docs are here: [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)*

Use these specific below commands in your terminal for this exercise to install in your home directory.  This example is for Oracle Linux 6

```
$ curl -L "https://github.com/docker/compose/releases/download/1.10.0/docker-compose-$(uname -s)-$(uname -m)" -o /home/opc/docker-compose
```

Change the executable permissions

```
$ chmod +x /home/opc/docker-compose
```

Verify and check which version of Docker Compose was installed

```
$ ./docker-compose --version
```

## Create a Wordpress "stack"

Follow these steps to create a simple Wordpress stack referenced here:

[https://docs.docker.com/compose/wordpress/](https://docs.docker.com/compose/wordpress/) 

Here is a synopsis of the steps in the above URL:

Use an editor or VI to create a file named docker-compose.yml

```
vi docker-compose.yml
```

that contains the following text **(important, copy and paste from the YAML below)

```
version: '2'

services:
   db:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: wordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "80:80"
     restart: always
     volumes:
       - /var/www/html:/var/www/html:rw
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_PASSWORD: wordpress
volumes:
     db_data:
```

Save docker-compose.yml

If you are using  VI, save the file by typing the Esc key - colon - w (for write) - q (for quit):

	
```
esc : w q 
```

Run the Wordpress stack by this command

```
$ ./docker-compose up -d
```

Verify the running stack, by visiting the Wordpress setup page.

In your browser, navigate to the IP of the Docker host, port 8000

```
http://docker_host_ip/wp-admin/install.php
```

**Congratulations, you have successfully launched your first Wordpress app in Docker!**

Stop and Remove the running Wordpress and Database containers by using their short id.

Remember this from a previous exercise

```
docker ps -a

docker stop short_id

docker rm short_id
```

Repeat for next container

## Basics of Persistent storage

 **Understand Docker Volumes

This section will explore data persistence through the use of host data volumes.

A full introduction to Docker volumes is located here: [https://docs.docker.com/engine/tutorials/dockervolumes/](https://docs.docker.com/engine/tutorials/dockervolumes/)

In short, unless a container volume is mounted to a persistent host volume, any data stored within the container will be lost when the container is removed.

So let's explore how data is persisted in the Wordpress stack we just used.

In your browser navigate to Host_IP and append it with the Wordpress initialization URL: /wp-admin/install.php.  

> *Note - this is the same setup URL you saw when we deployed with Docker Compose above, however this time, we are going to setup Wordpress and create blog post.*

```
http://docker_host_ip/wp-admin/install.php
```

First, select your language:

<img src=images/033-wp-setup1.png />
***

Setup the Wordpress login details.  Be sure to keep the Username and Password in your notes:

<img src=images/034-wp-setup2.png />
***

Click the "log In" button to log into to Wordpress:

<img src=images/034-wp-setup2.png />
***

Login using the credentials you created earlier:

<img src=images/036-wp-login2.png />
***

Select "Write your first blog post" in the Next Steps section:

<img src=images/037-write-blog.png />
***

Create a sample blog post.  Include an image of your choosing, if you would like and click Publish:

<img src=images/038-publish-blog.png />
***

Click on the "Permalink" to navigate to the blog post:

<img src=images/039-permalink.png />
***

Copy the Permalink URL of the blog post and keep this in your notes, as you will need it later:

<img src=images/040-view-blog.png />
***

Stop and Remove the running Wordpress and Database containers by using their short id.

Remember this from a previous exercise:

```
docker ps -a

docker stop short_id

docker rm short_id
```

Repeat for next container

Verify in the browser that the Wordpress blog post is gone by refreshing the page:

<img src=images/043-refresh.png />
***

Redeploy the Wordpress stack.  

```
$ ./docker-compose up -d
```

navigate back to the blog post URL that you noted, in your browser and refresh the page:

<img src=images/047-refresh-blog.png />
***

The data persisted because it was written to the host volume, and then re-joined to the containers when they were re-deployed on the same hosts.

## Use Github and Docker Hub together to build an Image and Run the Container

**Requirements: user account for Github and DockerHub**

> If you do not have a Github account, get a free one here: [https://github.com/join](https://github.com/join) 

Now, you will explore another method of creating a image using GitHub and Docker Hub.

> *Note - in this exercise you will build a Dockerfile from Github on Docker Hub, deploy the latest version, then modify the Index.html in Github to trigger an automated Docker image build in DockerHub, and then verify the new build and image as a running container in.*

**To begin, fork this Github repo to your own


**Once you have completed all the steps above, follow these steps:**

In your Github account navigate to the URL where you have forked the above Docker-Hello-World.  Replace your Github username in the below URL.

https://github.com/*username*/docker-images/blob/master/ContainerCloud/images/docker-hello-world/

On the Github page, click on the link for "Index.html":

<img src=images/049-hw-index.png />
***

This is the HTML for the home page of the HelloWorld Demo from above.  

You are going to modify this Index.html to create a new "Hello Earth" page, this will automatically trigger a new image build in Docker hub.  You will then run the resulting container to observe the changes.

Edit the page via the pencil icon and make these changes:

<img src=images/050-edit-index.png />
***

On line 8, change the background color to 

```
black
```

Add a new line 9 with the text

```
color: white;
```

On line 14, edit the H2 header to this, replacing "your_name" and "your_city" with your own name and city
```
<h2>Hello Earth from "your_name" at Oracle Code in "your_city"!</h2>
```

Create a new line after </body> on line 15, and add this line for an image of the earth 

```
<img src="http://www.freeimageslive.com/galleries/space/earth/pics/a17_h_148_22725.gif">
```

Check to see that it looks just like this (edits are highlighted in the red boxes):


<img src=images/051-index-edit.png />
***

Scroll Down and Commit your Changes.  Add a description and press the "Commit Changes" button:

<img src=images/052-commit-index.png />
***

This will trigger a new automated build in Docker Hub, which will run the Dockerfile, which incorporates the new changes in index.html as part of the build process.  

It will take a few minutes for this to complete in Docker Hub.  When it does, Success will be noted in the Status column:

<img src=images/053-docker-build.png />
***

Back in your Docker environment.


Once the container is running, verify the Host that the container is running on: 

Visit the host’s IP on port 8080 and observe your changes and a new Hello Earth!

<img src=images/058-hello-earth.png />
***

Tweet to the world that you have created your own Hello Earth containerized app!

'''
Check out the HelloEarth #Docker app that I just created at #OracleCode posted in my Docker Hub https://hub.docker.com/r/yourname/hello-earth
'''

**Congratulations!**  You have successfully completed this Hands On Lab!

***

##Demo of Oracle Container Cloud Service Showing Participant's Containers

Check out running your container in the Oracle Container Cloud Service:

* [Get Started for Free](https://cloud.oracle.com/tryit)

***

## Summary/Recap Pointer to Further Resources

* [Oracle Github](https://github.com/oracle/docker-images)

* [Oracle Container Registry](https://container-registry.oracle.com)

* [Oracle Container Registry Docs](http://docs.oracle.com/en/cloud/iaas/container-cloud/index.html)

Oracle Blogs:

* [Containers, Docker and Microservices](https://community.oracle.com/community/cloud_computing/containers-docker-and-microservices)

* [Container Cloud Service Blog](https://community.oracle.com/community/cloud_computing/infrastructure-as-a-service-iaas/oracle-container-cloud-service)

**DONE**

