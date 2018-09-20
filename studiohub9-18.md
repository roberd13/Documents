# Short Description
DataStax Studio is official tooling for [DataStax Enterprise](https://hub.docker.com/r/datastax/dse-server). DataStax Studio is an interactive tool for CQL (Cassandra Query Language) and DataStax Enterprise Graph. 

# Supported Tags
* 6.0.2, 6.0.1, 6.0.0 ([6.0/Dockerfile](https://github.com/datastax/docker-images/blob/master/studio/6.0/Dockerfile))
* 6.0.0-openjdk8
* 2.0.0 ([2.0/Dockerfile](https://github.com/datastax/docker-images/blob/master/studio/2.0/Dockerfile))
* 2.0.0-openjdk8

# Quick Reference 
### Where to get help:
[DataStax Docker Docs](https://academy.datastax.com/quick-downloads?utm_campaign=Docker_2019&utm_medium=web&utm_source=docker&utm_term=-&utm_content=Web_Academy_Downloads), [DataStax Slack](https://academy.datastax.com/slack), [Github](https://github.com/datastax/docker-images)

Full documentation and advanced tutorials are located within [DataStax Academy](https://academy.datastax.com/quick-downloads?utm_campaign=Docker_2019&utm_medium=web&utm_source=docker&utm_term=-&utm_content=Web_Academy_Downloads)

Featured Tutorial - [DataStax Enterprise 6 Guided Tour](https://academy.datastax.com/resources/guided-tour-dse-6-using-docker)

### Where to file issues:
https://github.com/datastax/docker-images/issues

### Maintained by 
[DataStax](https://www.datastax.com/) 

# What is DataStax Studio

DataStax Studio is an interactive tool for CQL (Cassandra Query Language) and [DataStax Enterprise](https://hub.docker.com/r/datastax/dse-server) Graph. 

![](https://upload.wikimedia.org/wikipedia/commons/e/e5/DataStax_Logo.png)


# Getting Started with DataStax and Docker

DataStax Docker images are licensed only for Development purposes in non-production environments. You can use these images to learn [DSE](https://hub.docker.com/r/datastax/dse-server), [OpsCenter](https://hub.docker.com/r/datastax/dse-opscenter) and [DataStax Studio](https://hub.docker.com/r/datastax/dse-studio), to try new ideas, to test and demonstrate your application.

## Creating a container
Follow these steps to create a DataStax Studio container that is connected to a [DataStax Enterprise (DSE) server](https://hub.docker.com/r/datastax/dse-server) container on the same Docker host.

To create and connect the containers:

1. Create a DataStax Studio container:

```
docker run -e DS_LICENSE=accept --link my-dse --name my-studio -p 9091:9091 -d datastax/dse-studio
```
2. Open a browser and go to `http://DOCKER_HOST_IP:9091`

3. Create the new connection using my-dse as the hostname, see DataStax Studio User Guide > [Creating a new connection](http://docs.datastax.com/en/dse/5.1/dse-dev/datastax_enterprise/studio/stdToc.html) for further instructions.

Studio is ready to use with DSE. See [DataStax Studio User Guide](http://docs.datastax.com/en/dse/5.1/dse-dev/datastax_enterprise/studio/stdAbout.html) for detailed usage and configuration instructions.

## Managing the configuration

Manage the Studio configuration using one of the following options:  

* [DSE configuration volume](https://docs.datastax.com/en/docker/doc/docker/docker60/dockerDSEVolumes.html) For configuration management, we’re providing a simple mechanism to let you provide custom configuration file(s) without customizing the containers or over using host volumes. You can add any of the [approved config files](https://github.com/datastax/docker-images/tree/master/config-templates) to a single mounted host volume and we’ll handle the hard work of mapping them within the container.

* Docker file/directory volume mounts

* Docker overlay file system

* **NOTE** When using memory resource contraints, you must must set JVM heap size using the environment variable `JVM_EXTRA_OPTS` or custom `cassandra-env.sh` or DSE running inside the container due to java not honoring resource limits set for the container. Java utilizes the resources (memory and CPU) of the host. Otherwise DSE will set the heap to 1/4 of the physical ram of the docker host.

## Volumes and data

To persist data, pre-create directories on the local host and map the directory to the corresponding volume using the docker run `-v` flag. 

**NOTE:** If the volumes are not mounted from the local host, all data is lost when the container is removed.

Studio images expose the following volumes.  

* `/var/lib/datastax-studio`

```
docker run -v <local_directory>:<container_volume>
```

See Docker docs > [Use volumes](https://docs.docker.com/engine/tutorials/dockervolumes/#mount-a-host-directory-as-a-data-volume) for more information.

## Opening an interactive bash shell

If the container is running in the background (using the `-d`), use the following command to open an interactive bash shell to run DSE commands. 

```
docker exec -it <container_name> bash
```

To exit the shell without stopping the container type exit.

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
[Studio License Terms](https://www.datastax.com/terms/datastax-studio-license-terms)
[DataStax License Terms](https://www.datastax.com/terms)
