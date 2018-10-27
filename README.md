haproxy
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-haproxy.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-haproxy)

The Reliable, High Performance TCP/HTTP Load Balancer

[Unit tests](https://travis-ci.org/robertdebock/ansible-role-haproxy) are done on every commit and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-haproxy/issues)

To test this role locally please use [Molecule](https://github.com/metacloud/molecule):
```
pip install molecule
molecule test
```
There are many scenarios available, please have a look in the `molecule/` directory.

Context
-------
This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/haproxy.png "Dependency")

Requirements
------------

Access to a repository containing packages, likely on the internet.

Role Variables
--------------

-   haproxy_stats: If statistics should be enabled. Default: yes
-   haproxy_stats_port: What TCP port should serve statistics. Default: 1936

The next set of variables are values copy-pasted from haproxy.cfg an can be altered to your preference.
-   haproxy_retries: Default: 3
-   haproxy_timeout_http_request: Default: 10s
-   haproxy_timeout_queue: Default: 1m
-   haproxy_timeout_connect: Default: 10s
-   haproxy_timeout_client: Default: 1m
-   haproxy_timeout_server: Default: 1m
-   haproxy_timeout_http_keep_alive: Default: 10s
-   haproxy_timeout_check: Default: 10s
-   haproxy_maxconn: Default: 3000

-   haproxy_frontends: A list of dictionaries to configure all frontends. Default:
```
haproxy_frontends:
  - name: main
    address: "*"
    port: 80
    default_backend: app
```

-   haproxy_backends: A list of dictionaries to configure all backends. Default:
```
haproxy_backends:
  - name: app
    balance: roundrobin
    servers: "{{ groups['all'] }}"
    port: 80
    options:
      - check
```

Note: Front and backends must match, if you refer to a non-existing backend, haproxy will run into troubles.

Dependencies
------------

This role can be used to prepare your system:

-   [robertdebock.bootstrap](https://travis-ci.org/robertdebock/ansible-role-bootstrap)

Download the dependencies by issuing this command:
```
ansible-galaxy install --role-file requirements.yml
```

Compatibility
-------------

This role has been tested against the following distributions and Ansible version:

|distribution|ansible 2.4|ansible 2.5|ansible 2.6|ansible 2.7|ansible devel|
|------------|-----------|-----------|-----------|-----------|-------------|
|alpine-edge*|yes|yes|yes|yes|yes*|
|alpine-latest|yes|yes|yes|yes|yes*|
|archlinux|yes|yes|yes|yes|yes*|
|centos-6|yes|yes|yes|yes|yes*|
|centos-latest|yes|yes|yes|yes|yes*|
|debian-latest|yes|yes|yes|yes|yes*|
|debian-stable|yes|yes|yes|yes|yes*|
|debian-unstable*|yes|yes|yes|yes|yes*|
|fedora-latest|yes|yes|yes|yes|yes*|
|fedora-rawhide*|yes|yes|yes|yes|yes*|
|opensuse-leap|yes|yes|yes|yes|yes*|
|opensuse-tumbleweed|yes|yes|yes|yes|yes*|
|ubuntu-artful|yes|yes|yes|yes|yes*|
|ubuntu-devel*|yes|yes|yes|yes|yes*|
|ubuntu-latest|yes|yes|yes|yes|yes*|

A single star means the build may fail, it's marked as an experimental build.

Example Playbook
----------------

The simplest way possible:
```
- hosts: servers
  become: true

  roles:
    - robertdebock.bootstrap
    - robertdebock.haproxy
```

To use a group in the inventory as the list of servers in the backend:

`inventory`:
```
[webservers]
192.168.0.1
192.168.0.2
192.168.0.3
```

`playbook.yml`:
```
- hosts: haproxy
  become: true

  roles:
    - robertdebock.bootstrap
    - robertdebock.haproxy
      haproxy_backends:
       - name: app
         balance: roundrobin
         servers: "{{ groups['webservers'] }}"
         port: 80
         options:
           - check
```

Install this role using `galaxy install robertdebock.haproxy`.

License
-------

Apache License, Version 2.0

Author Information
------------------

[Robert de Bock](https://robertdebock.nl/) <mailto:robert@meinit.nl>
