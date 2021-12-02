# Some basic Docker Commands and Their Options:

## docker run

* The `docker run` command first **creates** a writeable container layer over the specified image, and then **starts** it using the specified command. 

* It is equivalent to the `API/containers/create` then `/containers/(id)/start`.

* A stopped container can be restarted with all its previous changes intact using docker start.

### Some Examples:

#### 1. Simple Image
```
    docker run hello-world
```
* This command runs a local copy(if present) of the image specified.
* If the local copy is absent:
    * The docker client contacts the Docker Daemon and pulls the image(if present) from Docker Hub.
    * The Docker daemon then creates a new container from that image which runs the executable that produces the output on the terminal.

#### 2. Running Image in Interactive mode
```
    docker run -it ubuntu bash

    docker run -d -p 80:80 docker/getting-started
    or
    docker run -dp 80:80 docker/getting-started
```
Here are some info on the flags being use:
* The first command runs `ubuntu` on docker env in an interactive mode, allowing us to use terminal commands.
* `docker/getting-started` - the image to use

### Some Important Flags:

* `-it` option ensures that the container(Ex - `ubuntu`) image is opened in an interactive mode.
* `-d` - run the container in detached mode (in the background).
* `-p 80:80` - map port 80 of the host to port 80 in the container.
* `-rm` - map port 80 of the host to port 80 in the container.
* `--env, -e` - Set environment variables.
* `--env-file` - Read in a file of environment variables.
* `--expose` - Expose a port or a range of ports.
* `--hostname , -h` - Container host name.
* `--init` - Run an init inside the container that forwards signals and reaps processes.
* `--interactive, -i` - Keep STDIN open even if not attached.
* `--ip` - IPv4 address (e.g., 172.30.100.104)
* `--ip6` - IPv6 address (e.g., 2001:db8::33)
* `--pid` - PID namespace to use
* `--pids-limit` - Tune container pids limit (set -1 for unlimited)
* `--privileged` - Give extended privileges to this container
* `--publish , -p` - Publish a container's port(s) to the host
* `--publish-all , -p` - Publish all exposed ports to random ports
* `--pull missing` - Pull image before running `("always"|"missing"|"never")`
* `--read-only` - Mount the container's root filesystem as read only
* `--restart no` - Restart policy to apply when a container exitSysctl optionsRestart policy to apply when a container exits

[Click to see more such flags](https://docs.docker.com/engine/reference/commandline/run/ "Click to see more such flags")
