## v0.1.0

- Initial effort

## v0.1.1

- Basic installation
- README updates

## v0.1.2

- Renamed module to couchbase_server
- Update email address

## v0.1.3

- Add symlinks / plumbing to examples dir for convenience

## v0.1.4

- Remove the unnecessary library symlink as role pulls in the module now

## v0.1.5

- Note about linking role for examples

## v0.1.6

- Update example playbooks to use role

## v0.1.7

- Add an example_hosts inventory to the examples dir

## v0.1.8

- Remove the Couchbase Server module
- Add a site playbook to the examples
- Update example create_bucket playbook to use shell module

## v0.1.9

- Added Vagrant example for Mac OS X development clusters

## v0.2.0

- Corrected bad references to old role

## v0.2.1

- More Vagrant improvements
- Documentation updates
- Fixed variable names for packages

## v0.2.2

- Update defaults and variables
- Inline docs updates

## v0.2.3

- Fix broken RedHat tasks

## v0.2.4

- Revert previous defaults/variables changes
- Update include path in bucket creation playbook

## v0.2.5

- Add disable THP
- comment out experimental sysctls

## v0.2.6

- Immediate disabling of THP

## v0.2.7

- Update tasks

## v0.2.8

- Correct OS family variables

## v0.2.9

- Update rebalance options for cluster_init playbook

## v0.2.10

- Added wait for nodes to come up before initializing the cluster

## v0.2.20

- Added a cluster_install.yml to example playbooks for installation only

## v0.2.21

- Moved Ubuntu files to Debian

## v0.2.22

- Fixed server RAM quota issue for Vagrant on Mac OS X

## v0.3.0

- Switched Vagrant box source for Ubuntu 12.04
- Updated Ubuntu deb SHA256 sum for refreshed 2.5.1 build

## v0.4.0

- Cleanup tasks for Couchbase Server packages
- Switch to apt module for Ubuntu package installation

## v0.5.0

- Streamlined create_bucket example playbook
- Added a load_bucket example playbook

## v0.6.0

- More documentation updates

## v0.7.0

- Updated to Couchbase Server version 3.0.0

## v0.7.1

- Updated Vagrant box URL

## v0.8.0

- Couchbase Server version 3.0.1

## v0.8.1

- Comment out older version

## v0.8.2

- Update tested versions
- Update private key locations in inventory for Vagrant version 1.7.0

## v0.9.0

- Couchbase Server version 3.0.2
- Couchbase Server version 2.52
- Updated variables

## v0.9.1

- Fix bad package URL

## v1.0.0

- Ansible 1.8 support
- Increased timeout in get_url tasks
- Updated docs

## v1.0.1

- Correct package versions

## v1.0.2

- Update versions

## v1.0.3

- Update docs
- Update versions

## v1.0.4

- Fix package names in CentOS
- Update versions

## v1.0.5

- Added Couchbase Server Community Edition
- Conditionally install Community or Enterprise editions based on
  couchbase_server_edition variable setting
- Update software versions
- Update documentation

## v1.0.6

- Increase timeout for package downloads to handle extremely slow internet

## v1.0.7

- Specific distribution support for CentOS, Debian, RHEL, and Ubuntu
- Update version support
- Update documentation

## v1.0.8

- Include Debian and Red Hat hosts

## v1.0.9

- Couchbase Server Enterprise Edition version 3.0.3
- Update README

## v1.0.10

- Specification of hostnames during node-init for >= 3.0.x clusters
- Updated version references
- Corrected meta versions
- Updated .gitignore
- Updated supported versions
- Updated documentation

## v1.0.11

- Updated supported versions
- Updated documentation

## v1.0.12

- Updated supported versions
- Updated documentation

## v1.0.13

- Corrected meta versions
- Updated documentation

## v1.0.14

- Corrected package/SHA

## v1.0.15

- Update timeout for cluster_init
- Use ansible_os_distribution throughout
- Do not attempt to disable THP on Ubuntu 12.04
- Rename couchbase_server_tune_disks to couchbase_server_tune_os
- Add template tags to templates/etc_sysctl.d_couchbase-server.conf.j2
- Rename default bucket to "default" in create_bucket playbook
- Update documentation

## v1.0.16

- create_bucket playbook now creates bucket named default on correct port
- Updated documentation

## v1.0.17

- Wait for all nodes to be listening on 8091 before clustering operations

## v1.0.18

- Update documentation regarding ansible-galaxy, roles path, etc.

## v1.0.19

- Removed rc.local task
- Updated variables
- Updated documentation

## v1.0.20

- Added cluster_collect_info playbook
- Updated cluster_install playbook
- Updated documentation

## v1.0.21

- Added cluster_backup playbook
- Corrected load_bucket playbook
- Updated documentation

## v1.0.22

