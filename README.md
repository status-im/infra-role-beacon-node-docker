# Description

This role provisions a [Nimbus](https://nimbus.status.im/) installation that can act as an ETH2 network bootstrap node.

# Introduction

Each host can run multiple nodes installed as [docker containers](https://github.com/status-im/nimbus/docker).
TCP and UDP ports starting from 9000 will be exposed (e.g. 9000, 9001 and 9002 if you run 3 nodes).

# Installation

Add to your `requirements.yml` file:
```yaml
- name: infra-role-beacon-node
  src: git+git@github.com:status-im/infra-role-beacon-node.git
  scm: git
```

# Configuration

The crucial settings are:
```yaml
beacon_node_network: 'testnet0'
# optional setting for debug mode
beacon_node_log_level: DEBUG
# Infura Web Sockets URLs
beacon_node_web3_urls: ['wss://mainnet.infura.io/ws/v3/123qwe123qwe123qwe']
```
The order of Web Socket URLs matters. First is the default, the rest are fallbacks.

# Usage

You can re-create containers on the host using:
```
cd /docker/beacon-node-testnet0-1
docker-compose --compatibility up -d --force-recreate
```
Which will use the `docker-compose.yml` file in that directory.

# Rebuilding

If you want to rebuild the image regularly you can enable it via:
```yaml
beacon_node_timer_rebuild: true
beacon_node_timer_frequency: 'daily'
```
Which should create a `beacon-node-${network}-rebuild` timer:
```
 > sudo systemctl list-timers 'beacon-node-*'
NEXT                        LEFT    LAST PASSED UNIT                               ACTIVATES                           
Wed 2020-11-04 00:00:00 UTC 9h left n/a  n/a    beacon-node-testnet3-rebuild.timer beacon-node-testnet3-rebuild.service

1 timers listed.
```
Which will run `docker-compose up --build --force-recreate -d`.

# Requirements

Due to being part of Status infra this role assumes availability of certain things:

* Docker for running containers
* The `iptables-persistent` module
