# Couchbase Server Ansible Role

```
                              #####################                             
                           ###########################                          
                         ###############################                        
                       ###################################                      
                      #####################################                     
                     ######       #############       ######                    
                     ######       #############       ######                    
                     ######                           ######                    
                     ######                           ######                    
                     ######\                         /######                    
                      #####################################                     
                       ###################################                      
                         ###############################                        
                           ###########################                          
                               ###################
```

## Works with Ansible Galaxy

You can install this role with the `ansible-galaxy` command, and can run it
directly from the git repository.

You should install it like this:

```
ansible-galaxy install couchbaselabs.couchbase_server
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
are good as-is.  The only required variable is the [`couchbase_nodes:`](#couchbase_nodes)

Should you need specific performance or otherwise wish to tweak them
for your particular purpose, this section describes all the role variables
for your reference in detail, including their default values.

### couchbase_server_edition

The version of Couchbase Server, this can be `enterprise` or `community`.  The default value is `enterprise`

### couchbase_server_version

The version and build that you want to install, by default the value is `latest`.  If you want to use a specific version, you would would specify `6.6.0-7909`.  To find the available versions run the command:

```bash
yum list --showduplicates couchbase-server
```
### couchbase_server_download_url

A fully qualified URL to an `*.rpm` or `*.deb` file that you wish to install Couchbase Server from.

### couchbase_os

All of the properties in the `couchbase_os` variable are optional, and will be assigned to the default value listed if not specified.

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| firewalld | false | Whether or not to install `firewalld` and add the Couchbase Ports to the the public zone |
|  disable_thp | true  | Whether or not to install the `disable-thp` script.  This will create a new startup script located in `/etc/init.d/disable-thp`  |
| common_tools  | false  |  Whether or not to install common tools, which includes: epel-release, git, jq, ntp, nmap, lshw, sysstat, lvm2, htop, iotop, wireshark, dstat, nmon |
| kernel_tunings  | true  | Whether or not to apply the `sysctl.conf` tunings i.e. `vm.swappiness = 1` This will create a new file located in `/etc/sysctl.d/couchbase-server.conf` |
| user_limits  | true  | Whether or not to set the user limits for the `couchbase` user.  This will create a new file located in `/etc/security/limits.d/couchbase-server.conf`  |

##### Example

```yaml
couchbase_os:
  firewalld: true
  disable_thp: true
  common_tools: true
  kernel_tunings: true
  user_limits: true
```

---

### couchbase_nodes

All of the properties in the `couchbase_nodes ` variable are optional except for the `hostname` property which is required.  If the `services:` property is not specified the value from `couchbase_server.default_services` will be used instead if it is defined, if it is not defined the default is `data,index,query`.  [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-server-add.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
|  \*hostname | *null*  | The hostname of the node to add.  This property is required for any installation/configuration  |
|  group | *null*  | The group name to assign the node to, if not specified the node will be added to the default group.  |
|  services | - data<br>- index<br>- query  | The services to run on the node, valid values are: <br>- data<br> - index<br> - query<br> - fts<br> - eventing<br> - analytics  |

##### Example

```yaml
couchbase_nodes:
  - hostname: host1.couchbase.example.com
    group: AZA
    services:
      - data
  - hostname: host2.couchbase.example.com
    group: AZA
    services:
      - data
  - hostname: host3.couchbase.example.com
    group: AZB
    services:
      - data
  - hostname: host4.couchbase.example.com
    group: AZB
    services:
      - data
  - hostname: host5.couchbase.example.com
    group: AZA
    services:
      - index
      - query
  - hostname: host6.couchbase.example.com
    group: AZB
    services:
      - index
      - query
```

---

### couchbase_cluster

All of the properties in the `couchbase_cluster ` variable are optional, and will be assigned to the default value listed if not specified.   [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-cluster-init.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| name  | My Cluster  | The name of the Couchbase Cluster  |
|  rest_protocol | http  | The REST protocol to use  |
| port  | 8091  | The default port to use for the cluster  |
| notifications  | true  | Whether or not console notifications should be enabled  |
|  index_storage |  default | Specifies the index storage mode for the index service. Accepted storage modes are "default" for the standard index backend or memopt for memory optimized indexes.  |
| default_services  | - data<br>- index<br>- query | The default services to use when initializing the cluster or adding a new node to the cluster  |

##### Example

```yaml
couchbase_cluster:
  name: Demo
  rest_protocol: http
  port: 8091
  notifications: true
  index_storage: default
  default_services:
    - data
    - index
    - query
