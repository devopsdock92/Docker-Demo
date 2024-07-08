# Docker Volumes

## Problem Statement

It is a very common requirement to persist the data in a Docker container beyond the lifetime of the container. However, the file system
of a Docker container is deleted/removed when the container dies. 

## Solution

There are 2 different ways how docker solves this problem.

1. Volumes
2. Bind Directory on a host as a Mount

### Volumes 

Volumes aims to solve the same problem by providing a way to store data on the host file system, separate from the container's file system, 
so that the data can persist even if the container is deleted and recreated.

![image](https://user-images.githubusercontent.com/43399466/218018334-286d8949-d155-4d55-80bc-24827b02f9b1.png)


Volumes can be created and managed using the docker volume command. You can create a new volume using the following command:

```
docker volume create <volume_name>
```

Once a volume is created, you can mount it to a container using the -v or --mount option when running a docker run command. 

For example:

```
docker run -it -v <volume_name>:/data <image_name> /bin/bash
```

This command will mount the volume <volume_name> to the /data directory in the container. Any data written to the /data directory
inside the container will be persisted in the volume on the host file system.

### Bind Directory on a host as a Mount

Bind mounts also aims to solve the same problem but in a complete different way.

Using this way, user can mount a directory from the host file system into a container. Bind mounts have the same behavior as volumes, but
are specified using a host path instead of a volume name. 

For example, 

```
docker run -it -v <host_path>:<container_path> <image_name> /bin/bash
```

## Key Differences between Volumes and Bind Directory on a host as a Mount

Volumes are managed, created, mounted and deleted using the Docker API. However, Volumes are more flexible than bind mounts, as 
they can be managed and backed up separately from the host file system, and can be moved between containers and hosts.

In a nutshell, Bind Directory on a host as a Mount are appropriate for simple use cases where you need to mount a directory from the host file system into
a container, while volumes are better suited for more complex use cases where you need more control over the data being persisted
in the container.


Docker volumes are a way to persist data even after a container is deleted or recreated. They allow you to store data outside of the container's filesystem, making it easier to manage and share data between containers.

Here are some key aspects of Docker volumes:

- Persistent storage: Volumes provide a way to store data that persists even if the container is deleted or recreated.
- External to the container: Volumes are stored outside of the container's filesystem, making it easier to manage and share data.
- Mapped to a host directory: Volumes are mapped to a directory on the host machine, allowing data to be shared between the container and the host.
- Supported by Docker: Volumes are a built-in feature of Docker, making it easy to use them in your containerized applications.

## Types of Docker Volumes:

- Host volumes: Directly map a directory on the host machine to a directory in the container.
- Anonymous volumes: Automatically created and managed by Docker, these volumes are deleted when the container is deleted.
- Named volumes: User-defined volumes that can be reused across containers.

Benefits of Docker Volumes:

- Data persistence: Volumes ensure that data is not lost when a container is deleted or recreated.
- Data sharing: Volumes allow data to be shared between containers and the host machine.
- Easy data management: Volumes make it easy to manage and backup data.

Here's an example of how to use a Docker volume:

docker run -v /host/data:/container/data my_image

This command maps the /host/data directory on the host machine to the /container/data directory in the container, allowing data to be shared between the two.
