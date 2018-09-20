# Supported Tags
* `6.5.2`, `6.5.1`, `6.5.0` ([6.5.0/Dockerfile](https://github.com/datastax/docker-images/blob/master/opscenter/6.5/Dockerfile))
* `6.5.0-openjdk8`
* `6.1.9`, `6.1.8`,`6.1.7`, `6.1.6`, `6.1.5`, `6.1.4` ([6.1/Dockerfile](https://github.com/datastax/docker-images/blob/master/opscenter/6.1/Dockerfile))
* `6.1.6-openjdk8`, `6.1.5-openjdk8`, `6.1.4-openjdk8`

# Quick Reference 
### Where to get help:
[DataStax Docker Docs](https://academy.datastax.com/quick-downloads?utm_campaign=Docker_2019&utm_medium=web&utm_source=docker&utm_term=-&utm_content=Web_Academy_Downloads), [DataStax Slack](https://academy.datastax.com/slack), [Github](https://github.com/datastax/docker-images)

Full documentation and advanced tutorials are located within [DataStax Academy](https://academy.datastax.com/quick-downloads?utm_campaign=Docker_2019&utm_medium=web&utm_source=docker&utm_term=-&utm_content=Web_Academy_Downloads)

Featured Tutorial - [DataStax Enterprise 6 Guided Tour](https://academy.datastax.com/resources/guided-tour-dse-6-using-docker)

### Where to file issues:
https://github.com/datastax/docker-images/issues

### Maintained by 
[DataStax](https://www.datastax.com/) 

# What is DataStax OpsCenter

OpsCenter is the web-based visual management and monitoring solution for [DataStax Enterprise DSE](https://hub.docker.com/r/datastax/dse-server). 

![](https://upload.wikimedia.org/wikipedia/commons/e/e5/DataStax_Logo.png)


# Getting Started with DataStax and Docker

DataStax Docker images are licensed only for Development purposes in non-production environments. You can use these images to learn [DSE](https://hub.docker.com/r/datastax/dse-server), [OpsCenter](https://hub.docker.com/r/datastax/dse-opscenter) and [DataStax Studio](https://hub.docker.com/r/datastax/dse-studio), to try new ideas, to test and demonstrate your application.

## Creating an Opscenter Container

Follow these steps to create an Opscenter container and a connected DataStax Enterprise server container on the same Docker host. 

To create and connect the containers:

1. First create an OpsCenter container.

```
docker run -e DS_LICENSE=accept -d -p 8888:8888 --name my-opscenter datastax/dse-opscenter
```
See [OpsCenter Docker run options](#OpsCenter-Docker-run-options) for additional options that persist data or manage configuration.

2. Create a [DataStax Enterprise (DSE) server](https://hub.docker.com/r/datastax/dse-server/) container that is linked to the OpsCenter container. 

```
docker run -e DS_LICENSE=accept --link my-opscenter:opscenter --name my-dse -d datastax/dse-server
```

3. Get the DSE container IP address:

On the host running the DSE container run
 
```
docker inspect my-dse | grep '"IPAddress":' 
```

4. Open a browser and go to `http://DOCKER_HOST_IP:8888`.
5. Click `Manage existing cluster`. 
6. In `host name`, enter the DSE IP address.
7. Click `Install agents manually`. Note that the agent is already installed on the DSE image; no installation is required.

OpsCenter is ready to use with DSE. See the [OpsCenter User Guide](http://docs.datastax.com/en/opscenter/6.5/) for detailed usage and configuration instructions.

## Managing the configuration

Manage the Studio configuration using one of the following options:  

* [DSE configuration volume](https://docs.datastax.com/en/docker/doc/docker/docker60/dockerDSEVolumes.html) For configuration management, we’re providing a simple mechanism to let you provide custom configuration file(s) without customizing the containers or over using host volumes. You can add any of the [approved config files](https://github.com/datastax/docker-images/tree/master/config-templates) to a single mounted host volume and we’ll handle the hard work of mapping them within the container.

* Docker file/directory volume mounts

* Docker overlay file system

## Volumes and data

Opscenter metrics are stored in the OpsCenter keyspace in DSE.

To persist a configuration file:

For example to mount the `opscenterd.conf`

1. Create a directory on the Docker host. 

2. Bind mount the local directory to the config file you want to persist by starting the container with the `-v` flag.
```
docker run -e DS_LICENSE=accept -d -v /dse/data/opscenter:/opt/opscenter/conf/opscenterd.conf datastax/dse-opscenter --name my-opscenter
```

## Opening an interactive bash shell

If the container is running in the background (using the `-d`), use the following command to open an interactive bash shell to run DSE commands. 

```
docker exec -it <container_name> bash
```

To exit the shell without stopping the container type exit.

## Viewing logs

You can view the DSE logs using the Docker log command. For example:

```
docker logs my-opscenter
```

# Next Steps

Head over to [DataStax Academy](https://academy.datastax.com/quick-downloads?utm_campaign=Docker_2019&utm_medium=web&utm_source=docker&utm_term=-&utm_content=Web_Academy_Downloads) for advanced documentation including

* Apache Cassandra™/Datastax configuration management
* Using environment variables
* Persisting data
* Exposing public ports
* Volumes and data directories
* Docker Compose examples to spin up connected clusters of DataStax Enterprise, Studio, and Opscenter (also on [github](https://github.com/datastax/docker-images/tree/master/example_compose_yamls))
* Step-by-step tutorials and examples
* How to build applications using Apache Cassandra™/ DataStax

# Licensing
Use the following links to review the license:
[OpsCenter License Terms](https://www.datastax.com/datastax-opscenter-license-terms)
[DataStax License Terms](https://www.datastax.com/terms)
