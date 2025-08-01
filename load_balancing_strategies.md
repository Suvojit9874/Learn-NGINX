<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# write all the load balancing strategies

Here’s a comprehensive overview of **load balancing strategies** commonly used in modern infrastructure. These strategies determine how incoming requests are distributed across multiple servers or resources to optimize performance, maximize resource utilization, and ensure high availability.

## Static Load Balancing Strategies

These do not consider the real-time load on servers:

- **Round Robin**
    - Requests are distributed equally and sequentially across all servers.
- **Weighted Round Robin**
    - Each server is assigned a static weight based on capacity or preference. Servers with higher weights get more requests proportionally.
- **IP Hash**
    - The load balancer uses a hash of the client’s IP address (or a combination of source and destination IPs) to consistently route a specific client to the same server—aids in session persistence.
- **Random**
    - Requests are assigned to servers randomly, without considering any server state[^1][^2][^3].


## Dynamic Load Balancing Strategies

These consider real-time server state and adapt based on load and performance:

- **Least Connections**
    - New requests go to the server with the fewest active connections at that moment—ideal for uneven workloads or varying connection times.
- **Weighted Least Connections**
    - Like Least Connections, but also considers each server’s capacity (weight), allowing stronger servers to handle more connections.
- **Least Response Time**
    - Directs traffic to the server with the quickest response or best recent performance.
- **Resource-Based**
    - Allocates requests according to current server resource usage (CPU, memory), often requiring real-time health checks or monitoring agents.
- **Custom Load**
    - Administrators define custom metrics or logic (such as CPU, disk I/O, or application-specific metrics) to route traffic as needed[^4][^5][^2].


## Specialized Load Balancing Approaches

- **Priority-Based**
    - Requests are always sent to the highest-priority server; others act as hot standbys and are used only if higher-priority servers fail[^6].
- **URL Hash/Path-Based**
    - Requests are routed based on specific HTTP paths or URLs, sometimes called application layer (L7) routing.
- **DNS-Based**
    - DNS responses rotate records or use geo/location-based selection to spread load globally.
- **Global Server Load Balancing (GSLB)**
    - Directs users to the nearest or best-performing data center across regions for improved latency and availability[^7][^8].


### Summary Table

| Strategy | Category | Key Use/Criterion |
| :-- | :-- | :-- |
| Round Robin | Static | Simple, even traffic distribution |
| Weighted Round Robin | Static | Distribute per server capacity |
| Random | Static | Random, even or unpredictable traffic |
| IP Hash | Static | Session persistence, user stickiness |
| Least Connections | Dynamic | Real-time server load, fewest connections |
| Weighted Least Conn. | Dynamic | Least loaded, considers server weights |
| Least Response Time | Dynamic | Fastest responding server |
| Resource-Based | Dynamic | CPU/memory/disk usage |
| Priority-Based | Specialized | Failover, “hot spare” servers |
| Custom | Specialized | Tailored to custom metrics |
| URL/Path-Based Routing | Specialized | Layer 7, directs specific URIs |
| DNS/GSLB | Specialized | Multi-region, geo-aware balancing |

Choosing the right load balancing strategy depends on your workload patterns, server heterogeneity, session persistence requirements, and real-time performance goals[^1][^4][^5].

<div style="text-align: center">⁂</div>

[^1]: https://kemptechnologies.com/load-balancer/load-balancing-algorithms-techniques

[^2]: https://www.designgurus.io/course-play/grokking-system-design-fundamentals/doc/load-balancing-algorithms

[^3]: https://lavellenetworks.com/blog/8-load-balancing-techniques-you-should-know/

[^4]: https://www.geeksforgeeks.org/system-design/load-balancing-algorithms/

[^5]: https://aws.amazon.com/what-is/load-balancing/

[^6]: https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/load-balancing-overview

[^7]: https://www.cloudflare.com/learning/performance/types-of-load-balancing-algorithms/

[^8]: https://www.f5.com/glossary/load-balancer

[^9]: https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/

