# Ansible Consul

Master: [![Build Status](https://travis-ci.org/lobsterdore/ansible-consul.svg?branch=master)](https://travis-ci.org/lobsterdore/ansible-consul)
Develop: [![Build Status](https://travis-ci.org/lobsterdore/ansible-consul.svg?branch=develop)](https://travis-ci.org/lobsterdore/ansible-consul)

* [To install](#to-install)
* [How to use](#how-to-use)
* [Tags](#tags)
* [Development](#development)
* [Contributing](#contributing)

Install and configures Consul as a server or agent, can optionally setup DNS resolution
vis Dnsmasq.





## To install

The easiest installation method is via Ansible Galaxy:

```BASH
ansible-galaxy install lobsterdore.ansible-consul
```

In requirements file:

```BASH
---
# requirements.yml

- src: lobsterdore.ansible-consul
  version: v1.0.0

```

To see available versions please check this roles [Ansible Galaxy page](https://galaxy.ansible.com/lobsterdore/ansible-consul/).




## How to use

Example single server setup:

```YAML
---

dependencies:

  - role: ansible-consul
    become: yes
    consul_settings:
      bootstrap: false
      bootstrap_expect: 1
      datacenter: "aws-eu-west-1"
      leave_on_terminate: true
      log_level: "warn"
      rejoin_after_leave: true
      retry_join: []
      server: true
```

Example agent setup:

```YAML
---

dependencies:

  - role: ansible-consul
    become: yes
    consul_settings:
      datacenter: "aws-eu-west-1"
      leave_on_terminate: true
      log_level: "warn"
      rejoin_after_leave: true
      ports: {}
      retry_join:
        - consul.server
      server: false
    consul_use_as_nameserver: yes
```

Enable AWS discovery via tags:

```YAML
---

dependencies:

  - role: ansible-consul
    become: yes
    consul_aws_ips_autodiscover_enabled: true
    consul_aws_ips_autodiscover_lookup_filter: "Name=tag:Env,Values=prd Name=tag:Role,Values=consul_server Name=instance-state-name,Values=running"
    consul_settings:
      bootstrap: false
      bootstrap_expect: 1
      datacenter: "aws-eu-west-1"
      leave_on_terminate: true
      log_level: "warn"
      rejoin_after_leave: true
      retry_join: []
      server: true
```




## Tags

This role uses two tags: **build** and **configure**

* `build` - Installs packages, can be used to bake AMIs
* `configure` - Configures packages, can be used on boot for pre-baked AMIs




## Development

A Vagrant box is included that you can use to test this role, a Makefile is included as well
that contains some useful targets for testing, to see a list of targets you can do the following:

```BASH
make
```




## Contributing

If you would like to contribute to this role please open a Github issue first, then open your PR and reference the issue.