```

---

### couchbase\_memory\_quotas

All of the properties in the `couchbase_memory_quotas ` variable are optional, and will be assigned to the default value listed if not specified.  [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-cluster-init.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| analytics  |  1024 | Sets the Analytics service memory quota (in MB).  This quota will be assigned to all future nodes added to the cluster with the analytics service.  |
| data  | 4098  |  Specifies the data services memory quota (in MB). This quota will be assigned to all future nodes added to the cluster with the data service. |
| eventing  | 256  | Sets the Eventing service memory quota (in MB). This quota will be assigned to all future nodes added to the cluster with the eventing service.  |
| fts  | 512  |  Sets the full-text service memory quota (in MB).  This quota will be assigned to all future nodes added to the cluster with the full-text service. |
| index  | 512  | Sets the index service memory quota (in MB).  This quota will be assigned to all future nodes added to the cluster with the index service.  |

##### Example

```yaml
couchbase_memory_quotas:
  analytics: 1024
  data: 16000
  eventing: 256
  fts: 512
  index: 512
```

---

### couchbase_security

All of the properties in the `couchbase_security ` variable are optional, and will be assigned to the default value listed if not specified.  [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-setting-security.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| admin_user  | Administrator  |  The cluster Administrators username |
| admin_password | password  |  The cluster Administrators password |
| disable\_http\_ui  |  false | Specifies whether the Couchbase Web Console can be accessible over http.   |
| disable\_www\_authenticate  | false  |  Specifies whether Couchbase Server will respond with WWW-Authenticate to unauthenticated requests. |
| cluster\_encryption\_level  |  control |  Specifies the cluster encryption level. The level is used when cluster encryption is turned on. If the level is "all" then both data and control messages between the nodes in the cluster will be sent over encrypted connections. If the level is "control" only control messages will be sent encrypted.  |
| tls\_min\_version  | tlsv1  | Specify the minimum TLS protocol version to be used across all of the Couchbase services. |
| tls\_honor\_cipher\_order  |  true |  Specifies whether the cipher-order must be followed across all of the services. When set to true, this allows weaker ciphers to be included in the cipher list for backwards compatibility of older clients/browsers while still forcing the newer clients to use stronger ciphers. |

##### Example

```yaml
couchbase_security:
  admin_user: Administrator
  admin_password: password
  disable_http_ui: false
  disable_www_authenticate: false
  cluster_encryption_level: control
  tls_min_version: tlsv1
  tls_honor_cipher_order: true
```

---

### couchbase_paths

All of the properties in the `couchbase_paths ` variable are optional, and will be assigned to the default value listed if not specified.  [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-node-init.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| data  |  /opt/couchbase/var/lib/couchbase/data | The path to store data files create by the Couchbase data service. Note that this path is also where view indexes are written on this server.   |
| index  |  /opt/couchbase/var/lib/couchbase/data | The path to store files create by the Couchbase index service.   |
| analytics  | /opt/couchbase/var/lib/couchbase/data  | The path to store files create by the Couchbase Analytics service.  |
| eventing  |  /opt/couchbase/var/lib/couchbase/data | The path to store files create by the Couchbase Eventing service.  |

##### Example

```yaml
couchbase_paths:
  data: /opt/couchbase/var/lib/couchbase/data
  index: /opt/couchbase/var/lib/couchbase/index
  analytics: /opt/couchbase/var/lib/couchbase/analytics
  eventing: /opt/couchbase/var/lib/couchbase/eventing
```

---

### couchbase\_rebalance\_settings

All of the properties in the `couchbase_rebalance_settings ` variable are optional, and will be assigned to the default value listed if not specified.  [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-setting-rebalance.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| rebalance_retry  | false  |  Enable or disable automatic rebalance retry  |
|  wait_for | 300  | Specify the amount of time to wait after a failed rebalance before retrying. Time must be a value between 5 and 3600 seconds  |
| max\_attempts  |  1 | Specify the number of times a failed rebalance will be retried. The value provided must be between 1 and 3. |
| moves\_per\_node  | 4  | Specify the number ofconcurrent vBucket to move per a node during a rebalance. The value provided must be between 1 and 64. A higher setting may improve rebalance performance, at the cost of higher resource consumption; in terms of CPU, memory, disk, and bandwidth. Conversely, a lower setting may degrade rebalance performance, while freeing up such resources. Note, however, that rebalance performance can be affected by many additional factors; and that in consequence, changing this parameter may not always have the expected effects. Note also that a higher setting, due to its additional consumption of resources, may degrade the performance of other systems, including the Data Service.  |

##### Example

```yaml
couchbase_rebalance_settings:
  rebalance_retry: false
  wait_for: 300
  max_attempts: 1
  moves_per_node: 4
