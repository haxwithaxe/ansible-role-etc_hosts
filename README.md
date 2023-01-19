etc_hosts
=========

Populate `/etc/hosts` with the primary IPv4 addresses or specified addresses of an inventory group. This will overwrite existing entries for hosts in the specified group. In the event of an IP address left without a hostname it will be commented out.

Requirements
------------

An inventory group with one or more hosts. The hosts don't need an `ansible_default_ipv4.address` defined if `etc_hosts_address` is set. One of those values is required to create an entry in `/etc/hosts`.

Role Variables
--------------

* `etc_hosts_group` - This is used to select the group to load into `/etc/hosts`. Defaults to `all`.
* `etc_hosts_path` - The path to the hosts file. Defaults to `/etc/hosts`
* `etc_hosts_localhost_ipv4_address` - The IP address to use in the hosts file for the current node. Defaults to `127.0.0.11`.
* `etc_hosts_suffix` - The suffix to add to the hostname (eg `.local`). Defaults to nothing (an empty string).
* `etc_hosts_orphaned_message` - The message to leave in the comment for orphaned IP addresses. Defaults `Orphaned by Ansible {{ ansible_date_time.iso8601 }}`.

Dependencies
------------

None.

Example Playbook
----------------

Populate the `/etc/hosts` file on each machine in group `servers` with all the IP and hostname pairs of all the machines in group `servers`.
```yaml
    - hosts: servers
      vars:
        etc_hosts_group: servers
      roles:
         - haxwithaxe.etc_hosts
```

Populate the `/etc/hosts` file on each machine in group `servers` with the IP and hostname with suffix pairs of all the machines in group `servers`.
```yaml
    - hosts: servers
      vars:
        etc_hosts_group: servers
        etc_hosts_suffix: .foo
      roles:
         - haxwithaxe.etc_hosts
```

Add the IP addresses of the `myhosts` group to the local `/etc/hosts`. Facts need to be gathered for hosts not in the group being modified by `etc_hosts`.
```yaml
- name: Gather facts for the group you need IPs from (etc_hosts_group)
  hosts: myhosts
  tasks:

- name: Add hosts file entries for hosts in mygroup
  hosts: localhost
  vars:
    etc_hosts_group: myhosts
  roles:
    - haxwithaxe.etc_hosts
```

To set an IP for a host set `etc_hosts_address` for the host and that address will be used in other hosts' `/etc/hosts`. For example:

`host_vars/alice.yml`
```yaml
etc_hosts_address: 1.2.3.4
```

License
-------

GPLv3

Author Information
------------------

Created by [haxwithaxe](https://github.com/haxwithaxe)
