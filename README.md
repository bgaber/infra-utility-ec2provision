Purpose?
---
The purpose of this Ansible code is to provision Linux or Windows EC2 instances.

How Is It Installed?
---
No installation is necessary.  This code is an Ansible Playbook launched by [AWX](https://awx.sre.example.com).

How Is It Run?
---
It is launched via an [AWX](https://awx.sre.example.com) Template that links to the main.yml Playbook in this repo.

Required Resources
---
In the .drone.yml file are references to Docker image requirements for 123456789012.dkr.ecr.us-east-1.amazonaws.com/sre-team/imagebuilder:1.2 which is found in our AWS Elastic Container Registry (ECR). ECR is a fully managed Docker container registry.

Drone Configuration
------------------

Drone configuration is found on the Drone server in the /docker/drone directory

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
      - DRONE_BITBUCKET_CLIENT_ID=
      - DRONE_BITBUCKET_CLIENT_SECRET=
      - DRONE_RPC_SECRET=
      - DRONE_SERVER_HOST=drone.sre.example.com
      - DRONE_SERVER_PROTO=https
      - DRONE_USER_CREATE=username:bg216063,admin:true
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
      - DRONE_SECRET=
    ports:
      - target: 3000
        published: 3001
    volumes:
      - /docker/drone/config.yml:/config.yml
```

More to come ...
---