```

---

### couchbase_audit

All of the properties in the `couchbase_audit ` variable are optional, and will be assigned to the default value listed if not specified.  [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-setting-audit.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| enabled  | true  |  Specifies whether or not auditing is enabled. |
|  log_path | /opt/couchbase/var/lib/couchbase/logs  | Specifies the auditing log path. This should be a path to a folder where the auditing log is kept. The folder must exist on all servers in the cluster.  |
|  log\_rotate\_interval |  86400 |  Specifies the audit log rotate interval. This is the interval in which the current audit log will be replaced with a new empty audit log file. The log file is rotated to ensure that the audit log does not consume too much disk space. The minimum audit log rotate interval is 15 minutes (900 seconds). |
| log\_rotate\_size  | 20971520  | Specifies the audit log rotate size. This is the size at which the current audit log will be replaced with a new empty audit log file. The log file is rotated to ensure that the audit log does not consume too much disk space. The minimum audit log rotate size is 0 bytes and maximum is 524,288,000 (500MB). When it is set to 0 the log will not be rotated based on size.  |

##### Example

```yaml
couchbase_audit:
  enabled: true
  log_path: /opt/couchbase/var/lib/couchbase/logs
  log_rotate_interval: 86400
  log_rotate_size: 20971520
```

---

### couchbase\_password\_policy

All of the properties in the `couchbase_password_policy ` variable are optional, and will be assigned to the default value listed if not specified.  [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-setting-password-policy.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| min_length  |  6 | Specifies the minimum password length for new passwords.  |
| uppercase  |  false | Specifies that new passwords must contain at least one upper case letter.  |
| lowercase  | false  | Specifies that new passwords must contain at least one lower case letter.  |
| digit  |  false | Specifies that new passwords must contain at least one digit.  |
| special_char  | false  |  Specifies that new passwords must contain at least one special character. |

##### Example

```yaml
couchbase_password_policy:
  min_length: 6
  uppercase: false
  lowercase: false
  digit: false
  special_char: false
```

---

### couchbase\_query\_settings

All of the properties in the `couchbase_query_settings ` variable are optional, and will be assigned to the default value listed if not specified.  [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-setting-query.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| pipeline_batch  | 16  |  Number of items execution operators can batch |
| pipeline_cap  |  512 | Maximum number of items each execution operator can buffer  |
| scan_cap  | 512  | Maximum buffer size for index scans; use zero or negative value to disable  |
| timeout  |  0 | Server execution timeout; use zero or negative value to disable  |
| prepared_limit  |  16384 | Maximum number of prepared statements  |
| completed_limit  | 4000  | Maximum number of completed requests  |
| completed_threshold  | 1000  | Cache completed queries lasting longer than this threshold (in milliseconds)  |
| log_level  | info  | Sets the log level for the query service.  Valid log levels include "trace", "debug", "info", "warn", "error", "server" and "none"  |
| max_parallelism  |  1 | Maximum parallelism per query; use zero or negative value to disable  |

##### Example

```yaml
couchbase_query_settings:
  pipeline_batch: 16
  pipeline_cap: 512
  scan_cap: 512
  timeout: 0
  prepared_limit: 16384
  completed_limit: 4000
  completed_threshold: 1000
  log_level: info
  max_parallelism: 1
```

---

### couchbase\_index\_settings

All of the properties in the `couchbase_index_settings ` variable are optional, and will be assigned to the default value listed if not specified.  [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-setting-index.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| max_rollback_points  | 2  | The maximum number of rollback points. The value of this option must be greater than or equal to 1.  |
| stable_snapshot_interval  |  5000 | Specifies the frequency of persisted snapshots for recovery in seconds. This means that in the event of a failover this is the farthest back we may have to rebuild the index from. This value of this parameter must be greater than 1.  |
| memory_snapshot_interval  |  200 | Specifies the frequency of in-memory snapshots in milliseconds. This determines the earliest possibility of a scan seeing a given KV mutation. This value of this parameter must be greater than 1.  |
| threads  | 0  | Sets the number of CPUs that can be used by the indexer. The value of this option must be between 0 and 1024.  |
| log_level  |  info |  Sets the log level for the index service. Valid log levels include "debug", "silent", "fatal", "error", "warn", "info", "verbose", "timing", and "trace" |

##### Example

```yaml
couchbase_index_settings:
  max_rollback_points: 2
  stable_snapshot_interval: 5000
  memory_snapshot_interval: 200
  threads: 0
  log_level: info
