# Dockerized Tor Proxy

The `Dockerfile` that builds a Tor Proxy from offical source code and supports Tor Bridges.

## Features

* Tor, compiled from the source, shorter chain of trust.
* `obfs4` and `meek` Pluggable Transport has been installed out of the box,
  waiting to be enabled by custom setting. Tor Bridges can be enabled by modifying
  `torrc` file.
* Support multi platform (amd64, arm32v7 tested).

## Usage

You need to install Docker in order to use this Image. To do so, please refer to
the official document <https://docs.docker.com/get-started/>

Once Docker is installed, please following these steps to start your Tor Proxy:

### Prepare the Configuration

By default, the Tor Proxy will run at it's default configuration, which may be
indesirable for your situation.

You can find the full configuration at the official web site of the Tor Project,
or here: <https://gitweb.torproject.org/tor.git/tree/src/config/torrc.sample.in>

There are two ways to feed your own configuration.

* Download and modify the configuration to fit your need, then save it as a file, `tor.conf` for this example. Then use ```docker run``` to download and run the Image.

* Modify the `torrc` file to add Tor Bridges or any other configurations you desire. Then build the Image and use ```docker-compose``` to run the Image.

### Download the Image and start running the proxy

Next, ask Docker to download and run the Image. The command is simple as following:

```console
docker run --detach \
--restart=always \
--publish 9050:9050 \
--name tor-proxy \
--env TOR_CUSTOM_CONFIGURATION="$(cat tor.conf)" \
niruix/tor:latest
```

This will download our Image, and load it into a Container named `tor-proxy`.
The Tor Proxy will be safely running inside this Container.

### Build the Image and run the proxy

If you wanted to, you can also build this Image by yourself.

```console
git clone git@github.com:niruix/docker-tor.git
cd docker-tor
```

Then start to build by using command:

```console
docker build -t "torproxy:Dockerfile" .
```

Optionally, if needed, you can set `HTTP_PROXY` environment value for the build
proccess:

```console
docker build --t "torproxy:Dockerfile" --build-arg HTTP_PROXY=http://localhost:8118 .
```

This way, the build script will use the specified proxy to download necessary
files when possible.

Then start the proxy by:

```console
docker-compose up
```

## Support

I don't provide any support on this, unless there is something wrong with my
code or configuration. Please use it at your own risk and help yourself out.
