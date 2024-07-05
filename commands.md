# Docker Commands

Some of the most commonly used docker commands are 

### docker images

Lists docker images on the host machine.

### docker build

Builds image from Dockerfile.

### docker run

Runs a Docker container. 

There are many arguments which you can pass to this command for example,

`docker run -d` -> Run container in background and print container ID
`docker run -p` -> Port mapping

use `docker run --help` to look into more arguments.

### docker ps

Lists running containers on the host machine.

### docker stop

Stops running container.

### docker start

Starts a stopped container.

### docker rm

Removes a stopped container.

### docker rmi

Removes an image from the host machine.

### docker pull

Downloads an image from the configured registry.

### docker push

Uploads an image to the configured registry.

### docker exec

Run a command in a running container.

### docker network

Manage Docker networks such as creating and removing networks, and connecting containers to networks.

## Diff between ADD and COPY

# Add a file from your host to your current location.
ADD ./localfile.txt .

# Add files from a remote URL
ADD https://example.com/remote-file.txt .

# Add and automatically unpack a tarball from the host (local.tar.gz) into the current directory.
ADD local.tar.gz .

COPY is straightforward and recommended for copying local files.
ADD has additional features like URL support and tar extraction but should be used cautiously to avoid unexpected behavior (e.g., automatic tar extraction).
For most purposes, especially when dealing with local files, COPY is preferred for its simplicity and clarity.

COPY
Purpose: Primarily intended to copy local files from your host into the Docker image.
Usage: COPY is straightforward; it takes a src and destination. It only supports the basic copying of local files from the context into the Docker image.
Example: COPY ./local/dir /container/dir
Best Practice: Preferred for copying files and directories from the build context into the image because of its simplicity and clarity.
ADD
Purpose: More complex than COPY. It can do everything COPY does, but it also has two additional features.
Additional Features:
URL Support: ADD allows you to specify a URL as the source, and it will download the content of the URL into the destination within the Docker image.
Tar Extraction: If the source is a local tar archive in a recognized compression format (identity, gzip, bzip2, or xz), ADD will automatically unpack it into the destination directory.
Usage: ADD is used when you need to utilize its ability to handle URLs and tar files.
Example: ADD https://example.com/big.tar.xz /usr/src/things/
Best Practice: Use ADD for cases where its unique features are required (e.g., adding remote files or auto-extracting archives). Otherwise, prefer COPY for its simplicity.
Summary
Use COPY for the straightforward copying of files and directories from the Docker context into the image.
Use ADD for more complex scenarios that require downloading remote files or auto-extracting tar archives.

## Entrypoint vs CMD
In Dockerfiles, ENTRYPOINT and CMD instructions both specify what command gets executed when running a container. However, they serve different purposes and are used in conjunction to achieve flexible and robust container execution behaviors.

ENTRYPOINT
Purpose: Sets the executable to be called when the container starts. Essentially, it defines the container's main command, allowing the container to be run as if it were that command or application.
Behavior: The ENTRYPOINT instruction allows you to configure a container that will run as an executable. Command line arguments passed to docker run <image> get appended to the ENTRYPOINT.
Example: ENTRYPOINT ["executable", "param1", "param2"]
Flexibility: You can override the ENTRYPOINT at runtime using the docker run --entrypoint flag.
CMD
Purpose: Provides defaults for an executing container. These defaults can include an executable or they can omit the executable, in which case you must specify an ENTRYPOINT instruction.
Behavior: If used in the shell form, the CMD will be executed in a shell (/bin/sh -c in Linux). If used in the exec form (preferred), it specifies the command and arguments.
Example: CMD ["param1","param2"] (as default parameters to ENTRYPOINT) or CMD ["executable","param1","param2"]
Flexibility: The CMD can be overridden when running the container with additional command line arguments.
Together
When both ENTRYPOINT and CMD are used in a Dockerfile, CMD's values become the default that can be overridden. CMD provides the default arguments to the ENTRYPOINT.
Example: If your Dockerfile contains ENTRYPOINT ["executable"] and CMD ["param1", "param2"], then param1 and param2 are passed as arguments to executable. However, if you run docker run <image> otherparam, otherparam replaces param1 and param2.
Summary
Use ENTRYPOINT when you need your container to run as an executable or service.
Use CMD to provide default arguments to the ENTRYPOINT or if your container can run different commands.
Together, they provide a powerful way to define how your container starts and what default arguments or commands are considered.