```

---

### couchbase_autofailover

All of the properties in the `couchbase_autofailover ` variable are optional, and will be assigned to the default value listed if not specified.  [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-setting-autofailover.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| enabled  | true  |   |
| failover_timeout  | 120  | Specifies the auto-failover timeout. This is the amount of time a node must be unresponsive before the cluster manager considers the node to be down and fails it over. The minimum auto-failover timeout is 30 seconds in Couchbase Community Edition and 5 seconds in Couchbase Enterprise Edition  |
| max_failovers  | 1  | Specifies the number of auto-failover events that will be handled before requiring user intervention. A single event could be one node failing over or an entire Server Group. The maximum allowed value is three. This feature is only available in Couchbase Enterprise Edition.  |
| failover\_of\_server\_groups  | false  |  Specifies whether or not auto-failover can failover entire Server Groups. This feature is only available in Couchbase Enterprise Edition. |
| failover\_on\_data\_disk\_issues  | true  | Specifies whether or not autofailover on Data Service disk issues is enabled. Set this option to "1" to enable failover on Data Service disk issue or "0" to disable it. "--failover-data-disk-period" needs to be set at the same time when enabling this option. This feature is only available in Couchbase Enterprise Edition.  |
| failover\_data\_disk\_period  | 120  |  Specifies the failover data disk period. This is the period of time over which the Data Service is checked for potential sustained Disk I/O failures. The Data Service is checked every second for disk failures. If 60% of the checks during that time period report disk failures, then the node may be automatically failed over. "--enable-failover-on-data-disk-issues" must be set when this option is used. The failover data disk period ranges from 5 to 3600 seconds. |
| can\_abort\_rebalance  | true  | Enables auto-failover to abort rebalance and perform auto-failover. This feature is only available in Couchbase Enterprise Edition.  |

##### Example

```yaml
couchbase_autofailover:
  enabled: true
  failover_timeout: 120
  max_failovers: 1
  failover_of_server_groups: false
  failover_on_data_disk_issues: true
  failover_data_disk_period: 120
  can_abort_rebalance: true
```

---

### couchbase\_email\_alerts

All of the properties in the `couchbase_email_alerts ` variable are optional, and will be assigned to the default value listed if not specified.  [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-setting-alert.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| enabled  | false  | Enables email alerts on this cluster.   |
| username  |  *null* | The email server username for the sender email address. This field is required if the email address provided requires authentication.  |
| password  |  *null*   |  The email server password for the sender email address. This field is required if the email address provided requires authentication. |
| host  | localhost  | The email server hostname that hosts the email address specified by the sender  |
|  port  | 25  | The email server port number of the server that hosts the email address specified by the sender option.  |
| encrypt | false  | Enables SSL encryption when connecting to the email server.  |
| sender  | couchbase@localhost  | If email alerts an enabled then this option will set the sender email address.  |
| recipients  | []  | A list of users to email when an alert is raised in the server.  |
|  alerts | - alert-auto-failover-node<br>- alert-auto-failover-max-reached<br>- alert-auto-failover-node-down<br>- alert-auto-failover-cluster-small<br>- alert-auto-failover-disable<br>- alert-ip-changed<br>- alert-disk-space<br>- alert-meta-overhead<br>- alert-meta-oom<br>- alert-write-failed<br>- alert-audit-msg-dropped<br>- alert-indexer-max-ram<br>- alert-timestamp-drift-exceeded<br>- alert-communication-issue  |  - **alert-auto-failover-node**: Specifies that an email alert should be sent when a node is automatically failed over.<br>- **alert-auto-failover-max-reached**: Specifies that an email alert should be sent when the maximum amount of auto-failovers is reached.<br>- **alert-auto-failover-node-down**: Specifies that an email alert should be sent when auto-failover could not be completed because another node in the cluster was already down.<br>- **alert-auto-failover-cluster-small**: Specifies that an email alert should be sent when auto-failover could not be completed because the cluster is too small.<br>- **alert-auto-failover-disable**: Specifies that an email alert should be sent when auto-failover could not be completed because auto-failover is disabled on this cluster.<br>- **alert-ip-changed**: Specifies that an email alert should be sent when the IP address on a node in the cluster changes.<br>- **alert-disk-space**: Specifies that an email alert should be sent when the disk usage on a node in the cluster reaches 90% of the available disk space.<br>- **alert-meta-overhead**: Specifies that an email alert should be sent when the metadata overhead on the data service is more than 50%.<br>- **alert-meta-oom**: Specifies that an email alert should be sent when all of the memory in the cache for a bucket is used by metadata. If this condition is hit the bucket will be unusable until more memory is added to the bucket cache.<br>- **alert-write-failed**: Specifies that an email alert should be sent when writing data to disk on the data service has failed.<br>- **alert-audit-msg-dropped**: Specifies that an email alert should be sent when writing event to audit log fails.<br>- **alert-indexer-max-ram**: Specifies that an email alert should be sent when the memory usage for the indexer service on a specific node exceeds the per node memory usage limit. This warning is only shown for if the index storage type is Memory Optimized Indexes (MOI).<br>- **alert-timestamp-drift-exceeded**: Specifies that an email alert should be sent if the clocks on two different machines in the cluster are more that five seconds apart.<br>- **alert-communication-issue**: Specifies that an email alert should be sent when nodes are experiencing communication issues.<br><br> |

##### Example

```yaml
couchbase_email_alerts:
  enabled: true
  host: localhost
  port: 25
  encrypt: false
  sender: couchbase@localhost
  recipients:
    - couchbase-admin@example.com
  alerts:
    - alert-auto-failover-node
    - alert-auto-failover-max-reached
    - alert-auto-failover-node-down
    - alert-auto-failover-cluster-small
    - alert-auto-failover-disable
    - alert-ip-changed
    - alert-disk-space
    - alert-meta-overhead
    - alert-meta-oom
    - alert-write-failed
    - alert-audit-msg-dropped
    - alert-indexer-max-ram
    - alert-timestamp-drift-exceeded
    - alert-communication-issue
