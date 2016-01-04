# Docker Postgres with SSL Certificate
This repo is for running a Docker postgres image with SSL based on the library
[postgres 9.5 image](https://github.com/docker-library/postgres).

## Build
```
docker pull nimbustech/postgres-ssl:9.5 
```

## Use

 1. First get postgres up and running (replace `$PG_DATA` and '`demo'` as required):
        
        docker run --rm --name psql -e POSTGRES_DB='demo' -e POSTGRES_PASSWORD='password' nimbustech/postgres-ssl:9.5
        
 2. Then copy your `server.crt` and `server.key` files to `/my/cert/folder`. You must make sure that the ownership
  and permisions are correct, typically by running the following *in the host*:
  
        sudo chown 999.docker *
        sudo chmod 600 server.key
 
 3. You can configure postgres to use your
    certificates with:
   
        docker run --name psql -d -v /my/cert/folder:/var/ssl -e POSTGRES_PASSWORD='password' nimbustech/postgres-ssl:9.5
        
         
 3. Then connect with the proper `sslmode` parameter that your client uses to connect to postgres.
([libpq docs](http://www.postgresql.org/docs/9.4/static/libpq-connect.html#LIBPQ-CONNECT-SSLMODE))
    * disable - **will not use ssl**
    * allow - **will revert to non-ssl mode with an outdated cert**
    * prefer - **will revert to non-ssl mode with an outdated cert**
    * require - **will fail with an outdated cert**
    * verify-ca - **will fail with an outdated cert**
    * verify-full- **will fail with an outdated cert**

```
PGSSLMODE="prefer"  psql -h xxx.xxx.xxx.xxx -U postgres -d dbname
```

## MIT License
See the `LICENSE` file for full information.
