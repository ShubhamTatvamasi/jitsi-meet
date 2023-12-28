# jitsi-meet

https://jitsi.github.io/handbook/docs/devops-guide/devops-guide-docker

https://github.com/jitsi/docker-jitsi-meet/releases/latest

Download latest release:
```bash
wget https://github.com/jitsi/docker-jitsi-meet/archive/refs/tags/stable-9111.tar.gz
```

Extract:
```bash
tar -zxvf stable-9111.tar.gz
```

Create `.env` file and set random passwords:
```bash
cd docker-jitsi-meet-stable-9111
cp env.example .env
./gen-passwords.sh
```

Update these values in `.env` file:
```
HTTP_PORT=80
HTTPS_PORT=443
```


