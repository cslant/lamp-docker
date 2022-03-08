# PYL Docker



## Steps

- copy PYL source code to folder **codebase**
- modify **.env**
- run `docker-compose up -d`

## Check the network ID of DB

### Option 1:

- run `docker ps` to check the Container ID of **paintyourlife-db**
- run the command `docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container ID>`
- use the output ID in the **main.php**

### Option 2:

- use the string `db_server` as the host in the **main.php**

