# Description

This role provisions a [Nimbus](https://nimbus.status.im/) installation that can act as an ETH2 network bootstrap node.

# Introduction

Nimbus is installed as a [docker container](https://github.com/status-im/nimbus/docker) and exposes the following ports:

* UDP 40404 - Used for peer discovery
* TCP 40404 - Used for persistent peer connections

At the moment, this role also setups a nginx site hosting the Nimbus testnet validator database (a simple json file).

# Installation

Add to your `requirements.yml` file:
```yaml
- name: nimbus
  src: https://github.com/status-im/infra-role-nimbus.git
```

# Requirements

Due to being part of Status infra this role assumes availability of certain things:

* Docker for running containers
* Nginx full installation
* The `iptables-persistent` module
