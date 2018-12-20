## DSE HELM CHART
This chart installs a DSE cluster onto Kubernetes using the official [DataStax Docker Images](https://hub.docker.com/r/datastax/dse-server/)

## Prerequisites

Kubernetes version 1.11+
Persistent Volumes for nodes

## Install Chart with specific DSE Version
By default, this Chart will create a DSE cluster running DSE 6.0.4. If you want to change the DSE version during installation you can use `image.tag={value}` argument or you can edit the `values.yaml`

For example:
Install DSE 6.7.0

```bash
helm install --namespace "dse" -n "dse" --set image.tag=6.7.0 datastax-dse
```

## Install Chart with specific cluster size
By default, this Chart will create a DSE cluster with 5 nodes. If you want to change the cluster size during installation, you can use `--set cassandra.replicas={value}` argument or you can edit the `values.yaml` 

For example:
Set cluster size to 3

```bash
helm install --namespace "dse" -n "dse" --set cassandra.replicas=5 datastax-dse
```



## Configuration

The following table lists the configurable parameters of the DataStax-DSE chart and their default values.

| Parameter                  | Description                                     | Default                                                    |
| -----------------------    | ---------------------------------------------   | ---------------------------------------------------------- |
| `repository`                         | `dse` docker image repository                    | `datastax/dse-server`                                                |
| `image.tag`                          | `dse` docker image tag   | `6.0.4`   |
| `image.args`                   | Image pull policy                          |  `IfNotPresent`    |
| `image.args`                  | Workload to deploy  not set = `cassandra`, analytics = `-k`, search = `-s`, graph =`-g`  | `not set`   |
| `service.ports`                   | Initdb Arguments                                | `9042`                                                     |
| `cassandra.num_tokens`                  | Initdb Arguments                                | `32`                                                      |
| `cassandra.seeds`                   | The number of seed nodes used to bootstrap new clients joining the cluster.                            | `2` |
| `cassandra.replicas`                | The number of nodes in the cluster.             | `3`                                                        |
| `cassandra.cluster_name`                | The name of the cluster.                        | `DSE Cluster`                                                |
| `cassandra.dc`                     | Initdb Arguments                                | `DC1`                                                      |
| `cassandra.rack`                   | Initdb Arguments                                | `RACK1`                                                     |
| `cassandra.concurrent_compactors`             | Initdb Arguments                                | `2`                                             |
| `cassandra.compaction_throughput_mb_per_sec`               | Initdb Arguments                                | `16`                                                    |
| `cassandra. memtable_allocation_type `   | Initdb Arguments                                | ``heap_buffers`                                                     |
| `cassandra.memtable_cleanup_threshold`               | Initdb Arguments                                | `0.40`                                                     |
| `cassandra.memtable_flush_writers`                 | Initdb Arguments              | `2`                                                      |
| `cassandra.memtable_heap_space_in_mb`                   | Initdb Arguments                                | `512`                                                    |
| `cassandra.memtable_offheap_space_in_mb`                    | Initdb Arguments   | `512`                                                       |
| `cassandra.stream_throughput_outbound_megabits_per_sec`                   | Initdb Arguments                 | `200`                                                       |
| `cassandra.inter_dc_stream_throughput_outbound_megabits_per_sec`                      | Initdb Arguments                    | `200`                                                       |
| `cassandra.phi_convict_threshold`                                | Initdb Arguments                             | `8`                                                       |
