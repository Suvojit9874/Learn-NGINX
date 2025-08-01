<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# NGINX Crash Course Brief

This guide summarizes core NGINX concepts you’ve covered: **static site hosting, reverse proxy, load balancing, and SSL**. For each, you'll find key concepts, practical code/configuration samples, and when and why you’d use these features.

## 1. Static Site Hosting

#### **Why Use?**

- Serve HTML, CSS, JS, and images directly from the server, efficiently and rapidly.
- Simple to deploy, high performance, and minimal resource usage.


#### **When to Use?**

- Personal portfolios, documentation, landing pages, blogs, or any static (non-dynamic) website.


#### **How to Use?**

**Sample `nginx.conf` block:**

```nginx
server {
    listen 80;
    server_name example.com;

    root /var/www/html;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

- Place your static files in `/var/www/html`.
- Restart NGINX after editing config:

```bash
sudo nginx -s reload
```


## 2. Reverse Proxy

#### **Why Use?**

- Forward requests to one or more backend servers (e.g., app servers), hiding internal server details.
- Improve scalability, security, and manageability.


#### **When to Use?**

- Separating frontend and backend (e.g., React frontend and Node.js backend).
- Deploying APIs, microservices, or serving apps from different frameworks/languages.


#### **How to Use?**

**Sample `nginx.conf` block:**

```nginx
server {
    listen 80;
    server_name api.example.com;

    location / {
        proxy_pass http://localhost:3000;  # Backend app
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

- Requests to `api.example.com` are transparently forwarded to your backend server.


## 3. Load Balancer

#### **Why Use?**

- Distribute traffic across multiple servers, ensuring high availability and better resource utilization.
- Prevents any single backend server from becoming overloaded.


#### **When to Use?**

- Applications with high or variable traffic, requiring horizontal scaling for reliability/performance.


#### **How to Use?**

**Sample `nginx.conf` block:**

```nginx
http {
    upstream app_servers {
        server 192.168.1.101;
        server 192.168.1.102;
    }

    server {
        listen 80;
        server_name www.example.com;

        location / {
            proxy_pass http://app_servers;
        }
    }
}
```

- NGINX will forward requests to both `192.168.1.101` and `192.168.1.102` using round-robin by default.


## 4. SSL (HTTPS)

#### **Why Use?**

- Encrypt traffic to protect sensitive data and user privacy.
- Essential for security, required for modern browsers and SEO ranking.


#### **When to Use?**

- Anytime you serve sensitive or personal content.
- Always recommended for public-facing sites.


#### **How to Use?**

**Prerequisite:** Obtain an SSL certificate (e.g., from Let’s Encrypt or a CA).

**Sample `nginx.conf` block:**

```nginx
server {
    listen 443 ssl;
    server_name secure.example.com;

    ssl_certificate     /etc/ssl/certs/example.crt;
    ssl_certificate_key /etc/ssl/private/example.key;

    ssl_protocols       TLSv1.2 TLSv1.3;
    ssl_ciphers         HIGH:!aNULL:!MD5;

    location / {
        root /var/www/html;
        index index.html;
    }
}
```

- Ensure SSL cert/key paths are correct.
- For Let’s Encrypt, you can often use `/etc/letsencrypt/live/domain.com/`.
- **Redirect HTTP to HTTPS**:

```nginx
server {
    listen 80;
    server_name secure.example.com;
    return 301 https://$host$request_uri;
}
```


## Summary Table

| Feature | Why Use? | When to Use? | Key Config Example |
| :-- | :-- | :-- | :-- |
| Static Hosting | Fast, simple hosting | Simple, unchanging sites | `root /var/www/html;` |
| Reverse Proxy | Security, flexibility | Fronting backend services via single endpoint | `proxy_pass http://localhost:3000;` |
| Load Balancer | Scale, redundancy | High/variable traffic, need for uptime | `upstream app_servers { ... }` |
| SSL | Secure communications | Any site, especially those with login/data entry | `listen 443 ssl;` + cert paths |

**Tip:**
You often combine these features—for example, hosting a static frontend, using reverse proxy to backend APIs, secured by SSL, and load balancing between multiple backend servers. Always test your config and reload NGINX after making changes.

This overview should help you recall what you've learned and apply NGINX confidently in various scenarios.


## NGINX Load Balancer Keywords: `backup`, `weight`, and More

### 1. `backup`

- **Purpose:** Marks a server as a backup within an `upstream` block.
- **How it works:** The backup server only receives traffic if all primary (non-backup) servers are unavailable or down. This is useful for **high availability** and **failover** scenarios.
- **Example:**

```nginx
upstream myapp {
    server app1.example.com;
    server app2.example.com;
    server backup1.example.com backup;
}
```

In this example, `backup1.example.com` will not receive requests unless both `app1` and `app2` are down[^2_1][^2_2].


### 2. `weight`

- **Purpose:** Controls traffic distribution—servers with a higher weight receive more requests.
- **Example:**

```nginx
upstream myapp {
    server app1.example.com weight=5;
    server app2.example.com weight=1;
}
```

Here, `app1` will receive five times as many requests as `app2`[^2_1].


### 3. Other Parameters (`max_fails`, `fail_timeout`, `down`)

- **max_fails:** Number of failed attempts before marking a server as unavailable.
- **fail_timeout:** Time to consider a server unavailable after failures.
- **down:** Marks a server as permanently down (during maintenance or draining traffic).


## Canary Deployment with NGINX

**Canary Deployments** allow you to send a small percentage of traffic to a new (canary) version of your app while the majority continues to use the stable version.

### How to Do It:

- You define multiple upstreams (e.g., one for stable, one for canary).
- Use NGINX's `split_clients` directive to route a specific percentage of requests to the canary version.

**Example:**

```nginx
http {
    split_clients "${remote_addr}" $upstream_name {
        95%     "stable";
        5%      "canary";
    }

    upstream stable {
        server stable.example.com;
    }

    upstream canary {
        server canary.example.com;
    }

    server {
        listen 80;
        server_name app.example.com;
        location / {
            proxy_pass http://$upstream_name;
        }
    }
}
```

This sends 5% of users to the canary deployment and 95% to the stable one, great for testing new features with real traffic while controlling risk[^2_3].

## Blue-Green Deployment with NGINX

**Blue-Green Deployments** use two identical environments (Blue and Green): one is live, the other is staged for the next release.

### How to Do It:

- Deploy the new version in the idle environment (say, Green while Blue is live).
- Once tested, switch NGINX config to route all production traffic to the new environment by updating your upstream or proxy rules and then reload NGINX.
- If problems arise, revert by pointing traffic back to the previous (Blue) environment.

**Simplified Example:**

```nginx
upstream app_backend {
    server blue.example.com;  # Initially active
    # To switch, change to server green.example.com;
}
# ...in your server block
location / {
    proxy_pass http://app_backend;
}
```

Change the `upstream` to point to the "green" environment and reload NGINX to cut over all traffic. Automation scripts can make this a zero-downtime and quick switch strategy[^2_4][^2_5][^2_6][^2_7].

### **Summary Table**

| Keyword/Strategy | Use-Case | Example Use |
| :-- | :-- | :-- |
| `backup` | Failover to backup servers | `server backend backup;` |
| `weight` | Traffic distribution weighting | `server backend weight=5;` |
| Canary Deployment | Gradual rollout of new version | `split_clients ... {}` |
| Blue-Green Deployment | Zero-downtime cut-over between two environments | Switch upstream or server |

**Tip:** Use `backup` for reliability, `weight` for load shaping, and NGINX config switching (`split_clients`, upstream edits) to achieve safer, faster deployments with minimal downtime[^2_1][^2_2][^2_3][^2_4][^2_5][^2_6].

If you want full config samples for each, just ask!

<div style="text-align: center">⁂</div>

[^2_1]: https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/

[^2_2]: http://nginx.org/en/docs/http/ngx_http_upstream_module.html

[^2_3]: https://www.linkedin.com/pulse/canary-deployment-nginx-k8s-reza-asadi-k6rue

[^2_4]: https://github.com/kaalpanikh/blue-green-deployment

[^2_5]: https://dev.to/agusrdz/blue-green-deployment-in-a-local-environment-with-docker-43j9

[^2_6]: https://semaphore.io/blog/blue-green-deployment-nodejs

[^2_7]: https://dev.to/justincy/blue-green-node-js-deploys-with-nginx-bkc

[^2_8]: http://nginx.org/en/docs/http/load_balancing.html

[^2_9]: https://docs.nginx.com/nginx/admin-guide/load-balancer/tcp-udp-load-balancer/

[^2_10]: https://upcloud.com/resources/tutorials/configure-load-balancing-nginx/

[^2_11]: https://docs.nginx.com/nginx-instance-manager/admin-guide/maintenance/backup-and-recovery/

[^2_12]: https://www.stakater.com/post/setup-nginx-ingress-controller-to-implement-canary-releases

[^2_13]: https://antmedia.io/docs/guides/clustering-and-scaling/load-balancing/nginx-load-balancer/

[^2_14]: https://kubernetes.github.io/ingress-nginx/examples/canary/

[^2_15]: https://www.linkedin.com/pulse/ultimate-guide-high-availability-load-balancing-nginx-govind-yadav-x1ctf

[^2_16]: https://devtron.ai/blog/how-to-execute-canary-deployments-using-nginx-ingress/

[^2_17]: https://coderpad.io/blog/development/how-to-configure-different-load-balancing-algorithms-on-nginx/

[^2_18]: https://docs.byteplus.com/en/docs/vke/Implementing-canary-and-blue-green-deployments-using-an-Nginx-Ingress

[^2_19]: https://techannotation.wordpress.com/2019/12/13/blue-green-canary-deployment-with-nginx/

[^2_20]: https://serverfault.com/questions/722384/nginx-upstream-and-the-backup-parameter