```

---

### couchbase_buckets[]

The `couchbase_buckets` variable is an empty list by default.  When specified the following properties are available to each item in the list, the `name` property is required, all other properties are optional and will be assigned to the default value listed if not specified.  [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-bucket-create.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| \*name  |  *null* | The name of the bucket to create. The only characters that can be used for the bucket-name are those in the ranges of A-Z, a-z, and 0-9; plus the underscore, period, dash, and percent characters. The name can be a maximum of 100 characters in length.  |
| type  | couchbase  | The type of bucket to create. Accepted bucket types are "couchbase", "ephemeral", and "memcached". The Couchbase bucket is the standard bucket type. It supports data persistence, replication, caching, indexing, views, and N1QL queries. The Ephemeral bucket is an in-memory bucket similar to the Couchbase bucket; but it does not support data persistence or views.   The Memcached bucket is a cache-only bucket that does not support persistence, replication, indexing, views, or N1QL querying: this bucket type provides the same behavior as Memcached Server. Memcached buckets are deprecated and Ephemeral buckets should be used instead.  |
| ram_size  | 100  | The amount of memory to allocate to the cache for this bucket, in Megabytes. The memory quota of this bucket must fit into the overall cluster memory quota. The minimum cache size is 100MB.  |
| replicas  | 1  |  The number of servers to which data is replicated. Replicas provide protection against data loss by keeping copies of this bucket’s data on multiple servers. By default, the number of replicas is one, even if there is only a single server in the cluster. The minimum number of replicas is zero, and the maximum three. This option is valid for Couchbase and Ephemeral buckets only.  |
|  priority | low  | Specifies the priority of this bucket’s background tasks. This option is valid for Couchbase and Ephemeral buckets only. For Couchbase buckets, background task-types include disk I/O, DCP stream-processing, and item-paging. For Ephemeral buckets, background task-types are the same as for Couchbase buckets, with the exception of disk I/O, which does not apply to Ephemeral buckets. The value of this option may be "high" or "low". The default is "low". Specifying "high" may result in faster processing; but only when more than one bucket is defined for the cluster, and when different priority settings have been established among the buckets. When Couchbase and Ephemeral buckets have different priority settings, this affects the prioritization only of task-types other than disk I/O.  |
| eviction_policy  |  valueOnly | The memory-cache eviction policy for this bucket. This option is valid for Couchbase and Ephemeral buckets only.<br><br>Couchbase buckets support either "valueOnly" or "fullEviction". Specifying the "valueOnly" policy means that each key stored in this bucket must be kept in memory. This is the default policy: using this policy improves performance of key-value operations, but limits the maximum size of the bucket. Specifying the "fullEviction" policy means that performance is impacted for key-value operations, but the maximum size of the bucket is unbounded.<br><br>Ephemeral buckets support either "noEviction" or "nruEviction". Specifying "noEviction" means that the bucket will not evict items from the cache if the cache is full: this type of eviction policy should be used for in-memory database use-cases. Specifying "nruEviction" means that items not recently used will be evicted from memory, when all memory in the bucket is used: this type of eviction policy should be used for caching use-cases.  |
|  conflict_resolution | sequence  | Specifies this bucket’s conflict resolution mechanism; which is to be used if a conflict occurs during Cross Data-Center Replication (XDCR). Sequence-based and timestamp-based mechanisms are supported.<br><br>Sequence-based conflict resolution selects the document that has been updated the greatest number of times since the last sync: for example, if one cluster has updated a document twice since the last sync, and the other cluster has updated the document three times, the document updated three times is selected; regardless of the specific times at which updates took place.<br><br>Timestamp-based conflict resolution selects the document with the most recent timestamp: this is only supported when all clocks on all cluster-nodes have been fully synchronized.  |
| flush  | false  | Specifies whether or not the flush operation is allowed for this bucket.   |
| durability\_min\_level  |  none | The minimum durability level for the bucket. Accepted values for "ephemeral" buckets are "none" or "majority". Accepted values for "couchbase" buckets are "none", "majority", "majorityAndPersistActive", or "persistToMajority".<br><br>"none" specifies mutations to the bucket are asynchronous and offer no durability guarantees. "majority" specifies mutations must be replicated to (that is, held in the memory allocated to the bucket on) a majority of the Data Service nodes. "majorityAndPersistActive" specifies mutations must be replicated to a majority of the Data Service nodes. Additionally, it must be written to disk on the node hosting the active vBucket for the data. "persistToMajority" specifies mutations must be persisted to a majority of the Data Service nodes. Accordingly, it will be written to disk on those nodes.  |
|  compression_mode | passive  | Specifies the compression-mode of the bucket. There are three options; off, passive and active. All three modes are backwards compatible with older SDKs, where Couchbase Server will automatically uncompress documents for clients that do not understand the compression being used. This option is only available for Couchbase and Ephemeral buckets in Couchbase Enterprise Edition.<br><br>Off: Couchbase Server will only compress document values when persisting to disk. This is suitable for use cases where compression could have a negative impact on performance. Please note it is expected that compression in most use cases will improve performance.<br><br>Passive: Documents which were compressed by a client, or read compressed from disk will be stored compress in-memory. Couchbase Server will make no additional attempt to compress documents that are not already compressed.<br><br>Active: Couchbase Server will actively and aggressively attempt to compress documents, even if they have not been received in a compressed format. This provides the benefits of compression even when the SDK clients are not complicit.  |
|  max_ttl | 0  | Specifies the maximum TTL (time-to-live) for all documents in bucket in seconds. If enabled and a document is mutated with no TTL or a TTL greater than than the maximum, its TTL will be set to the maximum TTL. Setting this option to 0 disables the use of max-TTL and the largest TTL that is allowed is 2147483647. This option is only available for Couchbase and Ephemeral buckets in Couchbase Enterprise Edition.  |
|  enable\_index\_replica | false  | Enables view index replication for the current bucket. This option is valid for Couchbase buckets only.  |

##### Example

```yaml
couchbase_buckets:
  - name: baseball
    type: couchbase
    ram_size: 400
    replicas: 1
    compression_mode: active
  - name: ecommerce
    type: couchbase
    ram_size: 700
    replicas: 1
    compression_mode: active
  - name: movies
    type: couchbase
    ram_size: 450
    replicas: 1
    compression_mode: active
