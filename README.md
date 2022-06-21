lihas_keepalived
=========

Install and configure keepalived to manage gateways on virtualization hosts

Intended special setup:
* 1 gateway per virtualization host to enable a local virtual machine (lxc/kvm) to exit this very virtualization host via a direct attached interface instead of relaying via central gateway on one of the virtualization hosts, despite using internal private IPs
* to achieve this the gateway and the lxc/kvm share a host-only network, the gateways on all hosts have the same IP in this network
* the official IPs for services are DNATed via firewall-lihas
* the official IPs for services are active on the gateway on the same host with the service lxc/kvm, managed via keepalived

Requirements
------------

uses lihas_common for configuration
To run solo:

```
ansible-galaxy install -r requirements.yml
ansible-playbook -i localhost, keepalived.yml
```

Tags
----
* keepalived_config: only do configuratiuon, no software installation, needs tag `variables` as well

Role Variables
--------------

```
keepalived.default_interface
keepalived.routers.%: % is the value of %.config.roles.keepalived.ip_correlation.%.extern_ipv4_gw
keepalived.routers.%.rt_table_name: routing table name for separate gateway
keepalived.routers.%.rt_table_id: routing table id

roles.proxmox.lxc.%.extern_ipv4 # extern IPv4 with netmask
roles.proxmox.lxc.%.extern_ipv4_gw
roles.proxmox.lxc.%.extern_ipv4_iface # external interface name on gateway for this IP
roles.proxmox.lxc.%.intern_ipv4
roles.proxmox.lxc.%.infrastructure: firewall_cluster # only informative

%.config.keepalived.default_interface: default interface for vrrp packets
%.config.keepalived.vip.ID.interface: specific interface for vrrp packets, ID is the VRRP instance number, unique, 0-255
%.config.keepalived.vip.ID.ipres[]: list of ip resources
```
Dependencies
------------

Example Playbook
----------------

```
---
- hosts: '*'
  role: lihas_keepalived
...
```

Example Variables
-----------------
```
myhost:
  config:
    keepalived:
      default_interface: eth0
      vip:
        '53':
           interface: eth2
           ipres:
           - 10.0.0.1/24 dev eth1
```

License
-------

GPL-3.0-or-later

Author Information
------------------

Adrian Reyer <lihas@lihas.de>, http://lihas.de/
