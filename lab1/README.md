# hello earth
Another hello world type docker container example

This example is used for this Hands On Lab and runs on port :80

Fork this example to your GitHub account, then after you build it in your Docker Hub account, replace "myDockerUser" with your Docker Hub user name in the Docker commands below.

To pull this image: 
```
docker pull myDockerUser/helloworld:latest
```

To run this image: 
```
docker run -p 80:80/tcp "myDockerUser/helloworld:latest"
```
