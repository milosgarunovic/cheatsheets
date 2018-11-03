### Docker images

* Read-only template which is base foundation to create a container from.
* Every image starts from a base image; for example Ubuntu or Linux image.
  * You can pick up an existing image from the internet:
  * For java, there is `openjdk`, `tomcat`, `wildfly`...
* Set up an config for image and just focus on the app itself.
* Images are created using a series of commands, called **instructions**. Instructions are placed in **Dockerfile**.
  * The result of running build process on Dockerfile will be the image.
  * Each instruction creates a new **layer** in the image.
  * That image layer becomes the parent for the layer created by the next instruction.
* It's important to know that Docker uses images to run your code, not the Dockerfile. The Dockerfile is used to create
the image when you run `docker build` command.
* Also, if you publish your image to Docker Hub, you publish a resulting image with its layers, not a Dockerfile itself.

### Layers

* Are read-only. Are a bit same as the commit history of a Git repository.

### Containers

A running instance of an image is called a container. Docker launches them using the Docker images as read-only 
templates. If you start an image, you have a running container of this image. To run a container, we use the 
`docker run` command:  
`docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`  

To stop a container, use the `docker stop` command:  
`docker stop`  
A container when stopped will retain all settings and filesystem changes (in the top layer that is writable). All 
process running in the container will be stopped and you will lose everything in memory. This is what differentiates a 
stopped container from a Docker image.

To list all containers you have on your system, either running or stopped, execute the `docker ps` command:
`docker ps -a`

To remove a container, you can just use the `docker rm` command. If you want to remove a couple of them at once, you can
use the list of containers (given by the `docker ps` command) and filter:
`docker rm $(docker ps -a -q -f status=exited)`

We've said that a Docker image is always read-only and immutable. IF we didn't have the posibility to change the image,
it would not be very useful. When the container is started, the writable layer on top of the layers stack is for our
disposal. We can actually make changes to a running container; this can be adding or modifying files, the same as
installing a software package, configuring the OS and so on. If you modify a file in the running container, the file
will be taken out of the underlying (parent) read-only layer and placed in the top, writable layer. Our changes are only
possible in the top layer. The union filesystem will then cover the underlying file. The original, underlying file will
not be modified; it still exists safely in the underlying, read-only layer. By issuing the 
`docker commit` command, you create a new read-only image from a running container (and all it changes
in the writable layer):  
`docker commit <container-id> <image-name>`  
The `docker commit` command saves changes you have made to the container in the writable layer. To avoid corruption or
inconsistency, Docker will pause a container you are committing changes to. The result of the `docker commit` command is
a brand new, read-only image, which you can create new containers from. In response to successful commit, Docker will
output the full ID of a newly generated image.  
Creating images by altering the top writable layer in the container is useful when debugging and experimenting, but it's
usually better to use a Dockerfile to manage your images in a documented and maintainable way.

### Docker registry, repository, and index

### Commands
`docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`  
`docker ps -a -s`  
`docker images`  
`docker inspect {image-name}`  

### Additional tools
* IntelliJ IDEA Docker integration plugin
* Kubernetes

### Docker plugins