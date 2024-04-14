# Info

This Repo is a fork of [mpawlik/zabbix_zfs-on-linux](https://github.com/mpawlik/zabbix_zfs-on-linux) which is a fork of [Cosium/zabbix_zfs-on-linux](https://github.com/Cosium/zabbix_zfs-on-linux) which was originally made by [pbergdolt](https://www.zabbix.com/forum/zabbix-cookbook/35336-zabbix-zfs-discovery-monitoring?t=43347).

Most importatly it is a passive Template instead of active and fixes some small issues like incorrect IO reporting.

It works on Zabbix 6.0, I didn't try other versions.


# Monitor ZFS on Linux on Zabbix

This template will give you screens and graphs for memory usage, zpool usage and performance, dataset usage, etc. It includes triggers for low disk space (customizable via Zabbix own macros), disks errors, etc.

Example of graphs:
- Arc memory usage and hit rate:
![arc1](images/example_arc_1.png)
- Complete breakdown of META and DATA usage:
![arc2](images/example_arc_2.png)
- Dataset usage, with available space, and breakdown of used space with directly used space, space used by snapshots and space used by children:
![dataset](images/example_dataset_usage_1.png)
- Zpool IO throughput:
![throughput](images/example_zfs_throughput.png)

# Supported OS and ZoL version
Any Linux variant should work, tested version by myself include:
- Debian 12

About the ZoL version, this template is intended to be used by ZoL version 0.7.0 or superior but still works on the 0.6.x branch.


# Installation on Zabbix server


## Import the template
Import the template that is in the "template" directory of this repository or download it directly with this link: ![template](template/zol_template.xml)


# Installation on the server you want to monitor
## Prerequisites
The server needs to have some very basic tools to run the user parameters:
- awk
- cat
- grep
- sed
- tail

Usually, they are already installed and you don't have to install them.


## Add the userparameters file on the servers you want to monitor

On recent ZFS on Linux versions (eg version 0.7.0+), you don't need sudo to run `zpool list` or `zfs list` so just install the file ![ZoL_without_sudo.conf](userparameters/ZoL_without_sudo.conf) and you are done.

If you don't know where your "userparameters" directory is, this is usually the `/etc/zabbix/zabbix_agentd.d` folder. If in doubt, just look at your `zabbix_agentd.conf` file for the line begining by `Include=`, it will show where it is.


## Restart zabbix agent
Once you have added the template, restart zabbix-agent so that it will load the new userparameters.


# Customization of alert level by server
This template includes macros to define when the "low disk spaces" type triggers will fire.

By default, you will find them on the macro page of this template:
![macros](images/macros.png)

If you change them here, they will apply to every hosts linked to this template, which may not be such a good idea. Prefer to change the macros on specific servers if needed.

You can see how the macros are used by looking at the discovery rules, then "Trigger prototypes":
![macros](images/trigger_prototypes_zpool.png)

