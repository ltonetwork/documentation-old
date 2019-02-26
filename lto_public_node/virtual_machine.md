## Run on a (virtual) machine

```
docker-compose up
```
    
Docker compose is configured to run the node on a local machine on port 80. If you would like to run the node on different
port you will need to change the `docker-compose.yml` to

```
ports:
    - <your-port>:6869
```

This way the node will be accessible via port 80.

Or you can use a reverse proxy like NGINX to make the node publicly available. This is highly recommended. 
