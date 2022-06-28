# mysqlsh-plugin

Ansible Role for install mysqlsh-plugins and for manage proxysql-plugin with Mysql8.0 InnoDB Replication

these plugins is developed by <https://github.com/lefred/mysqlshell-plugins>

## Requirements

This role requires Ansible 2.9 or higher.

## Role Variables

The role defines its variables in `defaults/main.yml`:

## Varibles definitition

|VARIABLE|DESCRIPTION|DEFAULT VALUE|
|--------|-----------|-------------|
|mysql_user|user for mysql|"root"|
|mysql_root_password|password for mysql user|"password"|
|proxysql_remoteadmin_user|remote admin for proxysql|"remoteadmin"|
|proxysql_remoteadmin_password|remoteadmin password for proxysql|"password"|
|mysql_ip|ip of mysql MASTER|"192.168.0.130"|
|proxysql_ip|proxysql ip, this can be a VIP if you have 2 proxysql|"192.168.0.134"|

## Proxysql Varibles for plugin

|VARIABLE|DESCRIPTION|DEFAULT VALUE|
|--------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|
|max_writers|ProxySQL will limit the number of Read-Write instances populated in the writer hostgroup based on the value defined in max_writers for the cluster. Any nodes exceeding this number are placed into the backup_writer_hostgroup and ProxySQL will move these to the writer_hostgroup to actively serve traffic as needed|"1"|
|writer_is_also_reader|This parameter has three possible values: (0, 1, 2).
– writer_is_also_reader=0: nodes with `read_only=0` will be placed either in the writer_hostgroup and in the backup_writer_hostgroup after a topology change, these will be excluded from the reader_hostgroup.
– writer_is_also_reader=1: nodes with `read_only=0` will be placed in the writer_hostgroup or backup_writer_hostgroup and are all also placed in reader_hostgroup after a topology change.
– writer_is_also_reader=2 : Only the nodes with `read_only=0` which are placed in the in the backup_writer_hostgroup are also placed in the reader_hostgroup after a topology change i.e. the nodes with `read_only=0` exceeding the defined `max_writers`.|"1"|
|max_transaction_behind| determines the maximum number of transactions behind the writers that ProxySQL should allow before shunning the node to prevent stale reads (this is determined by querying the transactions_behind field of the sys.gr_member_routing_candidate_status table in MySQL|"200"|
|use_ssl|enable ssl comunication with mysql backend |"0"|
|monitor_user|monitor user, Use Always monitor as monitor-user also in proxysql.conf |"monitor"|
|monitor_password|password for user monitor |"password"|

## Example Playbook

Run with default vars:

```
---
- name: Copy adn install plugins for mysqlsh and enabale/connect proxysql-plugin 
  hosts: all
  become: true
  become_user: root
  become_method: sudo
  gather_facts: true
  roles:
    - role: mysqlsh-plugin-role
```
