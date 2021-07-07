

## First time set up

```

mkdir -p /docker/letsencrypt-docker-nginx/src/letsencrypt/

cd /tmp; git clone https://github.com/pingzh/nginx-docker-cert-setup.git

sudo cp -r /tmp/nginx-docker-cert-setup/letsencrypt/* /docker/letsencrypt-docker-nginx/src/letsencrypt/

cd /docker/letsencrypt-docker-nginx/src/letsencrypt/
sudo vim nginx.conf # update line 4

sudo docker-compose up

sudo docker run -it --rm \
  -v /docker-volumes/etc/letsencrypt:/etc/letsencrypt \
  -v /docker-volumes/var/lib/letsencrypt:/var/lib/letsencrypt \
  -v /docker/letsencrypt-docker-nginx/src/letsencrypt/letsencrypt-site:/data/letsencrypt \
  -v "/docker-volumes/var/log/letsencrypt:/var/log/letsencrypt" \
  certbot/certbot certonly --webroot \
  --agree-tos --no-eff-email --webroot-path=/data/letsencrypt \
  --email <your-email> \
  -d <your-domain-that-points-to-the-host>
```

## Renew

```
cd /docker/letsencrypt-docker-nginx/src/letsencrypt
# Stop any running container on the host

sudo rm -rf /docker-volumes/ && cd /docker/letsencrypt-docker-nginx/src/letsencrypt && sudo docker-compose up

sudo docker run -it --rm \
  -v /docker-volumes/etc/letsencrypt:/etc/letsencrypt \
  -v /docker-volumes/var/lib/letsencrypt:/var/lib/letsencrypt \
  -v /docker/letsencrypt-docker-nginx/src/letsencrypt/letsencrypt-site:/data/letsencrypt \
  -v "/docker-volumes/var/log/letsencrypt:/var/log/letsencrypt" \
  certbot/certbot certonly --webroot \
  --agree-tos --no-eff-email --webroot-path=/data/letsencrypt \
  --email <your-email> \
  -d <your-domain-that-points-to-the-host>
```
