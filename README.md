Ansible Role: Firewall
=========
[![Build Status](https://travis-ci.org/tenequm/ansible-firewall.svg?branch=master)](https://travis-ci.org/tenequm/ansible-firewall)

This role applies basic firewall configurations for Ubuntu/Debian and disables `ufw` service if installed.

After applying this role, you will have `firewall` init service, that can be controlled with the standard `service firewall [start|stop|restart|status]` commands.

Requirements
------------
This role requires only root access for accomplishing its operations, so either run it in a playbook with a global `become: yes`, or invoke the role in your playbook like:
```
- hosts: localhost
  become: yes
  roles:
    - role: tenequm.firewall
```

Role Variables
--------------
Drop traffic only when connecting to public interface (defaults to false):
```
firewall_drop_only_pub: false
```
Name of public interface (defaults to 'eth0'), used when `firewall_drop_only_pub` is set to `true`:
```
firewall_pub_interface: "eth0"
```
List of open TCP/UDP ports for incoming traffic:
```
firewall_allowed_tcp_ports:
  - 80
  - 443
  …
firewall_allowed_udp_ports: []
```
List of forwarded TCP/UDP ports:
```
firewall_forwarded_tcp_ports:
  - { src: "22", dest: "2222" }
  - { src: "80", dest: "8080" }
firewall_forwarded_udp_ports: []
```
Additional rules that can be added to the firewall in its standard CLI command format (e.g. iptables [rule] / ip6tables [rule]):
```
firewall_additional_rules: []
firewall_ip6_additional_rules: []

For example:

# Allow only the IP 173.123.12.55 to access port 6989.
firewall_additional_rules:
  - "iptables -A INPUT -p tcp --dport 6989 -s 173.123.12.55 -j ACCEPT"

# Allow only the IPv6 2001:db8:a0b:12f0::1 to access port 3306.
firewall_additional_rules:
  - "ip6tables -A INPUT -p tcp --dport 3306 -s 2001:db8:a0b:12f0::1 -j ACCEPT"
```

Example Playbook
----------------
```
- hosts: localhost
  become: yes
  roles:
    - { role: tenequm.firewall }
```
License
-------
MIT

Author Information
------------------
This role was created in 2017 by [Mykhaylo Kolesnik](http://github.com/tenequm).
