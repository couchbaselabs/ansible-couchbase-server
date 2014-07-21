# Couchbase Server

This role can be used to install Couchbase Server on cluster nodes.

You can also initialize clusters, create buckets, and uninstall clusters
with the additional playbooks included in the `examples` directory as well.

## Requirements

This role has been tested for basic functionality with the following 
software versions:

* Couchbase Server versions 1.8.1 - 2.5.1
* CentOS versions 6.2 - 6.5
* Ubuntu versions 12.04 - 13.10

## Role Variables

In most cases, the default values for the Couchbase Server role's common 
variables can be left as is. However, should you need to tweak them for your
particular situation, this section describes all variables in detail including
their default values.

### Common Variables

| Name                                 | Default                                                                                   | Description                             |
| ------------------------------------ | ----------------------------------------------------------------------------------------- | --------------------------------------- |
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
| couchbase_server_ubuntu_pkg_version  | 2.5.1                                                                                     | Ubuntu package version                  |
| couchbase_server_ubuntu_pkg_file     | couchbase-server-enterprise_2.5.1_x86_64.deb                                              | Ubuntu package filename                 |
| couchbase_server_ubuntu_pkg_url      | http://packages.couchbase.com/releases/2.5.1/couchbase-server-enterprise_2.5.1_x86_64.deb | Ubuntu package URL                      |
| couchbase_server_ubuntu_pkg_sha256   | 26c8c990addbd56024fbc5c8e841962b985034f5b7c0e936eb9af94674e5f12a                          | Ubuntu package SHA256 checksum          |


### Special Variables

*The following variables should be set with caution* as they can cause issues,
and should not be used without understanding the changes which are made to
the operating system configuration:

| Name                                 | Default  | Description                                    |
| ------------------------------------ | -------- | ---------------------------------------------- |
| couchbase_server_tune_disks          | false    | Whether to mount disks with optimized settings |

## Dependencies

None

## License

Apache

## Author Information

- Brian Shumate (<brian@couchbase.com>)
