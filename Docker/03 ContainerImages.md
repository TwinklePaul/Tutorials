# What is Container Image?

* When running a container, it uses an isolated filesystem. This custom filesystem is provided by a container image. 

* Since the image contains the container’s filesystem, it must contain everything needed to run an application - all dependencies, configuration, scripts, binaries, etc. The image also contains other configuration for the container, such as environment variables, a default command to run, and other metadata.