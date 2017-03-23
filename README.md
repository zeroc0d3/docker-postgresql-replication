# ZeroC0D3 Docker PostgreSQL Replication

Docker PostgreSQL Master-Slave Replication, run:
```
docker-compose build && docker-compose up
```

## Requirements:
   * [Docker for Linux](https://docs.docker.com/engine/installation/linux/ubuntu)
   * [Docker Toolbox for Windows & Mac](https://www.docker.com/products/docker-toolbox)
   * [Docker Compose](https://docs.docker.com/compose/install/) 
   * [Kitematic](https://kitematic.com/) 

## Configuration:
   * Environment
     - DB_USER=zeroc0d3_user
     - DB_PASS=zeroc0d3_password
     - DB_NAME=zeroc0d3_dbname
   * Using pgAdmin
     - Show running container docker
       ```
       docker ps
       ```
     - Show the IP Address container
       ```
       docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' [container_name_or_id]
       ```
       * Read [this](http://stackoverflow.com/questions/17157721/getting-a-docker-containers-ip-address-from-the-host)
     - Open browser: 
       ```
       http://172.21.0.x:5050
       ```
       * x = IP Address of docker pgadmin
     - Create server (Master/Slave)
     - Connection
       * Hostname: 172.21.0.x
       * Port: 5432
       * Username: zeroc0d3_user
       * Password: zeroc0d3_password

## Multi Slave Replication:
   * Change the [docker-compose-multislave.yml](https://github.com/zeroc0d3/docker-postgresql-replication/blob/master/docker-compose-multislave.yml) to [docker-compose.yml](https://github.com/zeroc0d3/docker-postgresql-replication/blob/master/docker-compose.yml)
   * Rerun the docker compose
     ```
     docker-compose build && docker-compose up --force-recreate
     ```
  
## Optional:
   * Run kitematic & select the docker master/slave container, click on "EXEC" button, or using:
     ```docker exec -it [container_id] bash```
   * Change owner database  
     - Login as postgres:
         ```
         sudo -i -u postgres
         ```
     - Enter psql:
         ```
         psql
         ```
     - Alter database owner:
	 ```
	 ALTER DATABASE zeroc0d3_dbname OWNER TO zeroc0d3_user;
	 ```
   * Change roles user 
	 - Alter for SUPERUSER:
	     ```
	     ALTER ROLE zeroc0d3_user SUPERUSER;
	     ```
	 - Alter for REPLICATION:
	     ```
	     ALTER ROLE zeroc0d3_user REPLICATION;
	     ```

## Check Replication Status:
   * After entering psql (master container):
     ```
     SELECT * FROM pg_stat_replication;
     ```
     - You can read all [here](https://www.enterprisedb.com/blog/monitoring-approach-streaming-replication-hot-standby-postgresql-93)
     
## Common PostgreSQL Command:
   * List of database: ```\l```
   * Connect to database (use database): ```\c [database_name]```
   * View table database (after use): ```\dt```
   * List of user roles: ```\du```
   * Expand display: ```\x```
   * Quit: ```\q```

## License
[MIT License](https://github.com/zeroc0d3/docker-postgresql-replication/blob/master/LICENSE) (MIT)
