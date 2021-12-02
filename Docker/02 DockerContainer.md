# What is a Container?

A container is a sandboxed process on your machine that is isolated from all other processes on the host machine. That isolation leverages kernel namespaces and cgroups, features that have been in Linux for a long time. Docker has worked to make these capabilities approachable and easy to use.

To summarize - A container:
* is a runnable instance of an image. You can create, start, stop, move, or delete a container using the DockerAPI or CLI.

* can be run on local machines, virtual machines or deployed to the cloud.

* is portable (can be run on any OS)

* Containers are isolated from each other and run their own software, binaries, and configurations.

[See how containers are created from scratch.](https://youtu.be/8fi7uSYlOdc "See how containers are created from scratch.")

Use `docker ps -a` to view a list of all containers