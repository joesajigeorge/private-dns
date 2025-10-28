# Complete Private DNS Topology

## Overview
This repository provides a lab DNS topology, implemented in Docker Compose, using BIND. The environment consists of a private root, delegated TLDs, and separate authoritative and resolver services. Zone transfers, notifications, and explicit glue records are fully configured, supporting advanced DNS delegation and failover scenarios. No internet connectivity is required.

## Directory Structure
```
dns/
├── auth-master/
│   ├── named.conf
│   └── zones/
├── auth-replica1/
│   ├── named.conf
│   └── zones/
├── auth-replica2/
│   ├── named.conf
│   └── zones/
├── docker-compose.yml
├── logs/
│   ├── auth-master/
│   ├── auth-replica1/
│   ├── auth-replica2/
│   ├── resolver/
│   ├── root/
│   ├── tld-lab/
│   └── tld-pvt/
├── resolver/
│   └── named.conf
├── root/
│   ├── db.root
│   └── named.conf
├── tld-lab/
│   ├── db.lab
│   └── named.conf
├── tld-pvt/
│   ├── db.pvt
│   └── named.conf
```
## Services

root: Authoritative DNS root. Delegates .lab and .pvt to TLD services.

tld-lab, tld-pvt: Authoritative for lab and pvt. Delegate zones to auth servers.

auth-master: Master for all lab and pvt test zones. Sends NOTIFY/AXFR to replicas.

auth-replica1, auth-replica2: BIND slaves, receive transfers from master.

resolver: Recurses, starting at internal root. Forwarding mode enabled for lab root.

## Testing

```
dig @127.0.0.0.1 -p 1054 www.test1example.lab
dig @127.0.0.0.1 -p 1054 api.test1example.lab
dig @127.0.0.0.1 -p 1054 www.test2example.lab
dig @127.0.0.0.1 -p 1054 api.test2example.lab
dig @127.0.0.0.1 -p 1054 www.test3example.lab
dig @127.0.0.0.1 -p 1054 api.test3example.lab
dig @127.0.0.0.1 -p 1054 www.test1example.pvt
dig @127.0.0.0.1 -p 1054 api.test1example.pvt
```
