# docker-redis-server

The idea was to create single **AWS Lightsails Instance** behind  **Load Balancing**, acting as a **Redis** server, to **Broadcast** data to all **socket.io** client, share **Caching Data** from Database to all accross **node** **Adonis** instances.


## 1. Start a new container running Redis

Give the container name **redis-serv** and exposing port **6379**

    docker run -d -p 6379:6379 --name redis-serv redis
To check it running, with

    docker ps
To view log output, with

    docker logs redis-serv

## 2. Running the Redis CLI in the container
