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
use **-it** to run **redis-cli** via shell **sh**

    docker exec -it redis-serv sh
now we can start run **redis-cli**

    # redis-cli
    127.0.0.1:6379>


## 3. Basic Redis command
run Ping command

    127.0.0.1:6379> ping
    PONG
try to save same key value

    127.0.0.1:6379> set firstname John
    OK
    127.0.0.1:6379> set lastname Doe
    OK
and now exit out

    127.0.0.1:6379> exit
    # exit

 ## 4. Create another container, as client and linked to redis-serv
 New container name is **client-1**, copied from **redis** image, but this client will not running **Redis** itself . So we ask it to run shell **sh** in **-it** mode
 **--rm** will delete itself after the shell exits.

    docker run -it --rm --link redis-serv:redis --name client-1 redis sh
    
as **client**, connect it to **redis sever** via **redis-cli**

    # redis-cli -h redis
    redis:6379>

Now we tried to call last two keys that we save earlier 

    redis:6379> get firstname
    "John"
    redis:6379> get lastname
    "Doe"
    redis:6379>

on exiting will be destroy **client-1** container

    redis:6379> exit
    # exit


## Clean Up
list all runnning container

    docker ps
stop and remove **redis-serv**

    docker stop redis-serv
    docker rm redis-serv
