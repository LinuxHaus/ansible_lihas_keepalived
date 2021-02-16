lihas_keepalived
=========

Install and configure keepalived to manage gateways on virtualization hosts

Requirements
------------

uses lihas_common for configuration
To run solo:

```
ansible-galaxy install -r requirements.yml
ansible-playbook -i localhost, keepalived.yml
```

Role Variables
--------------

```
roles.proxmox.lxc.%.extern_ipv4 # extern IPv4 with netmask
roles.proxmox.lxc.%.extern_ipv4_gw
roles.proxmox.lxc.%.extern_ipv4_iface # external interface name on gateway for this IP
roles.proxmox.lxc.%.intern_ipv4
roles.proxmox.lxc.%.infrastructure: firewall_cluster # only informative
```
Dependencies
------------

Example Playbook
----------------

```
---
- hosts: '*'
  role: lihas_dehydrated
...
```


License
-------

GPL-3.0-or-later

Author Information
------------------

Adrian Reyer <lihas@lihas.de>, http://lihas.de/
