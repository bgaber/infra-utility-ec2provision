Purpose?
---
The purpose of this Ansible code is to provision Linux or Windows EC2 instances.

How Is It Installed?
---
No installation is necessary.  This code is an Ansible Playbook launched by [AWX](https://awx.sre.compucom.com).

How Is It Run?
---
It is launched via an [AWX](https://awx.sre.compucom.com) Template that links to the main.yml Playbook in this repo.

Required Resources
---
In the .drone.yml file are references to Docker image requirements for 472510080448.dkr.ecr.us-east-1.amazonaws.com/sre-team/imagebuilder:1.1 which is found in our AWS Elastic Container Registry (ECR). ECR is a fully managed Docker container registry.

Drone Configuration
------------------
This works when connected to VPN
https://drone.sre.compucom.com/

## Connected to VPN

nslookup drone.sre.compucom.com
Server:  spw099wdc01.compucom.local
Address:  10.98.3.49

Name:    internal-sre-1878520259.us-east-1.elb.amazonaws.com
Addresses:  10.251.2.38
          10.251.4.116
Aliases:  drone.sre.compucom.com

## Not Connected to VPN

nslookup drone.sre.compucom.com
Server:  tor-dns-recursive.primus.ca
Address:  216.254.141.2

Non-authoritative answer:
Name:    sre-external-1047161721.us-east-1.elb.amazonaws.com
Addresses:  52.5.20.116
          52.201.202.179
Aliases:  drone.sre.compucom.com


Load Balancers are found in the Shared Account

Drone configuration is found on the SPLAWSSREDKR01 server in the /docker/drone directory

Configuration file is /docker/drone/docker-compose.yml

```
[root@SPLAWSSREDKR01 drone]# cat /docker/drone/docker-compose.yml
version: "3.7"

services:
  drone:
    container_name: drone
    image: drone/drone
    restart: unless-stopped
    environment:
      - DRONE_BITBUCKET_CLIENT_ID=w7QLNK7Rj54UaSgfMD
      - DRONE_BITBUCKET_CLIENT_SECRET=CPwGFmtS8AtckBwSxWs4CVtcWhTYPgHN
      - DRONE_RPC_SECRET=38f6fd4705dbac9bbb6ca116b23f0e35
      - DRONE_SERVER_HOST=drone.sre.compucom.com
      - DRONE_SERVER_PROTO=https
      - DRONE_USER_CREATE=username:ch230427,admin:true
    ports:
      - target: 80
        published: 7001
      - target: 443
        published: 7002
    volumes:
      - /docker/drone/drone-data:/data
  drone-registry:
    container_name: drone-registry
    image: drone/registry-plugin
    restart: unless-stopped
    environment:
      - DRONE_CONFIG_FILE=/config.yml
      - DRONE_DEBUG=true
      - DRONE_SECRET=ec3278d6d9cff082fb109f66984fd76a
    ports:
      - target: 3000
        published: 3001
    volumes:
      - /docker/drone/config.yml:/config.yml
```

More to come ...
---