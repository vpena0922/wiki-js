# Traefik

Today, we’re going to use SSL for everything.No more self-sign certs.No more http.No more hosting things on odd ports.We’re going all in with SSL for our internal services and our external services too.We going to set up a reverse proxy using Traefik, Portainer, and use that to get wildcard certificates from Let’s Encrypt. Join me and let’s secure all the things.

📺 [Watch Video](https://www.youtube.com/watch?v=n1vOfdz5Nm8)

> Looking to do this same thing in Kubernetes? Check out traefik + cert-manager on Kubernetes
> 

## Docker Setup

See [this post](https://technotim.live/posts/docker-compose-install/) on how to install `docker` and `docker-compose`

## Traefik

```bash
mkdir traefik
cd traefik
mkdir data
cd data
touch acme.json
chmod 600 acme.json
touch traefik.yml
```

`traefik.yml` can be found [here](https://github.com/techno-tim/techno-tim.github.io/tree/master/reference_files/traefik-portainer-ssl/traefik)

create docker network

```bash
docker network create proxy
```

```bash
touch docker-compose.yml
```

`docker-compose.yml` can be found [here](https://github.com/techno-tim/techno-tim.github.io/tree/master/reference_files/traefik-portainer-ssl/traefik)

```bash
cd data
touch config.yml
```

```bash
docker-compose up -d
```

## Portainer

```bash
mkdir portainer
cd portainer
touch docker-compose.yml
mkdir data
```

`docker-compose.yml` can be found [here](https://github.com/techno-tim/techno-tim.github.io/tree/master/reference_files/traefik-portainer-ssl/portainer)

### Generate Basic Auth Password

```bash
sudo apt update
sudo apt install apache2-utils
```

```bash
echo $(htpasswd -nb "<USER>" "<PASSWORD>") | sed -e s/\\$/\\$\\$/g
```

NOTE: Replace `<USER>` with your username and `<PASSWORD>` with your password to be hashed.

> If you’re having an issue with your password, it might not be escaped properly and you can use the following command to prompt for your password
> 

```bash
echo $(htpasswd -nB USER) | sed -e s/\\$/\\$\\$/g
```

Paste the output in your `docker-compose.yml` in line (`traefik.http.middlewares.traefik-auth.basicauth.users=<USER>:<HASHED-PASSWORD>`)

### Spin up the container

```bash
docker-compose up -d
```

## Traefik Routes Config

```bash
cd traefik/data
nano config.yml
```

`config.yml` [here](https://github.com/techno-tim/techno-tim.github.io/tree/master/reference_files/traefik-portainer-ssl/traefik)

```bash
docker-compose up -d --force-recreate
```

Your folder structure should look like the below, if you are following along with the example.But feel free to make it however you wish just keep in mind you’ll need to change the location in the corresponding files.

```bash
./traefik
├── data
│   ├── acme.json
│   ├── config.yml
│   └── traefik.yml
└── docker-compose.yml
```