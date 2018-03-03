# Base alpine image with node.js
FROM node:carbon-alpine

# This image provides a Node.JS environment you can use to run your Node.JS
# applications.

EXPOSE 8080

# Add $HOME/node_modules/.bin to the $PATH, allowing user to make npm scripts
# available on the CLI without using npm's --global installation mode
# This image will be initialized with "npm run $NPM_RUN"
# See https://docs.npmjs.com/misc/scripts, and your repo's package.json
# file for possible values of NPM_RUN
# Description
# Environment:
# * $NPM_RUN - Select an alternate / custom runtime mode, defined in your
#   package.json files' scripts section (default: npm run "start").
# Expose ports:
# * 8080 - Unprivileged port used by nodejs application

ENV \
    # Path to be used in other layers to place S2I scripts into
    STI_SCRIPTS_PATH=/usr/libexec/s2i \
    NODEJS_VERSION=8 \
    NPM_RUN=start \
    APP_ROOT=/opt/app-root \
    # The $HOME is not set by default, but some applications needs this variable
    HOME=/opt/app-root/src \
    # NPM_CONFIG_LOGLEVEL=info \
    NPM_CONFIG_PREFIX=$HOME/.npm-global \
    PATH=$APP_ROOT/bin:$HOME/bin:$HOME/node_modules/.bin/:$HOME/.npm-global/bin/:$PATH \
    DEBUG_PORT=5858 \
    NODE_ENV=production \
    DEV_MODE=false

ENV SUMMARY="Platform for building and running Node.js $NODEJS_VERSION applications" \
    DESCRIPTION="Node.js $NODEJS_VERSION available as docker container is a base platform for \
building and running various Node.js $NODEJS_VERSION applications and frameworks. \
Node.js is a platform built on Chrome's JavaScript runtime for easily building \
fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model \
that makes it lightweight and efficient, perfect for data-intensive real-time applications \
that run across distributed devices."

LABEL summary="$SUMMARY" \
      description="$DESCRIPTION" \
      io.k8s.description="$DESCRIPTION" \
      io.k8s.display-name="Node.js $NODEJS_VERSION" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,nodejs,node$NODEJS_VERSION" \
      io.openshift.s2i.scripts-url="image://$STI_SCRIPTS_PATH" \
      io.s2i.scripts-url="image://$STI_SCRIPTS_PATH" \
      com.redhat.dev-mode="DEV_MODE:false" \
      com.redhat.deployments-dir="$HOME" \
      com.redhat.dev-mode.port="DEBUG_PORT:5858" \
      com.redhat.component="alpine-node$NODEJS_VERSION-docker" \
      name="s2i-node-alpine" \
      version="$NODEJS_VERSION" \
      maintainer="SoftwareCollections.org <sclorg@redhat.com>" \
      help="For more information visit https://github.com/sclorg/s2i-nodejs-container" \
      usage="s2i build <SOURCE-REPOSITORY> s2i-node-alpine:$NODEJS_VERSION <APP-NAME>"

# This is the list of basic dependencies that all language Docker image can consume.
RUN mkdir /src && \
    mkdir /opt/usr && \
    mkdir -p $APP_ROOT && \
    mkdir -p $HOME && \
    mkdir -p $HOME/.npm-global && \
    mkdir -p $STI_SCRIPTS_PATH && \
    apk update && \
    apk add findutils \
        gettext \
        bash \
        python \
        make \
        gcc \
        clang \
        g++ \
        linux-headers \
        binutils-gold \
        libstdc++ && \
    rm -rf /var/cache/apk/* && \
    npm install -g nodemon

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

# Copy extra files to the image.
COPY ./root/ /

# When bash is started non-interactively, to run a shell script, for example it
# looks for this variable and source the content of this file. This will enable
# the SCL for all scripts without need to do 'scl enable'.
ENV BASH_ENV=$APP_ROOT/etc/scl_enable \
    ENV=$APP_ROOT/etc/scl_enable \
    PROMPT_COMMAND=". $APP_ROOT/etc/scl_enable"

# Drop the root user and make the content of /opt/app-root owned by user 1001
RUN adduser -u 1001 -S -G root -h ${HOME} -s /sbin/nologin default && \
    chown -R 1001:0 $APP_ROOT && \
    chown -R 1001:0 $STI_SCRIPTS_PATH && \
    chown -R 1001:0 /opt/usr && \
    chown -R 1001:0 /src

# This default user is created in the alpine image
USER 1001

# Directory with the sources is set as the working directory so all STI scripts
# can execute relative to this path.
WORKDIR ${HOME}

# Set the default CMD to print the usage of the language image
CMD $STI_SCRIPTS_PATH/usage
