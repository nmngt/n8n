#
# n8n app
#

Starts n8n with Traefik, PostgreSQL and redis on Ubuntu 24.04.

## Getting started

init new virtual server at hetzner cloud, set your ssh key and login as root

1. Init env
```
apt update && sudo apt upgrade -y
dpkg-reconfigure tzdata
```

2. Install docker
```
apt install apt-transport-https ca-certificates curl software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
apt update
apt install docker-ce docker-ce-cli containerd.io -y
systemctl start docker
systemctl enable docker
systemctl status docker
```

3. instal ufw
```
apt install ufw
ufw enable
ufw allow ssh
ufw allow https
ufw allow http
```

4. add n8n user
```
adduser n8n
usermod -aG sudo n8n
```

5. setup directories
```
cd /srv
git clone https://github.com/nmngt/n8n.git n8n
cd n8n
```

6. edit .env
```
cp .env.sample .env
vi .env
```

7. start env
```
docker compose up -d
```

To view logs use:
```
docker logs n8n
docker logs postgres
docker logs traefik
docker logs redis
```

## Configuration

The default name of the database, user and password for PostgreSQL can be changed in the [`.env`](.env) file in the current directory.
