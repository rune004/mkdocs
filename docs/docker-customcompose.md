# Custom Docker-compose

## Portainer Templates

### Seafile 
```
version: '2.0'
services:
  db:
    image: mariadb
    container_name: seafile-mysql
    environment:
      - MYSQL_ROOT_PASSWORD=*
      - MYSQL_LOG_CONSOLE=true
    volumes:
      - ./data/mariadb:/var/lib/mysql
    networks:
      - seafile-net

  memcached:
    image: memcached
    container_name: seafile-memcached
    entrypoint: memcached -m 256
    networks:
      - seafile-net
          
  seafile:
    image: seafileltd/seafile-mc
    container_name: seafile
    ports:
      - "8080:80"
    volumes:
      - ./data/app:/shared
    environment:
      - DB_HOST=db
      - DB_ROOT_PASSWD=*
      - TIME_ZONE=Etc/UTC
      - SEAFILE_ADMIN_EMAIL=*
      - SEAFILE_ADMIN_PASSWORD=*
      - SEAFILE_SERVER_LETSENCRYPT=false
      - SEAFILE_SERVER_HOSTNAME=*
    depends_on:
      - db
      - memcached
    networks:
      - seafile-net

networks:
  seafile-net:
```

### Vaultwarden 
```
version: '3.4'
services:
  vaultwarden:
    image: vaultwarden/server:latest
    environment:
      - ADMIN_TOKEN=*
      - SIGNUPS_ALLOWED=false
    volumes:
      - ./vw_data:/data
    ports:
      - 17881:80
```

### Flamedashboard
```
version: '2.1'
services:
  flame:
    image: pawelmalak/flame:latest
    container_name: flame
    volumes:
      - /srv/config/flame:/app/data
    ports:
      - 5005:5005
    environment:
       - PASSWORD=*
    restart: unless-stopped
```

