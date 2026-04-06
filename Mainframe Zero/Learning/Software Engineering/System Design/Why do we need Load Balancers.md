# Why do we need Load Balancers?
From [Video](https://www.youtube.com/watch?v=chyZRNT7eEo)
Web applications are deployed and hosted on servers. This servers live on hardware machines with finite resources such as memory, processing powers and network connections. As traffic to an application increases, these resources can become limiting factors and prevent the machine from serving requests. This limit is known as the system's capacity. 
At first, some of the scaling problems can be solved by simply increasing the memory or the CPU of the server or by using available resources more efficiently as is the case for something like multi-threading. But eventually solution to this problem would be to add more servers to the system also known as horizontal scaling. When more than one server can be used to serve a request it become necessary to decide which server to send the request to, that's where load balancers come in.
## How do the Load Balancers work?
A good load balancer will efficiently distribute incoming traffic to maximize the system's capacity and minimize the time to fulfill the request. Load Balancers can use serval different strategies: 
- Round robin servers: next server to be assigned guaranteed to be the least recently used.
- Least connections: next server will be the server that currently handling the least number of requests.
- Consistent hashing: similar to database sharding servers can be assigned base on IP address or URL.
- Engineering don't implements their own load balancer but they use an industry standard reverse proxy like AJ proxy or nginx to perform load balancing SSL termination and health checks.
- We need them in distributed computing.
## When and Where do we need Load Balancer
When system would benefits from increased capacity or redundancy. Often Load balancer sits  right between external traffic and the application servers. In microservice architecture it is important to use load balancer in front of each internal service so that every part of system can be scaled independently. 
## What can't be solved by Load Balancing?
* Database performance (slow database queries)
* Algorithmic complexity (inefficient calculation)
* Other type of resource contention  (unreliable third party APIs)
In these cases processing tasks asynchronously such as Job queue may be necessary. 

> [! warning] Keep In Mind
> Load balancing is distinct from Rate limiting when traffic intentionally throttled or dropped to prevent abuse by particular user or organization. 

### Advantages
1. Scalability, make it easier to scale up and down with demands by adding and removing back-end servers
2. Reliability, provides redundancy and can minimize down time by automatically detecting and replacing unhealthy servers
3. Performance, by distributing workload evenly across servers load balancers can improve average response time
### Considerations
- As scale increases Load balancer can become bottle neck or single point of failure so, multiple load balancers must be used to guarantee availability.
- DNS round robin can be used to distribute traffics across multiple load balancers
- Different request can be served using multiple back-end servers, this can be problematic if application uses session data that is not shared across servers.
- Deploying new servers can take longer and require more machines. This is because load balancer needs to roll over traffics to new servers and drain requests from the old machines.