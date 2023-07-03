# Nginx Proxy Manger
Docker Version

## Installations Anleitung

Nginx Proxy Manager Docker Version.

### Voraussetzungen
    OS: Ubuntu 22.04
    CPU: 2 Kerne
    RAM: 1024 MB
    HDD: 10 GB ( Geht auch Kleiner Minimal 5GB)
    NET: 1x VMBR(X) / Statische IP Adresse

### Docker Installation
```bash
apt update && apt upgrade -y
apt install curl -y
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
mkdir -p ~/.docker/cli-plugins/

```

### Docker Compose Installation
```bash
curl -SL https://github.com/docker/compose/releases/download/v2.2.2/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
chmod +x ~/.docker/cli-plugins/docker-compose
apt install docker-compose
```
    
### Portainer Installation
```bash
docker volume create portainer_data
docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```


## Benutzer Anmelde Daten
Jetzt Musst du dich nur noch einmal Anmelden dann hast du den Teil mit Portainer erledigt.

Docker Anmeldung:

- URL: http://DEINE-VM-IP-ADRESSE:9000/
- Benutzer: admin
- Passwort: MUST DU DANN SETZEN



### Vorbereitung der Nginx Proxy Manager daten

```bash
# Wir Erstellen das Verzeichniss
mkdir /root/voelk-it 

# Nun ein Weiteres Verzeichniss
mkdir /root/voelk-it/docker

# Jetzt erstellen wir das Verzeichniss für den Nginx Proxy Manager
mkdir /root/voelk-it/docker/nginx-proxy-manager

# Jetzt kommt die Magie wir erstellen eine date
nano docker-compose.yml
```
#### docker-compose.yml

```bash
version: '3.8'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '443:443'
      - '81:81'
    environment:
      DB_MYSQL_HOST: "db"
      DB_MYSQL_PORT: 3306
      DB_MYSQL_USER: "nginx-proxy-manager-user"
      DB_MYSQL_PASSWORD: "Q84f91$CrCzy"
      DB_MYSQL_NAME: "nginx-proxy-manager-db"
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
    depends_on:
      - db

  db:
    image: 'jc21/mariadb-aria:latest'
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: 'xX#3N12V0XQR'
      MYSQL_DATABASE: 'nginx-proxy-manager-db'
      MYSQL_USER: 'nginx-proxy-manager-user'
      MYSQL_PASSWORD: 'Q84f91$CrCzy'
    volumes:
      - ./mysql:/var/lib/mysql
```

#### Nginx Proxy Manager Starten

```bash
cd /root/voelk-it/docker/nginx-proxy-manager
docker-compose up -d
```

#### Standart Administratoren Benutzer
```bash
Email:    admin@example.com
Passwort: changeme
```
## Verfasser

- [@Voelk-IT](https://www.github.com/Voelk-IT)