```

---

### couchbase\_sample\_buckets[]

The `couchbase_sample_buckets` variable is an empty list by default.  Only the following values are supported: "travel-sample", "beer-sample" or "gamesim-sample"

##### Example

```yaml
couchbase_sample_buckets:
  - travel-sample
```

---

### couchbase\_xdcr\_remotes[]

The `couchbase_xdcr_remotes` variable is an empty list by default.  When specified the following properties are available to each item in the list, each of the 4 properties are required when specifying an XDCR Remote.  [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-xdcr-setup.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| \*name  |  *null* | The name for the remote cluster reference.  |
| \*hostname  | *null*  | The hostname of the remote cluster reference.  |
| \*username  | *null*  | The username of the remote cluster reference.  |
| \*password  | *null*  | The password of the remote cluster reference.  |

##### Example

```yaml
couchbase_xdcr_remotes:
  - name: My Cluster
    hostname: remotehost.couchbase.example.com
    username: Administrator
    password: password
```

---

### couchbase\_xdcr\_replications[]

The `couchbase_xdcr_replications` variable is an empty list by default.  When specified the following properties are available to each item in the list, the `from_bucket`, `to_bucket` and `cluster_name` properties are required, all other properties are optional and will be assigned to the default value listed if not specified.  [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-xdcr-replicate.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| \*from\_bucket | *null*   |  The name bucket to replicate data from. |
| \*to\_bucket  | *null*   | The name bucket to replicate data to.  |
| \*cluster\_name  | *null*   | The name of the cluster reference to replicate to.  |
| filter\_expression  | *null*  |  A regular expression used to filter the replication stream. |
|  checkpoint\_interval | 600  | The interval between checkpoints in seconds. The value of this option must be between 60 and 14,400.  |
| worker\_batch\_size  | 500  |  The worker batch size. The value of this option must be between 500 and 10,000. |
|  doc\_batch\_size | 2048  | The document batch size in Kilobytes. The value of this option must be between 10 and 100,000.  |
|  failure\_restart\_interval | 10  | Interval for restarting failed XDCR connections in seconds. The value of this option must be between 1 and 300.  |
| optimistic\_replication\_threshold  |  256 |  Document body size threshold in bytes used to trigger optimistic replication. |
| source\_nozzle\_per\_node  | 2  |  The number of source nozzles to each node in the target cluster. The value of this option must be between 1 and 10. |
| target\_nozzle\_per\_node  | 2  | The number of outgoing nozzles to each node in the target cluster. The value of this option must be between 1 and 10.  |
| bandwidth\_usage\_limit  | 0  | The bandwidth limit for XDCR replications in Megabytes per second for this replication.  |
| enable\_compression  | true | Specifies whether or not XDCR compression is enabled. This feature is only available in Couchbase Enterprise Edition and can only be used where the target cluster supports compression.  |
| log\_level  | Info  | The XDCR log level. The following values are supported: "Error", "Info", "Debug", "Trace"  |
| stats\_interval  | 1000  | The interval for statistics updates in milliseconds.  |
| priority  |  High | Specify the priority for the replication.  The options are "High", "Medium" or "Low"  |
| reset\_expiry  | false |  When set to true, all mutations sent to the target cluster will have the expiration set to zero. This means documents will not expire in target cluster. This can be overridden by setting max-ttl on the target bucket. |
| filter\_deletion  |  false |  When set to true, delete mutations will not be sent to the target cluster. This means documents will not be deleted in the target cluster via delete operations on the source cluster. |
| filter\_expiration  | false  | When set to true, expiry mutations will not be sent to the target cluster. This means documents will not be deleted in the target cluster via expirations on the source cluster.  |

##### Example

```yaml
couchbase_xdcr_replications:
  - from_bucket: beer
    to_bucket: demo
    cluster_name: My Cluster
