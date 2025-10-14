# Complete Private DNS Hierarchy

## Overview
This project defines a complete private DNS root, TLD zone servers, an iterative resolver, and a multi-master (plus replicas) authoritative setup for multiple lab zones, all orchestrated via Docker Compose. The system uses BIND (named) containers and custom configuration for isolation, delegation, and replication between simulated root, TLD, and child zones, enabling complex DNS experiments without external dependencies.​

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
## Components

### Root Server
Authoritative for the DNS root zone; delegates .lab and .pvt TLDs to respective containers

### TLD Servers
Serve lab and pvt TLDs, delegate child zones to auth servers

### Authoritative Master & Replicas
Master at auth-master serves four zones, triggers NOTIFY and AXFR/IXFR to replicas
Replicas (auth-replica1, auth-replica2) serve as BIND slaves for transfer and failover

### Resolver
Recursively resolves from root, traversing the hierarchy via iterative queries
Forwarders are set to root server for referral chains within the lab