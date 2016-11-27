# Note

This is a fork from mhart/alpine-node using rpi alpine 

Minimal Node.js Docker Images
---------------------------------------------------------

Versions v7.2.0, v6.9.1.
built on [Alpine Linux](https://alpinelinux.org/).

All versions use the one [fulhack/rpi-alpine-node](https://hub.docker.com/r/fulhack/rpi-alpine-node/) repository,
but each version aligns with the following tags (ie, `fulhack/rpi-alpine-node:<tag>`). The sizes are for the
*unpacked* images as reported by Docker – compressed sizes are about 1/3 of these:

- Full install built with npm:
  - `latest`, `7`, `7.2`, `7.2.0` – 51.07 MB (npm 3.10.9)
  - `6`, `6.9`, `6.9.1` – 46.71 MB (npm 3.10.8)

Major io.js versions [are tagged too](https://hub.docker.com/r/fulhack/rpi-alpine-node/tags/).

Examples
--------

    $ docker run fulhack/rpi-alpine-node node --version
    v7.2.0

    $ docker run fulhack/rpi-alpine-node:6 node --version
    v6.9.1

Example Dockerfile for your own Node.js project
-----------------------------------------------

If you don't have any native dependencies, ie only depend on pure-JS npm
modules, then my suggestion is to run `npm install` locally *before* running
`docker build` (and make sure `node_modules` isn't in your `.dockerignore`) –
then you don't need an `npm install` step in your Dockerfile and you don't need
`npm` installed in your Docker image.

    FROM fulhack/rpi-alpine-node:6

    WORKDIR /src
    ADD . .

    # If you have native dependencies, you'll need extra tools
    # RUN apk add --no-cache make gcc g++ python

    # If you need npm, don't use a base tag
    # RUN npm install

    EXPOSE 3000
    CMD ["node", "index.js"]

Caveats
-------

As Alpine Linux uses musl, you may run into some issues with environments
expecting glibc-like behavior – especially if you try to use binaries compiled
with glibc. You should recompile these binaries to use musl (compiling on
Alpine is probably the easiest way to do this).

Inspired by:

- https://github.com/alpinelinux/aports/blob/454db196/main/nodejs/APKBUILD
- https://github.com/alpinelinux/aports/blob/454db196/main/libuv/APKBUILD
- https://hub.docker.com/r/ficusio/nodejs-base/~/dockerfile/
