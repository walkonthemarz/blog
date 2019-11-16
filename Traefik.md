### Using Trafik to build you local develop environment

1. Start trafik with docker-compose.yml:

   ```yml
   version: '3'
   
   services:
     reverse-proxy:
       # The official v2.0 Traefik docker image
       image: traefik:v2.0
       # Enables the web UI and tells Traefik to listen to docker
       command: --api.insecure=true --providers.docker
       ports:
         # The HTTP port
         - "80:80"
         # The Web UI (enabled by --api.insecure=true)
         - "8080:8080"
       volumes:
         # So that Traefik can listen to the Docker events
         - /var/run/docker.sock:/var/run/docker.sock
       restart: always
   
   networks:
       default:
           external:
               name: dev
   ```

   Run `docker-compose up`.

2. Start your mysql:

   ```yaml
   version: "2"
   
   services:
     pma_db:
       image: mysql:8
       #command: '--default-authentication-plugin=mysql_native_password --sql-mode="" --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci'
       command: '--default-authentication-plugin=mysql_native_password --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci'
       environment:
         MYSQL_ROOT_PASSWORD: g3nt00567
       volumes:
         #- /data/mysql:/var/lib/mysql
         #- /data/mysql56:/var/lib/mysql
         #- /home/setup/data/mysql/mysql57:/var/lib/mysql
         - $PWD/mysql8:/var/lib/mysql
           #ports:
           #  - "33060:3306"
       restart: always
     pma:
       image: phpmyadmin/phpmyadmin:latest
       #ports:
       #  - "8000:80"
   #   networks:
   #      - xxxx
       depends_on:
         - pma_db
       environment:
         - PMA_ARBITRARY=0
         - PMA_HOSTS=pma_db
         #- PMA_USER=root
         #- PMA_PASSWORD=xxxx
       restart: always
       labels:
           - "traefik.enable=true"
           - "traefik.docker.network=dev"
           - "traefik.http.routers.pma.rule=Host(`pma.docker.localhost`)"
   
   networks:
     default:
       external:
         name: dev
   ```

   Just add the **labels** for traefik to discover the http service.

   Start mysql container: `docker-compose run`, manage your mysql via browser with url [http://pma.docker.localhost](http://pma.docker.localhost).

   Done.

3. CONNECT FROM A CONTAINER TO A SERVICE ON THE HOST:see [https://docs.docker.com/docker-for-mac/networking/](https://docs.docker.com/docker-for-mac/networking/)

   `host.docker.internal` or `gateway.docker.internal` will direct to the host(for current now works on Mac/Windows, not working on Linux, See [https://stackoverflow.com/questions/24319662/from-inside-of-a-docker-container-how-do-i-connect-to-the-localhost-of-the-mach](https://stackoverflow.com/questions/24319662/from-inside-of-a-docker-container-how-do-i-connect-to-the-localhost-of-the-mach)). 