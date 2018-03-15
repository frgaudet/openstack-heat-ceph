# Disclaimer

This template is for educationnal purpose only. It just create a cluster ready to welcome Ceph.

# Introduction

* X OSD nodes, each one comes with 3 * 10G disks
* Y Monitors nodes
* 1 admin node
* An SSH user called `cephuser` is created and able to login from and to any node
* A default repository pointing to `jewel` is defined

# Quick start

1/ Source your OpenStack environnement file

```bash
source fgaudet-openrc.sh
```

2/ Create your stack using default parameters

```bash
openstack stack create -t ceph.yaml --parameter "key_name=fgaudet-key;image_id=Centos7;private_network=fgaudet-net2;public_network=ext-isimanet" Ceph
```

Of course you can override some default parameters :

```bash
openstack stack create -t main.yaml --parameter "key_name=fgaudet-key;image_id=Centos7;private_network=fgaudet-net2;public_network=ext-isimanet;osd_count=5;flavor=m1.medium" Ceph
+---------------------+---------------------------------------------------+
| Field               | Value                                             |
+---------------------+---------------------------------------------------+
| id                  | 098d20dc-8bc0-4034-aa71-0f60edf92528              |
| stack_name          | Ceph                                              |
| description         | Template that installs a cluster of ceph servers. |
| creation_time       | 2018-03-15T13:28:34                               |
| updated_time        | None                                              |
| stack_status        | CREATE_IN_PROGRESS                                |
| stack_status_reason | Stack CREATE started                              |
+---------------------+---------------------------------------------------+
```

3/ Get your public address

Use the stack ID (see above)

```bash
openstack stack show <Stack_ID>

+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| Field                 | Value                                                                                                                                |
+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| id                    | 098d20dc-8bc0-4034-aa71-0f60edf92528                                                                                                 |
| stack_name            | Ceph2                                                                                                                                |
| description           | Template that installs a cluster of ceph servers.                                                                                    |
| creation_time         | 2018-03-15T13:28:34                                                                                                                  |
| updated_time          | None                                                                                                                                 |
| stack_status          | CREATE_COMPLETE                                                                                                                      |
| stack_status_reason   | Stack CREATE completed successfully                                                                                                  |
| parameters            | OS::project_id: bd20d02f33a848ca88b7f13b83968fcb                                                                                     |
|                       | OS::stack_id: 098d20dc-8bc0-4034-aa71-0f60edf92528                                                                                   |
|                       | OS::stack_name: Ceph                                                                                                                 |
|                       | admin_name: admin                                                                                                                    |
|                       | flavor: m1.small                                                                                                                     |
|                       | image_id: Centos7                                                                                                                    |
|                       | key_name: fgaudet-key                                                                                                                |
|                       | mon_count: '3'                                                                                                                       |
|                       | mon_name: mon                                                                                                                        |
|                       | osd_count: '3'                                                                                                                       |
|                       | osd_name: osd                                                                                                                        |
|                       | private_key: '******'                                                                                                                |
|                       | private_network: fgaudet-net2                                                                                                        |
|                       | public_key: '******'                                                                                                                 |
|                       | public_network: ext-isimanet                                                                                                         |
|                       |                                                                                                                                      |
| outputs               | - description: The floating IP address of the admin node.                                                                            |
|                       |   output_key: ip                                                                                                                     |
|                       |   output_value: xxx.xxx.xxx.xxx                                                                                                      |
|                       |                                                                                                                                      |
| links                 | - href: https://orchestration.oscloud.isima.fr/v1/bd20d02f33a848ca88b7f13b83968fcb/stacks/Ceph/098d20dc-8bc0-4034-aa71-0f60edf92528  |
|                       |   rel: self                                                                                                                          |
|                       |                                                                                                                                      |
| disable_rollback      | True                                                                                                                                 |
| parent                | None                                                                                                                                 |
| tags                  | None                                                                                                                                 |
| stack_user_project_id | 8997bef2631c4da9bd426b39e832d705                                                                                                     |
| capabilities          | []                                                                                                                                   |
| notification_topics   | []                                                                                                                                   |
| timeout_mins          | None                                                                                                                                 |
| stack_owner           | None                                                                                                                                 |
+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------+
```

4/ Connect to your cluster

Look at the outputs values to find out the actual IP address

```bash
ssh centos@xxx.xxx.xxx.xxx
```

5/ Use privileged user

```bash
[centos@admin ~]$ sudo su - cephuser
```
SSH easily to any nodes
```bash
[cephuser@admin ~]$ ssh mon-1
Warning: Permanently added 'mon-1,10.0.1.193' (ECDSA) to the list of known hosts.
[cephuser@mon-1 ~]$
```