```

---

### couchbase_ldap

All of the properties in the `couchbase_ldap ` variable are optional, and will be assigned to the default value listed if not specified.  [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-setting-ldap.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| hosts  | []  | List of LDAP hosts.  If this is empty, LDAP is not configured  |
|  port  | 389  |  LDAP port. |
|  encryption | none  | Security used to communicate with LDAP servers. Supported options are: tls, startTLS, none.  |
| cacert  |  cacert |  Path to CA certificate to be used for server’s certificate validation, required if certificate validation is not disabled |
| server\_cert\_validation  | false  |  Enable or disable certificate validation when connecting to LDAP server. |
|  bind\_dn | *null*  | The DN of a user to authenticate as to allow user search and groups synchronization. If bind details or client TLS certificate are not provided anonymous bind will be used instead.  |
| bind\_password  | *null*  |  The bind user password |
| client\_cert  | *null*  | The client TLS certificate to be used to bind against the LDAP Server to allow user and groups synchronization. If bind details or client TLS certificate are not provided anonymous bind will be used instead. '--client-cert' and '--client-key' need to be set together.  |
| client\_key  | *null*  | The client TLS key. This is used with '--client-cert' flag for certificate authentication. '--client-cert' and '--client-key' need to be set together.  |
| authentication\_enabled  | false  | Enables using LDAP to authenticate users  |
|  user\_dn\_query | *null*  | LDAP query to get user’s DN. Must contains at least one instance of %u Example: ou=Users,dc=example??one?(uid=%u)  |
| authorization\_enabled  | false  | Enables using LDAP to give users authorization  |
| group\_query  | *null*  |  LDAP query, to get the users' groups by username in RFC4516 format. The %u and %D placeholders can be used, for username and user’s DN respectively. If attribute is present in the query, the list of attribute values in the search result is considered as list of user’s groups (single entry result is expected): for example: '%D?memberOf?base'. If the attribute is not present in the query, every returned entry is considered a group, for example: 'ou=groups,dc=example,dc=com??one?(member=%D)' |
| max\_parallel\_connections  | 100  | Maximum number of parallel connections that can be established with LDAP servers.  |
| max\_cache\_size  | 10000  | Maximum number of requests that can be cached, defaults to 10000.  |
| cache\_value\_lifetime  | 300000  | Lifetime of values in cache in milliseconds. Default 300000 ms.  |
| enable\_nested\_groups  | false  | If enabled Couchbase server will try to recursively search for groups for every discovered ldap group.  |
| nested\_groups\_max\_depth  |  10 |  Maximum number of recursive groups requests the server is allowed to perform. This option is only valid when nested groups are enabled. The depth is an integer between 1 and 100, default is 10. |
| request_timeout  |  1000 | The timeout for LDAP requests in milliseconds.  |

##### Example

```yaml
couchbase_ldap:
  hosts:
    - ldap.example.com
  port: 389
  encryption: none
  server_cert_validation: false
  bind_dn: uid=aaronb,ou=People,dc=example,dc=com
  bind_password: password
  authentication_enabled: true
  user_dn_query: ou=People,dc=example,dc=com??one?(uid=%u)
  authorization_enabled: true
  group_query: ou=People,dc=example,dc=com?(gidNumber=5000)?one
  max_parallel_connections: 100
  max_cache_size: 10000
  cache_value_lifetime: 300000
  enable_nested_groups: true
  enable_nested_groups: true
  nested_groups_max_depth: 10