- Remove cleanup tasks (it's in the tmp dir after all)
- Update playbooks
- One wait instance for REST port only

## v1.0.23

- Perform cluster operations serially
- Add node_failover playbook
- Add cleanup tasks back to post installation
- Add /etc/profile.d/couchbase-server.sh template
- Vagrant hosts convenience script updates
- Updated Vagrantfile
- Updated documentation

## v1.0.24

- Update versions
- Update documentation
- Add retrieve_ssl_cert playbook
- Reduce server RAM quota for Vagrants

## v1.0.25

- Namespace node_role variable
- More granularity in clustering operations in advance of 4.0.0 and 
  per node service types
- Update Vagrantfile with ANSIBLE_PLAYBOOK environment variable support
- Add couchbase_server_services variable
- Remove unnecessary OS-specific host inventory examples
- Update documentation

## v1.0.26

- Update Vagrantfile
- Update cluster_init playbook
- Add vagrant_hosts example inventory

## v1.0.27

- Update Vagrantfile with additional vars examples
- Update versions
- Update documentation

## v1.0.28

- Update documentation

## v1.0.29

- Update THP task
- Update supported Ansible version to 1.9.2
- Update documentation

## v1.0.30

- Update Vagrantfile
- Update vagrant_hosts
- Update convenience script

## v1.0.31

- Fix Vagrant SSH key path issue
- Prepare firewall rules for use
- Update documentation

## v1.0.32

- Fixed default variables issue

## v1.0.33

- Fix firewall includes
- Fix default varible issue
- Update versions
- Update README_VAGRANT

## v1.0.34

- Include tuning variables

## v1.0.35

- Update preinstall script
- Updated Vagrantfile
- Update Vagrant documentation
- Update cluster init playbook

## v1.0.36

- Updated documentation

## v1.0.37

- Merge PR from Jordan Moore to prefer ansible_fqdn in cluster init steps
- Updated all playbooks to use ansible_fqdn where applicable
- Updated default variables
- Updated playbooks to use new variables
- Added node health verification in create bucket playbook
- Added bucket warmup verification in load bucket playbook
- Added bucket warmup verification in cluster backup playbook
- Fixed variable issue in cluster backup playbook
- Updated documentation

## v1.1.0

- New local package copy option
- Improved cluster init playbook
- Use file module instead of shell module for cleanup tasks
- Updated documentation
 - Added troubleshooting section
- Updated supported OS versions in meta

## v1.2.0

- Updated for Couchbase Server version 3.1.0
- Updated documentation

## v1.2.1

- Removed CentOS 2.5.1 package
- Added comments in cluster initialization playbook
- Fixed conditionals in cluster initialization playbook
- Removed serial: 1 from tasks in cluster initialization playbook
- Added failures for node warmup and health to bucket create 
  and bucket load playbooks

## v1.2.2

- Updated playbooks to reduce some wait times
- Fixed version checking in cluster initialization commands
- Updated documentation

## v1.3.0

- Couchbase Server 4.0.0
- Support for MDS / services
- Maximum supported OS versions (e.g. RHEL 7, Ubuntu 14.04) by default
- Removed all vars/* files and placed their content in defaults/* files
- Updated documentation

## v1.3.1

- Remove unicode from README so role will reimport into Galaxy again

## v1.3.2

- Updated iptables rules; thank you, @Hughjmp
- Moved MDS example host inventory into example_hosts file
- Removed vagrant_hosts_mds file
- Updated documentation

## v1.3.3

- Updated iptables playbooks
- Updated iptables ports

## v1.3.4

- Added .sh extension to preinstall script
- Updated documentation

## v1.3.5

- Updated supported versions
- Updated documentation

## v1.3.6

- Fix short host names

## v1.3.7

- Add Community Edition version 4.0.0 to defaults
- Add version 3.1.1 to defaults
- Remove version 3.0.0
- Remove version 2.5.2
- Update documentation

## v1.4.0

- Couchbase Server version 3.1.2

## v1.4.1

- Install CE 4.0.0 for Ubuntu 14.04 (thanks @SomeoneWeird)

## v1.4.2

- Fixed and renamed node_failover.yml
- Updated versions

## v1.5.0

- Add Couchbase Server Enterprise Edition version 4.1.0
- Add Couchbase Server Developer Preview version 4.1.0 (for community)

## v1.5.1

- Added version 3.1.3

## v1.5.2

- Updated versions

## v1.5.3

- Documentation fixes (thanks @zabullet)
- Update Vagrant documentation
- Add contributors to README

## v1.6.0

- Package file is now downloaded once to local / Ansible host and then
  copied to each node to speed up role and avoid excessive bandwidth usage
- Update Vagrantfile
- Update tasks
- Update variables
- Update docs

## v1.6.1

- Corrected tasks variables

## v1.6.2

- Consistent variable names in cluster_init

## v1.6.3

- Corrected Ubuntu tasks variables

## v1.6.4

- cluster_init now ensures data and index paths are present and have correct
  permissions (thanks @zabullet)

## v1.6.5

- Update playbooks
- Update main tasks
- Update supported software versions
- Update docs

## v1.6.6

- Include rc.local with THP disable
- Fix disable THP task

## v1.6.7

- Use file module for removal in playbooks

## v1.6.8

- Correct Galaxy meta

## v1.6.9

- Rename role to match GitHub repository name
- Update documentation

## v1.7.0

- Khanify dependencies, YAML, etc.

## v1.7.1

- YAML... how it does it?

## v1.7.2

- Use role_path to help with inclusion of role into other roles
  (thanks @franklefebvre)

## v1.7.3

- Use become instead of sudo
- Update minimum Ansible version in meta
- Update supported versions

## v1.7.4

- Ensure MDS services are set via host inventory
- Update Vagrant nodes to 2GB RAM
- Update playbook notes
- Update documentation

## v1.7.5

- Added Couchbase Server Enterprise Edition 3.1.4
- Updated documentation

## v1.7.6

- Added Couchbase Server Enterprise Edition 4.1.1
