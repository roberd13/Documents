# Supported Tags
* `6.0.2`, `6.0.1`, `6.0.0` ([6.0/Dockerfile](https://github.com/datastax/docker-images/blob/master/server/6.0/Dockerfile))
* `5.1.10`,  `5.1.9`, `5.1.8`, `5.1.6`, `5.1.5`([5.1/Dockerfile](https://github.com/datastax/docker-images/blob/master/server/5.1/Dockerfile))

# Quick Reference 
### Where to get help:
[DataStax Academy](https://academy.datastax.com/quick-downloads?utm_campaign=Docker_2019&utm_medium=web&utm_source=docker&utm_term=-&utm_content=Web_Academy_Downloads), [DataStax Slack](https://academy.datastax.com/slack)

For documentation and tutorials head over to [DataStax Academy](https://academy.datastax.com/quick-downloads?utm_campaign=Docker_2019&utm_medium=web&utm_source=docker&utm_term=-&utm_content=Web_Academy_Downloads). On Academy you’ll find everything you need to configure and deploy the DataStax Docker Images. 

### Featured Tutorial - [DataStax Enterprise 6 Guided Tour](https://academy.datastax.com/resources/guided-tour-dse-6-using-docker)

### Where to file issues:
https://github.com/datastax/docker-images/issues

### Maintained by 
[DataStax](https://www.datastax.com/) 

# What is DataStax Enterprise

Built on the best distribution of Apache Cassandra™, DataStax Enterprise is the always-on database designed to allow you to effortlessly build and scale your apps, integrating graph, search, analytics, administration, developer tooling, and monitoring into a single unified platform. We power your apps' real-time moments so you can create instant insights and powerful customer experiences.


![](https://upload.wikimedia.org/wikipedia/commons/e/e5/DataStax_Logo.png)

# Creating a DataStax Enterprise container

Use the options described in this section to create DataStax Enterprise server containers. 


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

* **NOTE** When using memory resource contraints, you must must set JVM heap size using the environment variable `JVM_EXTRA_OPTS` or custom `cassandra-env.sh` for DSE running inside the container due to java not honoring resource limits set for the container. Java looks through the container and utilizes the resources (memory and CPU) of the host.



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
`NUM_TOKENS`|*int*|The number of tokens randomly assigned to the node. Default: not set .
`DC` | *string* | Datacenter name. Default: `Cassandra`.
`RACK` | *string* | Rack name. Default: `rack1`.
`OPSCENTER_IP` | *IP_address* \| *string* | Address of OpsCenter instance to use for DSE management; it can be specified via linking the OpsCenter container using opscenter as the name.
`JVM_EXTRA_OPTS` | *string* |  Allows setting custom Heap using -Xmx and -Xms.
`LANG` | *string* |  Allows setting custom Locale
`SNITCH` | *string* |  This variable sets the snitch implementation this node will use. It will set the endpoint_snitch option of cassandra.yml


## Volumes and data

To persist data, pre-create directories on the local host and map the directory to the corresponding volume using the docker run `-v` flag. 

**NOTE:** If the volumes are not mounted from the local host, all data is lost when the container is removed.

DSE images expose the following volumes.  

* For DataStax Enterprise Transactional, Search, Graph, and Analytics workloads:
   * `/var/lib/cassandra`: Data from Cassandra
   * `/var/lib/spark`: Data from DSE Analytics w/ Spark
   * `/var/lib/dsefs`: Data from DSEFS
   * `/var/log/cassandra`: Logs from Cassandra
   * `/var/log/spark`: Logs from Spark

```
docker run -v <local_directory>:<container_volume>
```

See Docker docs > [Use volumes](https://docs.docker.com/engine/tutorials/dockervolumes/#mount-a-host-directory-as-a-data-volume) for more information.


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

# License

Use the following links to review the license:

* [DataStax License Terms](https://www.datastax.com/terms)
