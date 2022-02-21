# Docker Notes

1. Run (install if required) an image:

$ docker run <image_name>

It takes an image and converts it into a running container.


2. View docker images:

$ docker images

3. Run docker with interactive terminal of docker (-ti):

$ docker run -ti ubuntu:latest bash

4. Exit interactive terminal:

$ exit

5. View all containers including the stopped ones (-a):

$ docker ps -a

6. View last running container:

$ docker ps -l

7. Create an image from the container along with files inside the container:

$ docker commit <container id>

8. Create a tag name for a newly created image

$ docker tag <sha id generated when container was created except the sha256: part>

or

$ docker commit <container_id> <tag_name>

9. Run container

$ docker run

It starts a container by giving image a name and a process to run in that container. The container stops when that process exits. It is a main process. Even if other processes are running in that container, it will still exit when the main process stops.

10. Start a container and remove it when it exits

$ docker run --rm -ti ubuntu bash -C "sleep 3; echo all done"

11. A container can be kept running as a detached container using:

$ docker run -d -ti ubuntu bash

It runs the container in background

12. Attach a background container

$ docker attach <container name>

13. When inside a container terminal, to exit the terminal and keep the container running in background:

press Ctrl-p + Ctrl-q

14. To add process to a running container:

$ docker exec -ti <container name> <process name>

e.g. docker exec -ti <container name> bash

15. Docker keeps output of a container in docker logs as long as the container is running.

$ docker logs <container name>

Don't let the outputs of the container get huge

16. Stop container

$ docker kill <container name>

17. Delete a container

$ docker rm <container name>

18. To enforce limits like max memory to a container run:

$ docker run --memory maximum-allowed-memory <image name> command

$ docker run --cpu-shares 

It is relative to other containers to add CPU usage limit

$ docker run --cpu-quota

It is used to limit CPU usage in general

19. To expose a port in a container:

$  docker run --rm -ti -p 45678:45678 -p 45679:45679 --name echo-server ubuntu:latest bash

The format is <desktop port>:<docker port>

20. If you want to acces a port of the desktop on which a container is running, from the terminal inside the container you should use the address which refers to the host:

$ nc host.docker.internal <port>

host.docker.internal is a default name for the host desktop. On windows, IP address of the machine works too.

21. To view port mapping of a container run:

$ docker port <container name>

22. To view existing networks of docker:

$ docker network ls

Bridge: Used by containers that do not specify preference to be put into any other network

Host: When you want a container to not have any network isolation at all. It has some security concerns

None: When container should not have any networking.

23. Creating a network

$ docker network create <network name>

24. When creating a container, specify  container using --net <network name>

$ docker run --rm -ti --net <network name> --name <container name> ubuntu:latest bash

Containers inside the same private network can communicate with each other

25. Connect a running container to a network

$ docker network connect <network name> <container name>

A container can belong to multiple networks.

26. Pull images:

$ docker pull

27. Push images:

$ docker push

28. Remove images

$ docker rmi <image id>

29. Volumes are shared folders or virtual disks where you can store data and share between containers in the host or with the host. There are two types of volumes:

    a. Persistent

    b. Ephemeral

Volumes are not part of images.

30. Create a shared folder in the container to share with the host

$ docker run -ti -v /path/to/folder:/<path within the container that contains the shared folder> ubuntu bash

e.g. docker run -ti -v /example:/shared-folder ubuntu bash

The folder 'shared folder' will then be available in 'example' folder on the host machine. When sharing a file inside a container, the file must be created first before sharing the file or else docker will assume it is a folder name.


31. Create a shared folder in the container to share with other containers and not the host

$ docker run -ti -v /<shared folder name> ubuntu bash

The folder is not available to the host.

To access the shared folder from another container run:

$ docker run -ti --volumes-from <container namew which contains the shared folder> ubuntu bash

The shared folder will then be available in this container. When all containers sharing a volume are kiled, only then does the shared volume get deleted.
Therefore, volumes are ephemeral.

32. Docker images are retrieved from registies and published through it. A registry is a piece of software that manage and distribute images. You can host your own registries to keep data private.

33. Search packages

$ docker search <package name>

34. Login to docker hub:

$ docker login

35. Push an image to docker hub

$ docker tag <image name> <docker hub username>/<image name in docker hub>  [ Creates a new image from another image]

$ docker push <docker hub username>/<image name in docker hub>

Don't push images to public docker hub containing confidential informtion.

36. Docker files are small program to create an image. You run the program with:

$ docker build -t <image name> .

The dot refers to the current directory which should contain the docker file. The -t tag creates a name for the image.

Each step of running a dockerfile is cached. Docker can skip lines that weren't changed since the last build.

The parts that change the most belong at the end of the dockerfile.

Dockerfiles are not shell scripts.

Processes you start on one line will not be running on the next line. If you want two programs to keep running at the same time, the commands to run the programs should be on the same line. If they are on different lines, the second program will start after the first one ends.

Environment variables you set will be available on the next lines.

37. To build a docker file:

    a. Make a file called dockerfile

        Filename: Dockerfile

        FROM busybox # The image to begin with

        RUN echo "Building simple docker image" # Run a command through shell and after running the container it saves it into the an image

        CMD echo "Hello container" # Run container after saving the image

    Finally it creates an image

    b. Build the dockerfile:

        $ docker build -t hello .

    c. Installing a program with dockerfile

        Filename: Dockerfile

        FROM debian:sid

        RUN apt-get -y update

        RUN apt-get -y install nano

        ADD notes.txt /notes.txt # Add content from machine to container

        CMD ["/bin/nano", "/notes.txt"]


    d. 'ADD' commnand can copy files (given above), uncompress tar file and copy it to a container's folder e.g. ADD exmaple.tar.gz /folder and download content from a URL and copy it into a folder e.g. ADD example.com/download/project.rpm /project

    e. 'ENV' statement sets environment variables for all steps when building a docker file and the variables will be available in the image which is created.

        e.g. ENV DB_HOST=example.com

    f. 'ENTRYPOINT' statement specifies the start of a command to run

    g. 'CMD' specifies while command to run.

    h. 'EXPOSE' statement maps port of a container.

    i. 'VOLUME' statement defines shared or ephemeral volume. Generally, avoid shared volumes as it will remove the portablility of docker file as the folder is only available on the current machine.

    j. 'WORKDIR' command sets the working directory where commands will get executed.

    k. 'USER' statement sets which user container will run as.

38. To inspect a docker container run:

    $ docker inspect --format '{{.State.Pid}}' <container name>

It returns the process id of the running container.