```

---

### couchbase\_user\_groups[]

The `couchbase_user_groups` variable is an empty list by default.  When specified the following properties are available to each item in the list, the `name` property is required, all other properties are optional and will be assigned to the default value listed if not specified.  [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-user-manage.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| \*name  |  *null* |  Specifies the target group for the group operations |
| description  |  *null* | Specifies the group description  |
|  ldap_ref | *null*  | Specifies the LDAP group’s distinguished name, to link the couchbase group with the LDAP one. |
|  roles |  [] |  Specifies the roles to be given to an RBAC user profile.  See the [ROLES](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-user-manage.html) section for more details on the available roles in Couchbase. |

##### Example

```yaml
couchbase_user_groups:
  - name: Demo
    description: test
    roles:
      - cluster_admin
      - replication_admin
  - name: Test
    description: test
    roles:
      - cluster_admin
      - replication_admin
  - name: admins
    description: test
    roles:
      - cluster_admin
      - replication_admin
```

---

### couchbase_users[]

The `couchbase_users` variable is an empty list by default.  When specified the following properties are available to each item in the list, the `name` property is required, all other properties are optional and will be assigned to the default value listed if not specified.   [Additional Documentation](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-user-manage.html)

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| username  |  *null* | Specifies the username of the RBAC user to modify. This option is used when deleting, creating, or updating an RBAC user profile.  |
| password  |  *null* | Specifies the password to be used for an RBAC user profile. This option is used only when creating or updating a local RBAC user profile. Couchbase does not store password for external RBAC roles.  |
| name  | *null*  | Specifies the name to be used for an RBAC user profile and it is recommended that this option be set to the users full name.  |
| roles  | []  | Specifies the roles to be given to an RBAC user profile.  See the [ROLES](https://docs.couchbase.com/server/current/cli/cbcli/couchbase-cli-user-manage.html) section for more details on the available roles in Couchbase.  |
| groups  | *null*  | Specifies the groups the user should be added to.  |
|  domain | local  | Specifies the auth_domain to use for a RBAC user profile and it may be set to either local or external. Local users are users that are managed directly by the Couchbase cluster. External users are users managed by an external source such as LDAP.  |

##### Example

```yaml
couchbase_users:
  - username: aaronb
    name: Aaron B
    roles:
      - admin
      - cluster_admin
    groups:
      - Demo
    auth_domain: external
  - username: jadt
    password: password
    name: Jad Talbert
    roles:
      - admin
      - cluster_admin
    groups:
      - Demo
      - Test
    auth_domain: local
```

---

### couchbase_indexes[]

The `couchbase_indexes` variable is an empty list by default.  When specified the following properties are available to each item in the list, the `bucket` and `definition` properties are required, all other properties are optional and will be assigned to the default value listed if not specified.

| **Variable** | **Default** | **Description** |
| :--- | :--- | :--- |
| \*bucket  | *null*  | The name of the bucket to create the index on  |
| \*definition  | *null*  |  The index definition to create.  Do not specify the `WITH {...}` block  |
| num_replicas  |  *null* | The number of replicas to create  |
| num_partitions  |  *null* |  The number of index partitions to create if using `PARTITION BY` in the index definition |
| nodes  | []  |  A list of nodes to deploy the index to |
|  sec\_key\_size | *null*  |  The average length of the combined index key values |
|  doc\_key\_size |  *null* | The average length of the document key meta().id  |
| arr_size  | *null*  |  The average length of the array field. Non-array fields will be ignored. |
|  num_doc | *null*  | The number of documents expected to be in the index  |
| resident_ratio  | *null*  | The estimated resident ratio of the index  |

##### Example

```yaml
couchbase_indexes:
  - bucket: demo
    definition: CREATE INDEX idx_test ON demo (username)
    replicas: 1
  - bucket: demo
    definition: CREATE INDEX idx_test2 ON demo (email)
    replicas: 1
  - bucket: ecommerce
    definition: CREATE INDEX idx_test3 ON demo (username)
    replicas: 1
```

