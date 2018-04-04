# Contents
* [DataStax Platform Overview](#datastax-platform-overview)
* [Getting Started with DataStax and Docker](#getting-started-with-datastax-and-docker)
* [Prerequisites](#prerequisites)
* [Creating a DataStax Enterprise Container](#creating-a-datastax-enterprise-container)
   * [Enabling advanced features](#enabling-advanced-functionality)
   * [Container examples](#examples)
   * [Managing the configuration](#managing-the-configuration)
   * [Volumes and Data](#volumes-and-data)
   * [Exposing DSE public ports](#exposing-dse-public-ports)
* [Running DSE Commands and Viewing Logs](#running-dse-commands-and-viewing-logs)
* [Getting Help](#getting-help)
* [Licensing](#license)

![](https://upload.wikimedia.org/wikipedia/commons/e/e5/DataStax_Logo.png)

# DataStax platform overview

Built on the 
best distribution of Apache Cassandra™, DataStax Enterprise is the always-on 
database designed to allow you to effortlessly build and scale your apps, 
integrating graph, search, analytics, administration, developer tooling, 
and monitoring into a single unified platform. We power your 
apps' real-time moments so you can create instant insights 
and powerful customer experiences.

# Getting Started with DataStax and Docker

DataStax Docker images are licensed only for Development purposes in non-production environments. You can use these images to learn DSE, OpsCenter and DataStax Studio, to try new ideas, to test and demonstrate your application.

* Docker Hub:
   * [DataStax Studio](https://hub.docker.com/r/datastax/dse-server/): The best distribution of Apache Cassandra™ with integrated Search, Analytics, Graph, and Advanced Security. 
   * [DataStax Studio](https://hub.docker.com/r/datastax/dse-studio/): An interactive developer’s tool for DataStax Enterprise which is designed to help your DSE database, Cassandra Query Language (CQL), DSE Graph, and Gremlin Query Language development.
   * [DataStax OpsCenter](https://hub.docker.com/r/datastax/dse-opscenter/): The web-based visual management and monitoring solution for DataStax Enterprise (DSE).


# Prerequisites

* Basic understanding of Docker images and containers. 

* Docker installed on your local system, see [Docker Installation Instructions](https://docs.docker.com/engine/installation/). 

* When [building](#building) custom images from the DataStax github repository, a [DataStax Academy account](https://academy.datastax.com/). 

# Creating a DataStax Enterprise container

Use the options described in this section to create DataStax Enterprise server containers. 

## Docker run options
Use the following options to set up a DataStax Enterprise server container. 

Option | Description
------------- | -------------
`-e` | (**Required**) Sets [Environment variables](#using-environment-variables) to accept the licensing agreement and <BR> (optional) change the initial configuration.
`-d` | (Recommended) Starts the container in the background.
`-p` | Publish container ports on the host computer to allow remote access to DSE, OpsCenter, and Studio. See [Exposing DSE public ports](#exposing-public-ports)
`-v` | Bind mount a directory on the local host to a DSE Volume to manage configuration or preserve data. See [Volumes and data](#volumes-and-data). 
`--link` | Link DSE container to OpsCenter or Studio to DSE. For example, `--link my-opscenter:opscenter` or `--link my-dse`.
`--name` |Assigns a name to the container.

These are the most commonly used `docker run` switches used in deploying DSE.  For a full list please see [docker run](https://docs.docker.com/engine/reference/commandline/run/) reference.

## Enabling advanced functionality

By default, the DSE server image is set up as a transactional (database) node.
To set up the node with DSE advanced functionality, add the option that enables feature to the end of the `docker run` command.

Option | Description
------------- | -------------
-s | Enables and starts DSE Search.
-k | Enables and starts Analytics.
-g | Enables and starts a DSE Graph.

Combine startup options to run more than one feature. For more examples, see  [Starting DataStax Enterprise as a stand-alone process ](http://docs.datastax.com/en/dse/6.0/dse-admin/datastax_enterprise/operations/startStop/startDseStandalone.html).

## Examples


### Create a DSE database container


```
docker run -e DS_LICENSE=accept --name my-dse -d datastax/dse-server
```

### Create a DSE container with Graph enabled

```
docker run -e DS_LICENSE=accept --name my-dse -d datastax/dse-server -g
```

### Create a DSE container with Analytics (Spark) enabled

```
docker run -e DS_LICENSE=accept --name my-dse -d datastax/dse-server -k
```

### Create a DSE container with Search enabled

```
docker run -e DS_LICENSE=accept --name my-dse -d datastax/dse-server -s
```

## Managing the configuration

Manage the DSE configuration using one of the following options:  

* [DSE configuration volume](#using-the-dse-conf-volume) use configuration files from a mounted host directory without replacing or customizing the containers. 

* [DSE environment variables](#using-environment-variables) that change the configuration at runtime. 

* Docker file/directory volume mounts

* Docker overlay file system

* DSE uses the default values defined for the environment variables unless explicitly set at run time. **NOTE** Custom config files will override the default or explicitly set environment variables. 

### Using the DSE conf volume

DataStax provided Docker images include a start up script that swaps DSE configuration files found in the Volume `/config` with the configuration file in the default location on the container. 

To use this feature: 

1. Create a directory on your local host. 

2. Add the configuration files you want to replace. Use the following links for full list of configuration files:

     * [DSE](https://github.com/datastax/docker-images/blob/master/server/6.0/files/overwritable-conf-files)

     * [OPSCENTER](https://github.com/datastax/docker-images/blob/master/opscenter/6.5/files/overwritable-conf-files)

     * [STUDIO](https://github.com/datastax/docker-images/blob/master/studio/6.0/files/overwritable-conf-files)

   The file name must match a corresponding configuration file in the image and include all the required values, for example `cassandra.yaml`, `dse.yaml`, `opscenterd.conf`. 

3. Mount the local directory to the exposed Volume `/config`.

4. Start the container. For example to start a transactional node:
 
```
docker run -e DS_LICENSE=accept --name my-dse -d  -v /dse/config:/config datastax/dse-server
```

### Using environment variables


Configure the DSE image by setting environment variables when the container is created using the docker run command `-e` flag.

Variable | Setting        | Description                
------------ | ----------------- | -------------------- 
`DS_LICENSE` | accept | **Required**. Set to accept to acknowledge that you agree with the terms of the DataStax license. To show the license, set the variable `DS_LICENSE` to the value `accept`. *The image only starts if the variable set to accept.*
`LISTEN_ADDRESS` | *IP_address* | The IP address to listen for connections from other nodes. Defaults to the container's IP address.
`BROADCAST_ADDRESS` | *IP_address* | The IP address to advertise to other nodes. Defaults to the same value as the `LISTEN_ADDRESS`.
`NATIVE_TRANSPORT_ADDRESS` |*IP_address* | The IP address to listen for client/driver connections. Default: `0.0.0.0`.
`NATIVE_TRANSPORT_BROADCAST_ADDRESS` | *IP_address* | The IP address to advertise to clients/drivers. Defaults to the same value as the `BROADCAST_ADDRESS`.
`SEEDS` | *IP_address* | The comma-delimited list of seed nodes for the cluster. Defaults to this node's `BROADCAST_ADDRESS`. 
`START_RPC` | `true` \| `false` | Whether or not to start the Thrift RPC server. Will leave the default in the `cassandra.yaml` file if not set.
`CLUSTER_NAME` | *string* | The name of the cluster. Default: `Test Cluster`.
`NUM_TOKENS`|*int*|The number of tokens randomly assigned to the node. Default: `8`.
`DC` | *string* | Datacenter name. Default: `Cassandra`.
`RACK` | *string* | Rack name. Default: `rack1`.
`OPSCENTER_IP` | *IP_address* \| *string* | Address of OpsCenter instance to use for DSE management; it can be specified via linking the OpsCenter container using opscenter as the name.
`JVM_EXTRA_OPTS` | *string* |  Allows setting custom Heap using -Xmx and -Xms.
`LANG` | *string* |  Allows setting custom Locale
`SNITCH` | *string* |  This variable sets the snitch implementation this node will use. It will set the endpoint_snitch option of cassandra.yml


## Volumes and data

DSE images expose the following volumes.  

* For DataStax Enterprise Transactional, Search, Graph, and Analytics workloads:
   * `/var/lib/cassandra`: Data from Cassandra
   * `/var/lib/spark`: Data from DSE Analytics w/ Spark
   * `/var/lib/dsefs`: Data from DSEFS
   * `/var/log/cassandra`: Logs from Cassandra
   * `/var/log/spark`: Logs from Spark
   * `/config`: Directory to add custom config files for the container to pickup.

* For OpsCenter: `/var/lib/opscenter`

* For Studio: `/var/lib/datastax-studio`

### Preserving data
To persist data, pre-create directories on the local host and map the directory to the corresponding volume using the docker run `-v` flag. 

**NOTE:** If the volumes are not mounted from the local host, all data is lost when the container is removed.

* DSE Transactional, Search, Graph, and Analytics nodes:
   * `/var/lib/cassandra/data`  
   * `/var/lib/cassandra/commit_logs` 
   * `/var/lib/cassandra/saved_caches`

* OpsCenter: `/var/lib/opscenter`

* Studio: `/var/lib/datastax-studio`

To mount a volume, use the following syntax:  

```
docker run -v <local_directory>:<container_volume>
```
**Example**
Mount the host directory /dse/config to the DSE volume /config to manage configuration files.
```
docker run -e DS_LICENSE=accept --name my-dse -d  -v /dse/config:/config datastax/dse-server
```

See Docker docs > [Use volumes](https://docs.docker.com/engine/tutorials/dockervolumes/#mount-a-host-directory-as-a-data-volume) for more information.

## Exposing public ports

To allow remote hosts to access a DataStax Enterprise node, OpsCenter, or Studio, map the DSE public port to a host port using the docker run `-p` flag. 

For a complete list of ports see [Securing DataStax Enterprise ports](http://docs.datastax.com/en/dse/6.0/dse-admin/datastax_enterprise/security/secFirewallPorts.html#secFirewallPorts).
 
**NOTE**: When mapping a container port to a local host port ensure that host port is not already in use by another container or the host.
   
### Example

To allow access to the OpsCenter from browser on a remote host, open port `8888`:  
```
docker run -e DS_LICENSE=accept --name my-opscenter -d -p 8888:8888 datastax/dse-opscenter
```

# Running DSE commands and viewing logs


Use the `docker exec -it <container_name>` command to run DSE tools and other operations. 

## Opening an interactive bash shell

If the container is running in the background (using the `-d`), use the following command to open an interactive bash shell to run DSE commands. 

```
docker exec -it <container_name> bash
```

To exit the shell without stopping the container type exit.

## Opening an interactive CQL shell (cqlsh)

Use the following command to open cqlsh. 

```
docker exec -it <container_name> cqlsh
```

To exit the shell without stopping the container use *`ctl P ctl Q`*


## Viewing logs

You can view the DSE logs using the Docker log command. For example:

```
docker logs my-dse
```


## Using DSE tools

Use `docker exec` to run other tools. 

**For example**

`nodetool status` command:

```
docker exec -it my-dse nodetool status
```

See [DSE documentation](http://docs.datastax.com/en/dse/6.0/dse-admin/) for further info on usage/configuration 


# Getting help

File an [issue](https://github.com/datastax/docker-images/issues) or email us at <techpartner@datastax.com>.
  
* [DataStax Enterprise products and features](http://www.datastax.com/products/datastax-enterprise)

* [DataStax Administration Guide](docs.datastax.com/en/dse/6.0/dse-admin/)

   * [Starting DSE](http://docs.datastax.com/en/dse/56.0/dse-admin/datastax_enterprise/operations/startStop/startDseStandalone.html)

   * [Ports](http://docs.datastax.com/en/dse/6.0/dse-admin/datastax_enterprise/security/secFirewallPorts.html)

* [DataStax OpsCenter User Guide](http://docs.datastax.com/en/opscenter/6.5/index.html)

* [DataStax Studio User Guide](http://docs.datastax.com/en/dse/6.0/dse-dev/datastax_enterprise/studio/stdToc.html)

# Known issues

* CFS is not supported
* Changing any file not included in the list of approved configuration files will require an additional host volume or customization of the image. An example is SSL key management.
* You must must set JVM heap size for DSE running inside the container due to java not honoring resource limits set for the container.  Java looks through the container and utilizes the resources (memory and CPU) of the host. 


# License

Use the following links to review the license:

* [DataStax License Terms](https://www.datastax.com/terms)
* [OpsCenter License Terms](https://www.datastax.com/datastax-opscenter-license-terms)
* [Studio License Terms](https://www.datastax.com/terms/datastax-studio-license-terms)

