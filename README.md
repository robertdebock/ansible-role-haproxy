haproxy
=========

<img src="https://docs.ansible.com/ansible-tower/3.2.4/html_ja/installandreference/_static/images/logo_invert.png" width="10%" height="10%" alt="Ansible logo" align="right"/>
<a href="https://travis-ci.org/robertdebock/ansible-role-haproxy"> <img src="https://travis-ci.org/robertdebock/ansible-role-haproxy.svg?branch=master" alt="Build status"/></a> <img src="https://img.shields.io/ansible/role/d/28674"/> <img src="https://img.shields.io/ansible/quality/28674"/>

<a href="https://github.com/robertdebock/ansible-role-haproxy/actions"><img src="https://github.com/robertdebock/ansible-role-haproxy/workflows/GitHub%20Action/badge.svg"/></a>

Install and configure haproxy on your system.

Example Playbook
----------------

This example is taken from `molecule/resources/playbook.yml` and is tested on each push, pull request and release.
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - robertdebock.haproxy
```

The machine you are running this on, may need to be prepared, I use this playbook to ensure everything is in place to let the role work.
```yaml
---
- name: Prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - robertdebock.bootstrap
    - robertdebock.core_dependencies
```


Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

Role Variables
--------------

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for haproxy

# Configure stats in HAProxy?
haproxy_stats: yes
haproxy_stats_port: 1936

# Default setttings for HAProxy.
haproxy_retries: 3
haproxy_timeout_http_request: 10s
haproxy_timeout_queue: 1m
haproxy_timeout_connect: 10s
haproxy_timeout_client: 1m
haproxy_timeout_server: 1m
haproxy_timeout_http_keep_alive: 10s
haproxy_timeout_check: 10s
haproxy_maxconn: 3000

# A list of frontends and their properties.
haproxy_frontends:
  - name: main
    address: "*"
    port: 80
    default_backend: app

# A list of backends and their properties.
haproxy_backends:
  - name: app
    balance: roundrobin
    servers: "{{ groups['all'] }}"
    port: 80
    options:
      - check
```

Requirements
------------

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap
- robertdebock.core_dependencies

```

Context
-------

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/haproxy.png "Dependency")


Compatibility
-------------

This role has been tested on these [container images](https://hub.docker.com/):

|container|tags|
|---------|----|
|amazon|Candidate|
|alpine|all|
|debian|all|
|el|7, 8|
|fedora|all|
|opensuse|all|
|ubuntu|bionic|

The minimum version of Ansible required is 2.8 but tests have been done to:

- The previous version, on version lower.
- The current version.
- The development version.

Exceptions
----------

Some variarations of the build matrix do not work. These are the variations and reasons why the build won't work:

| variation                 | reason                 |
|---------------------------|------------------------|
| amazonlinux:1 | /etc/init.d/haproxy: line 17: /etc/sysconfig/network: No such file or directory |


Testing
-------

[Unit tests](https://travis-ci.org/robertdebock/ansible-role-haproxy) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-haproxy/issues)

Testing is done using [Tox](https://tox.readthedocs.io/en/latest/) and [Molecule](https://github.com/ansible/molecule):

[Tox](https://tox.readthedocs.io/en/latest/) tests multiple ansible versions.
[Molecule](https://github.com/ansible/molecule) tests multiple distributions.

To test using the defaults (any installed ansible version, namespace: `robertdebock`, image: `fedora`, tag: `latest`):

```
molecule test

# Or select a specific image:
image=ubuntu molecule test
# Or select a specific image and a specific tag:
image="debian" tag="stable" tox
```

Or you can test multiple versions of Ansible, and select images:
Tox allows multiple versions of Ansible to be tested. To run the default (namespace: `robertdebock`, image: `fedora`, tag: `latest`) tests:

```
tox

# To run CentOS (namespace: `robertdebock`, tag: `latest`)
image="centos" tox
# Or customize more:
image="debian" tag="stable" tox
```

License
-------

Apache-2.0


Author Information
------------------

[Robert de Bock](https://robertdebock.nl/)
