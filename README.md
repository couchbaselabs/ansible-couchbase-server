# Couchbase Server

               .CCCCC                       CCCCC.
              .CCCCCC                       CCCCCC.
              .CCCCCC                       CCCCCC.
              .CCCCCC                       CCCCCC.
              .CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC.
              .CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC.
              .CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC.
               .CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC.

[Couchbase Server](http://www.couchbase.com/couchbase-server/overview) is a
high performance NoSQL document database available in Community and Enterprise
editions for several supported operating systems.

This Ansible role can be used to install Couchbase Server on cluster nodes,
initialize working clusters, create buckets, and load buckets with test
documents with the playbooks included in the `examples` directory.

## Requirements

This role has been tested for basic functionality with the following 
software:

* Couchbase Server (versions 1.8.1-3.0.3)
* Ansible (version 1.9.0.1)
* CentOS (versions 6.2-6.5)
* Ubuntu (versions 12.04-13.10)

## Role Variables

In cases where you want simple clusters for development or other
non-production use, the default values for the Couchbase Server role's common 
variables can usually be left as-is.

However, should you need specific performance or otherwise wish to tweak them
for your particular purpose, this section describes all of the user editable
variables in detail including their default values for your reference.

### Common Variables

| Name                                 | Default                                                                                   | Description                             |
| ------------------------------------ | ----------------------------------------------------------------------------------------- | --------------------------------------- |
| couchbase_server_edition | enterprise | Couchbase Server edition to install: community or enterprise |
| couchbase_server_admin_port          | 8091                                                                                      | Administration and web console port     |
| couchbase_server_api_port            | 8092                                                                                      | Couchbase Server API port               |
| couchbase_server_internal_ports      | 11209:11211                                                                               | Memcached and client ports              |
| couchbase_server_node_data_ports     | 21100:21299                                                                               | Distributed Erlang communication ports  |
| couchbase_server_config_file         | /opt/couchbase/var/lib/couchbase/config/config.dat                                        | Full path to config.dat                 |
| couchbase_server_data_path           | /opt/couchbase/var/lib/couchbase/data                                                     | Path to data files                      |
| couchbase_server_home_path           | /opt/couchbase                                                                            | Couchbase Server installation base path |
| couchbase_server_index_path          | /opt/couchbase/var/lib/couchbase/data                                                     | Path to index files                     |
| couchbase_server_log_path            | /opt/couchbase/var/lib/couchbase/logs                                                     | Path to log files                       |
| couchbase_server_rhel_pkg_version    | 2.5.1                                                                                     | RHEL package version                    |
| couchbase_server_rhel_pkg_file       | couchbase-server-enterprise_2.5.1_x86_64.rpm                                              | RHEL package filename                   |
| couchbase_server_rhel_pkg_url        | http://packages.couchbase.com/releases/2.5.1/couchbase-server-enterprise_2.5.1_x86_64.rpm | RHEL package URL                        |
| couchbase_server_rhel_pkg_sha256     | 2310a31d177f9396e8c436a991d952b2b57a3b41f74658fa5100b19a1d7ac875                          | RHEL package SHA256 checksum            |
| couchbase_server_ubuntu_ee_pkg_version  | 2.5.1                                                                                     | Ubuntu package version                  |
| couchbase_server_ubuntu_ee_pkg_file     | couchbase-server-enterprise_2.5.1_x86_64.deb                                              | Ubuntu package filename                 |
| couchbase_server_ubuntu_ee_pkg_url      | http://packages.couchbase.com/releases/2.5.1/couchbase-server-enterprise_2.5.1_x86_64.deb | Ubuntu package URL                      |
| couchbase_server_ubuntu_ee_pkg_sha256   | 26c8c990addbd56024fbc5c8e841962b985034f5b7c0e936eb9af94674e5f12a                          | Ubuntu package SHA256 checksum          |


### Special Variables

*The following variables should be set with caution* as they have potential 
negative performance implications; they should not be used without knowledge
of the changes which are made to the operating system configuration:

| Name                                 | Default  | Description                                    |
| ------------------------------------ | -------- | ---------------------------------------------- |
| couchbase_server_tune_disks          | false    | Whether to mount disks with optimized settings |

## Examples

The `examples` directory contains some basic playbooks, host inventory
examples, and Vagrant bits (primarily for Mac OS X development use):

* `cluster_install.yml` prepares OS and installs Couchbase Server only
* `cluster_init.yml` installs Couchbase Server and initializes the cluster
* `create_bucket.yml` creates an example bucket
* `load_bucket.yml` loads sample JSON data into a bucket
* `example_hosts` example hosts inventory in format required by this project
* `Vagrantfile` example Vagrant development cluster definition
* `centos` CentOS hosts inventory for Vagrant based development cluster
* `ubuntu` Ubuntu hosts inventory for Vagrant based development cluster

### Quick Start for Simple Mac OS X Cluster

Follow these steps to have a simple 3 node development or evaluation
cluster on a >= 8GB Mac with OS X, VirtualBox and Vagrant:

1. export ROLEPATH=<ansible_role_path>
2. `ansible-galaxy install couchbase.couchbase-server`
3. cd $ROLEPATH/couchbase.couchbase-server/examples
4. vagrant up

This will install three (3) CentOS 6.5 nodes with 1.5GB RAM each and cluster
them together. The nodes will be available at 10.1.42.10, 10.1.42.20, and
10.1.42.30 as defined in the `Vagrantfile`.

To install Debian based nodes, change the command in step 4 to:

```
BOX_NAME=chef/debian-7.4 CLUSTER_HOSTS=debian vagrant up
```

To install Ubuntu based nodes, change the command in step 4 to:

```
BOX_NAME=ubuntu/trusty64 CLUSTER_HOSTS=ubuntu vagrant up
```
 
## Dependencies

None

## License

Apache

## Author Information

- Brian Shumate (brian at couchbase.com)
