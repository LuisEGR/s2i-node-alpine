NodeJS Docker images
====================

[![docker hub stats](http://dockeri.co/image/eduardomourar/s2i-node-alpine)](https://hub.docker.com/r/eduardomourar/s2i-node-alpine/)

This repository contains the source for building various versions of
the Node.JS application as a reproducible Docker image using
[source-to-image](https://github.com/openshift/source-to-image).
The resulting image can be run using [Docker](http://docker.io).

For more information about using these images with OpenShift, please see the
official [OpenShift Documentation](https://docs.openshift.org/latest/using_images/s2i_images/nodejs.html).

For more information about contributing, see
[the Contribution Guidelines](https://github.com/sclorg/welcome/blob/master/contribution.md).
For more information about concepts used in these docker images, see the
[Landing page](https://github.com/sclorg/welcome).


Versions
---------------
Node.JS versions currently provided are:
* [NodeJS 4](4)
* [NodeJS 6](6)
* [NodeJS 8](8)

Linux Alpine versions currently supported are:
* Alpine3

Installation
---------------
To build a Node.JS image:

    This image is available on DockerHub. To download it run:

    ```
    $ docker pull eduardomourar/s2i-node-alpine
    ```

    To build a Node.JS image from scratch run:

    ```
    $ git clone --recursive https://github.com/eduardomourar/s2i-node-alpine
    $ cd s2i-node-alpine
    $ git submodule update --init
    $ make build TARGET=alpine3 VERSIONS=8
    ```

**Notice: By omitting the `VERSIONS` parameter, the build/test action will be performed
on all provided versions of Node.JS.**


Usage
---------------------------------

For information about usage of Dockerfile for NodeJS 4, see [usage documentation](4/README.md).
For information about usage of Dockerfile for NodeJS 6, see [usage documentation](6/README.md).
For information about usage of Dockerfile for NodeJS 8, see [usage documentation](8/README.md).

Test
---------------------
This repository also provides a [S2I](https://github.com/openshift/source-to-image) test framework,
which launches tests to check functionality of a simple Node.JS application built on top of the s2i-nodejs image.

Users can choose between testing a Node.JS test application based on a Alpine image.

    ```
    $ cd s2i-node-alpine
    $ git submodule update --init
    $ make test TARGET=alpine3 VERSIONS=8
    ```

**Notice: By omitting the `VERSIONS` parameter, the build/test action will be performed
on all provided versions of Node.JS.**


Repository organization
------------------------
* **`<nodejs-version>`**

    * **Dockerfile**

        Alpine based Dockerfile.

    * **`s2i/bin/`**

        This folder contains scripts that are run by [S2I](https://github.com/openshift/source-to-image):

        *   **assemble**

            Used to install the sources into the location where the application
            will be run and prepare the application for deployment (eg. installing
            modules using npm, etc.)

        *   **run**

            This script is responsible for running the application, by using the
            application web server.

        *   **usage**

            This script prints the usage of this image.

    * **`contrib/`**

        This folder contains a file with commonly used modules.

    * **`test/`**

        This folder contains the [S2I](https://github.com/openshift/source-to-image)
        test framework with simple Node.JS echo server.

        * **`test-app/`**

            A simple Node.JS echo server used for testing purposes by the [S2I](https://github.com/openshift/source-to-image) test framework.

        * **run**

            This script runs the [S2I](https://github.com/openshift/source-to-image) test framework.
