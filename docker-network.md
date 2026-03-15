# Docker Networking

## What is Docker Networking

Docker networking allows containers to communicate with each other and with the outside world.

Each container gets its own network interface and IP address.

Docker provides different types of networks.



## Check Docker Networks

docker network ls

Example Output

NETWORK ID     NAME      DRIVER    SCOPE
xxxxx          bridge    bridge    local
xxxxx          host      host      local
xxxxx          none      null      local



## Types of Docker Networks

1 Bridge Network
2 Host Network
3 None Network
4 Custom Network



## Bridge Network (Default)

When a container runs, Docker automatically attaches it to the bridge network.
Example
docker run -d nginx

Check container network
docker inspect container_id



## Host Network

In host network mode, the container shares the host machine's network.

Example
docker run --network host nginx

In this mode, Docker does not create a separate IP for the container.



## None Network

In none network mode, the container has no network access.

Example
docker run --network none nginx
This container cannot communicate with other containers or the internet.


## Create Custom Network

docker network create mynetwork

Check networks
docker network ls
 

## Run Container in Custom Network
docker run -d --network mynetwork --name container1 nginx

Run another container in same network
docker run -d --network mynetwork --name container2 nginx

Now both containers can communicate with each other.



## Inspect Network

docker network inspect mynetwork

This shows containers connected to that network.


## Connect Container to Network

docker network connect mynetwork container_name


## Disconnect Container from Network

docker network disconnect mynetwork container_name



## Remove Network

docker network rm mynetwork



## Key Docker Networking Commands

-docker network ls
-docker network create network_name
-docker network inspect network_name
-docker network connect network_name container
-docker network disconnect network_name container
-docker network rm network_name
