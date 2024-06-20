# jitsi-meet

https://jitsi.github.io/handbook/docs/devops-guide/devops-guide-docker

https://github.com/jitsi/docker-jitsi-meet/releases/latest

Download latest release:
```bash
wget https://github.com/jitsi/docker-jitsi-meet/archive/refs/tags/stable-9457-2.tar.gz
```

Extract:
```bash
tar -zxvf stable-9457-2.tar.gz
```

Create `.env` file and set random passwords:
```bash
cd docker-jitsi-meet-stable-9457-2
cp env.example .env
./gen-passwords.sh
```

We have to open port `80/tcp`,`443/tcp`,`10000/udp`

Update these values in `.env` file:
```bash
HTTP_PORT=80
HTTPS_PORT=443
PUBLIC_URL=https://meet.example.com

ENABLE_LETSENCRYPT=1
LETSENCRYPT_DOMAIN=meet.example.com
LETSENCRYPT_EMAIL=info@example.com

RESTART_POLICY=unless-stopped

# This is if we are running behind NAT
# Private IP: 192.168.1.1
# Public IP: 1.2.3.4
JVB_ADVERTISE_IPS=192.168.1.1,1.2.3.4
```

Create directories:
```bash
mkdir -p ~/.jitsi-meet-cfg/{web,transcripts,prosody/config,prosody/prosody-plugins-custom,jicofo,jvb,jigasi,jibri}
```

Start docker compose:
```bash
docker-compose up -d
```

### Custom Build

Put `assetlinks.conf` file at this location in the repo: 
```
ls web/rootfs/defaults/assetlinks.conf
ls ~/.jitsi-meet-cfg/web/nginx/assetlinks.conf

ls web/rootfs/defaults/apple-app-site-association.conf
ls ~/.jitsi-meet-cfg/web/nginx/apple-app-site-association.conf
```

include `assetlinks.conf` in `default` file in 443 server block:
```bash
vim web/rootfs/defaults/default
```

```
	include /config/nginx/assetlinks.conf;
        include /config/nginx/apple-app-site-association.conf;
```


Put your `assetlinks.json` file at this location:
```
ls ~/.jitsi-meet-cfg/web/assetlinks.json
ls ~/.jitsi-meet-cfg/web/apple-app-site-association
```

Build docker image:
```bash
cd web
docker build -t jitsi/web:stable-9457-2 --build-arg BASE_TAG=stable-9457-2 .
```

