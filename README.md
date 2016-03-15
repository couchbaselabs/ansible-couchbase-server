# Couchbase Server

               .CCCCC                       CCCCC.
              .CCCCCC                       CCCCCC.
              .CCCCCC                       CCCCCC.
              .CCCCCC                       CCCCCC.
              .CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC.
              .CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC.
              .CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC.
               .CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC.

> **NOTE: This role is experimental and not intended for production use**.
> Feel free to use the role for evaluation, development, or testing, and
> borrow the ideas contained within for your own production role or playbooks.
> As with other Couchbase Labs projects, this is primarily for experimentation.

[Couchbase Server](http://www.couchbase.com/couchbase-server/overview) is a
high performance NoSQL document database available in Community and
Enterprise editions supported for common operating systems.

you can use this Ansible role to install Couchbase Server on cluster nodes,
initialize working clusters, create buckets, and load buckets with test
documents with the playbooks included in the `examples` directory.

## 3-Node Development Cluster Quick Start with Vagrant

Follow these steps to deploy a CentOS based, simple 3 node development or
evaluation cluster on a physical host with >= 8GB RAM, using VirtualBox and Vagrant.

### If You Have Previously Used Ansible Galaxy

1. `ansible-galaxy install couchbaselabs.couchbase-server`
2. `cd <roles_path>/examples`
3. `./bin/preinstall.sh`
4. `vagrant up`

### If You Have Not Previously Used Ansible Galaxy

1. `mkdir $HOME/ansible_roles`
2. `echo -e "[defaults]\n\nroles_path = $HOME/ansible_roles" > $HOME/.ansible.cfg`
2. `ansible-galaxy install couchbaselabs.couchbase-server`
3. `cd $HOME/ansible_roles/couchbaselabs.couchbase-server/examples`
4. `./bin/preinstall.sh`
5. `vagrant up`

Note that `$ANSIBLE_ROLES_PATH` defaults to `/etc/ansible/roles` or the path
you've specified in `~/.ansible.cfg` for *roles_path*.

This will install three (3) CentOS 6.5 nodes with 2GB RAM each and cluster
them together. The nodes will be available at 10.1.42.10, 10.1.42.20, and
10.1.42.30 as defined in the `Vagrantfile`.

To install Debian based nodes, change the command in step 4 to:

```
BOX_NAME=chef/debian-7.4 vagrant up
```

To install Ubuntu based nodes, change the command in step 4 to:

```
BOX_NAME=ubuntu/trusty64 vagrant up
```

See `examples/README_VAGRANT.md` for more Vagrant cluster based details.

## Requirements

This role functions with the following software:

* Couchbase Server (versions 3.0.1-4.1.0)
* Ansible (version 1.9.4)
* CentOS (versions 6-7)
* Debian (version 7)
* Ubuntu (versions 12.04-14.04)

This role does not function with the following software:

* CentOS 7.2 — [Bug in CentOS prevents operation of Couchbase Server service](https://bugs.centos.org/view.php?id=9906)

## Works with Ansible Galaxy

You can install this role with the `ansible-galaxy` command, and can run it
directly from the git repository.

You should install it like this:

```
ansible-galaxy install couchbaselabs.couchbase-server
```

Make sure you have write access to `/etc/ansible/roles/` since
that is the default Ansible role installation path, or define your own
Ansible role path by creating a `$HOME/.ansible.cfg` file with these contents:

```
[defaults]
roles_path = <path_to_your_preferred_role_location>
```

Change `<path_to_your_preferred_role_location>` to a directory you have write
access to.

See the [ansible-galaxy](http://docs.ansible.com/galaxy.html) documentation
for more details.

## Role Variables

In cases where you want simple clusters for development or other
non-production use, the values for Couchbase Server role's default variables
are good as-is.

Should you need specific performance or otherwise wish to tweak them
for your particular purpose, this section describes all the role variables
for your reference in detail, including their default values.

### Default Variables

| Name                                 | Default                                                                                   | Description                             |
| ------------------------------------ | ----------------------------------------------------------------------------------------- | --------------------------------------- |
| `couchbase_server_edition` | enterprise | Couchbase Server edition: Community or Enterprise |
| `couchbase_server_admin` | Administrator | Couchbase Server administrator user name |
| `couchbase_server_password` | couchbase |: Couchbase Server administrator user password |
| `couchbase_server_ram` | 3072 | The per server RAM quota specified in megabytes |
| `couchbase_server_admin_port` | 8091 | Administration and web console port |
| `couchbase_server_api_port` | 8092 | Couchbase Server API port |
| `couchbase_server_admin_ssl_port` | 18091 | SSL enabled Couchbase Server REST port |
| `couchbase_server_api_ssl_port` | 18092 | SSL enabled Couchbase Server CAPI port |
| `couchbase_server_internal_ports` | 11209:11211 | Memcached and client ports |
| `couchbase_server_node_data_ports` | 21100:21299 | Distributed Erlang communication ports |
| `couchbase_server_home_path` | /opt/couchbase | Base path to Couchbase Server installation |
| `couchbase_server_bin_path` | /opt/couchbase/bin | Path to Couchbase Server binary utilities |
| `couchbase_server_config_file` | /opt/couchbase/var/lib/couchbase/config/config.dat | Full path to the config.dat file |
| `couchbase_server_filesystem` | EXT4 | Default filesystem for data and index volumes |
| `couchbase_server_mountpoint` | / | Logical volume mountpoint |
| `couchbase_server_partition` | /dev/mapper/VolGroup-lv_root | Logical volume partition |
| `couchbase_server_mount_options` | 'noatime,barrier=0,errors=remount-ro' | Additional mount options |
| `couchbase_server_data_path` | /opt/couchbase/var/lib/couchbase/data | Path to data files |
| `couchbase_server_index_path` | /opt/couchbase/var/lib/couchbase/data | Path to index files |
| `couchbase_server_log_path` | /opt/couchbase/var/lib/couchbase/logs | Path to log files |
| `couchbase_server_cbbackup_path` | /tmp | Path to output of cbbackup on node |
| `couchbase_server_cbcollect_path` | /tmp | Path to cbcollect_info output on node |
| `couchbase_server_tmpdir` | /tmp | System wide TMPDIR for cbcollect_info |
| `couchbase_server_tune_os` | false | Whether to tune OS with optimized settings |
| `couchbase_server_firewall` | false | Whether to use strict firewall rules |

#### Variable Notes

* Other Couchbase Server versions: You can comment out the topmost / current
  version in the `defaults/*.yml` file for your OS and uncomment a previous
  version to have that version installed by default instead. This is also
  possible with specific arguments in the `Vagrantfile` as well.
* couchbase_server_local_package: Place the Couchbase Server package you
  wish to install into the `files` directory and set this to true to skip
  package download. This can greatly speed up the playbook for large clusters
  and is useful for cluster nodes without necessary access to directlyy
  download the package.

### Special Variables

*Configure the following variables with caution* as they have potential
negative performance implications; you should not use them without knowledge
of the changes they make to the operating system configuration:

| Name                                 | Default  | Description                                    |
| ------------------------------------ | -------- | ---------------------------------------------- |
| `couchbase_server_tune_os`          | false    | Whether to tune OS with optimized settings |

## Multi Dimensional Scaling and New Service Types

This `examples/cluster_init.yml` playbook now has basic support for
Couchbase Server version 4.x and its new service types. Specifically, you can
now tag a cluster node as providing the following services:

* data : the standard document data service
* index : the global secondary index (GSI) service
* query : the N1QL query service

To specify which services a node should be responsible for, add them
to the `couchbase_server_node_services` variable as part of your cluster node
host inventory definition.

Reference the *cluster_nodes_mds* section of `examples/example_hosts` for 
an example of service specification.

See [Services architecture and multidimensional scaling](http://developer.couchbase.com/documentation/server/4.0/architecture/services-archi-multi-dimensional-scaling.html) for more information about service types.

## Examples

The `examples` directory contains some basic playbooks, host inventory
examples, and Vagrant bits (primarily for development use)
as follows:

* `cluster_backup.yml` full backup of cluster and retrieval of backup tarball
* `cluster_collect_info.yml` gathers cluster logs with `cbcollect_info`
* `cluster_init.yml` installs Couchbase Server and initializes the cluster
* `cluster_install.yml` prepares OS and installs Couchbase Server
* `create_bucket.yml` creates an example bucket
* `load_bucket.yml` loads sample JSON data into a bucket
* `node_failover.yml` manual failover of cluster node
* `retreive_ssl_cert.yml` retrieve and store node's SSL certificate
* `site.yml` basic role inclusion example
* `example_hosts` example hosts inventory in format required by this project
* `Vagrantfile` example Vagrant development cluster definition
* `vagrant_hosts` default Vagrant hosts inventory file
* `vagrant_hosts_mds` example Vagrant hosts inventory file with MDS

### Create Buckets

The example playbook `create_bucket.yml` for bucket creation works as follows:

Upon first execution without specifying variable arguments via the `ansible-playbook` extra vars ('-e') option, the playbook will generate a
bucket with the following properties:

* Bucket name: *default*
* Bucket type: *couchbase*
* Bucket port: *11211*
* Bucket RAM size: 256MB
* Bucket replica number: 1

If you'd like to create your own buckets, then use the `ansible-playbook`
extra vars ('-e') option and specify values for the
*couchbase_server_bucket_name*, *couchbase_server_bucket_type*, *couchbase_server_bucket_port*, *couchbase_server_bucket_ram*, and *couchbase_server_bucket_replica* variables like so:

```
ansible-playbook -i vagrant_hosts create_bucket.yml \
-e "couchbase_server_bucket_name=danika couchbase_server_bucket_type=couchbase couchbase_server_bucket_port=11223 couchbase_server_bucket_ram=256 couchbase_server_bucket_replica=2"
```

or perhaps you'd like to make a memcached based bucket? No problem:

```
ansible-playbook -i vagrant_hosts create_bucket.yml \
-e "b_name=breandon couchbase_server_bucket_type=memcached couchbase_server_bucket_port=11224 couchbase_server_bucket_ram=512 couchbase_server_bucket_replica=0"
```

## Dependencies

None

## License

Apache

## Author Information

- Brian Shumate (brian at couchbase.com)

## Contributors

Thanks to these people who have contributed to the role:

* [ahamidi](https://github.com/ahamidi)
* [cricket007](https://github.com/cricket007)
* [Jonnymcc](https://github.com/Jonnymcc)
* [Hughjmp](https://github.com/Hughjmp)
* [SomeoneWeird](https://github.com/SomeoneWeird)
* [zabullet](https://github.com/zabullet)
