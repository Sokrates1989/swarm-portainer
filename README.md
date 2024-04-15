# swarm-portainer
Install portainer on multi-manager docker swarm using glusterfs

Minor adaptions from the [main tutorial](https://docs.portainer.io/start/install-ce/server/swarm/linux) to make it work with glusterfs

# Info
The default implementation from https://docs.portainer.io/start/install-ce/server/swarm/linux uses a volume that is created internally. This would lead to a fresh re-install, if it were re-deployed. According to their documentation, this happens quite often.

To address this issue, we simply map all portainer_data to a gluster volume. Therefore the data will persist on any re-deployment accross all manager nodes.


# First Setup

##### Setup repo at desired location

```bash
# Choose location on server (glusterfs when using multiple nodes is recommended).
mkdir -p /gluster_storage/swarm/monitoring/portainer
cd /gluster_storage/swarm/monitoring/portainer
git clone https://github.com/Sokrates1989/swarm-portainer.git .
```

##### Configuration
```bash
# Copy ".env.template" to ".env".
cp .env.template .env

# Edit .env
vi .env
```

# Deploy
```bash
docker stack deploy -c <(docker-compose config) portainer
```