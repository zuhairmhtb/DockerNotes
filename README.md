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

The format is <docker port>:<desktop port>

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