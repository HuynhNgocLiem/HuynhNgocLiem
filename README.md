## 1. Start Kong on local
### a. Install Docker
- On local, we use `Docker` to start `Keycloak`, first of all, we need to install Docker, please follow [this link](https://docs.docker.com/get-started/get-docker/) to install `Docker` to your computer.
- Please make sure your computer has `Docker Compose`, `Docker Desktop` includes Docker Compose along with Docker Engine and Docker CLI which are Compose prerequisites. In some cases, you will need to install it independently.
- 
![image](https://github.com/user-attachments/assets/9c1eda77-c25c-4ccf-ad27-7c827edb816d)

### b. Start Kong
- `Kong Gateway` uses `Postgres` as its database.
- `KongA` is an interface used to manage `Kong Gateway`.
- On local, I prepared `docker-compose.yml` file, after starting `Docker`, you just need to run one command to start both `Postgres` and `Kong Gateway`.

```
docker compose up -d
```

**Note:** DevOps prepared for us `Kong Gateway` Dev/Staging/Production, we can connect to the existing `Kong Gateway`, don't need to run `Kong Gateway` locally.

## 2. Create upstreams
The upstream object represents a virtual hostname and can be used to loadbalance incoming requests over multiple services (targets)
![upstream1](https://github.com/user-attachments/assets/9d7d2d28-8db8-4380-ba9d-7474a64fb5ea)
![upstream2](https://github.com/user-attachments/assets/f6e133b7-145d-4a0a-a275-cb2ca06fae05)

A target is an ip `address/hostname` with a port that identifies an instance of a backend service. Every upstream can have many targets, and the targets can be dynamically added and removed. So changes are effectuated on the fly.
![upstream3](https://github.com/user-attachments/assets/f53f7edf-93f4-4ed6-b7a9-fac7958c159f)
![upstream4](https://github.com/user-attachments/assets/861630e1-1641-40da-b8b0-e44324cc1b36)

## 3. Create service
Service entities, as the name implies, are abstractions of each of your own upstream services. Examples of Services would be a data transformation microservice, a billing API, etc.
- Protocol: The protocol used to communicate with the upstream. It can be one of http or https.
- Host: The host of the upstream server.
- Port: The upstream server port. Defaults to 80.
  
![service1](https://github.com/user-attachments/assets/d31cfea4-8a2c-4021-b397-91be2712b4c3)
![service2](https://github.com/user-attachments/assets/4b4378bc-b27c-49f5-85df-84605bcf19b5)
![service3](https://github.com/user-attachments/assets/8041d171-eb96-4d45-a3e9-4c597789a937)

## 3. Create routes
The Route entities defines rules to match client requests. Each Route is associated with a Service, and a Service may have multiple Routes associated to it. Every request matching a given Route will be proxied to its associated Service.
- Hosts: A list of domain names that match this Route. For example: example.com. At least one of hosts, paths, or methods must be set.
- Paths: Controls how the Service path, Route path and requested path are combined when sending a request to the upstream. See above for a detailed description of each behavior. Accepted values are: "v0", "v1". Defaults to "v1".
- Methods: A list of HTTP methods that match this Route. At least one of hosts, paths, or methods must be set.
- Strip Path: When matching a Route via one of the paths, strip the matching prefix from the upstream request URL.
  
![Routes1](https://github.com/user-attachments/assets/a3af028d-a8a9-457a-88f9-3bc5bb3b2068)
![Routes2](https://github.com/user-attachments/assets/938205ae-e234-4ff4-ac9d-86eb38c63171)
![Routes3](https://github.com/user-attachments/assets/97972200-54b3-4e1c-b11d-9c0a4228e776)
