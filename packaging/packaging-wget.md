How to package wget into an RPM
===============================

Requirements for following along with this document
---------------------------------------------------
    - Docker
    - Basic command line knowledge (how to `cd`, `ls`, `chmod`, ect.)

Getting started
---------------

You can check out the Dockerfile that I've prepped for this exercise however it
is not required for completing the exercise. It will be a centos 7 machine with
all of the tools required for building your own rpm file.

First we need to get the docker container downloaded so that we can begin
working inside it.

    `$ docker run -it demophoon/rpm-packaging-tutorial`

Alternatively you can build the docker image locally and use that instead

    ```shell
    $ docker build --tag demophoon:rpm-packaging-tutorial ./
    $ docker run -it demophoon:rpm-packaging-tutorial
    ```

You should be in the `/root/` directory. In here you will find a file named
`wget-1.19.tar.gz`. This file contains the source code for wget. Untar this file
and move into the directory
