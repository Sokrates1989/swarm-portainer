# swarm-portainer
Install portainer on multi-manager docker swarm using glusterfs

Minor adaptions from the [main tutorial](https://docs.portainer.io/start/install-ce/server/swarm/linux) to make it work with glusterfs

# Info
The default implementation from https://docs.portainer.io/start/install-ce/server/swarm/linux uses a volume that is created internally. This would lead to a fresh re-install, if it were re-deployed. According to their documentation, this happens quite often.

To address this issue, we simply map all portainer_data to a gluster volume. Therefore the data will persist on any re-deployment accross all manager nodes.


# First Setup

### Subdomain

```text
If you want to use traefik:
Make sure that subdomain exist and points to manager of swarm.
For Example: portainer.felicitas-wisdom.de
```


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

##### Option 1: Requires traefik
```bash
# Allows you to access portainer using the url (PORTAINER_URL) provided in .env.
# But requires you to have traefik set up.
docker stack deploy -c <(docker-compose -f config-stack-traefik.yml config) portainer
```

##### Option 2: Does not require traefik, uses default ports
```bash
# You can only call the dashboard using https://<MANAGER_IP_ADDRESS>:9443/.
docker stack deploy -c <(docker-compose -f config-stack.yml config) portainer
```


# Log in
##### Option 1: Using subdomain entered in .env / Requires traefik
- Open Browser and enter https://portainer.felicitas-wisdom.de (the url set for PORTAINER_URL in .env)

##### Option 2: Using IP Address and Port / Does not require traefik
- Retrieve IP Address of -> MANAGER_IP_ADDRESS
- Open Browser and enter https://<MANAGER_IP_ADDRESS>:9443/
- In case of Errors: Ensure, that port 9443 is accessible



# Initial Setup
- [Log in](#log-in)
- Follow instructions at https://docs.portainer.io/start/install-ce/server/setup