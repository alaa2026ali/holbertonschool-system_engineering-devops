# 3. Scale Up

## Overview

This infrastructure is designed to improve scalability, performance, and availability by separating the web server, application server, and database onto dedicated servers. It also uses two HAProxy load balancers configured as a high-availability cluster.

## Infrastructure Components

1. **Two HAProxy Load Balancers:** The two load balancers form a high-availability cluster. If one load balancer fails, the other can take over, provided that failover is properly configured.

2. **Web Server:** A dedicated Nginx server handles HTTP/HTTPS requests and serves static content such as images, CSS, and JavaScript files. Separating the web server reduces resource competition.

3. **Application Server:** A dedicated server runs the application code and processes dynamic requests. This allows application resources to be managed and scaled independently of the web server.

4. **Database Server:** A dedicated MySQL server stores application data. Separating the database improves resource management, security isolation, and database performance.

5. **DNS:** The DNS record for `www.foobar.com` directs users to the public entry point of the infrastructure, which must be connected to the HAProxy cluster.

## How It Works

1. A user requests `www.foobar.com`.
2. DNS resolves the domain to the public entry point.
3. The active HAProxy load balancer receives the request and forwards it to the web server.
4. Nginx serves static content or forwards dynamic requests to the application server.
5. The application server processes the request and communicates with the MySQL database when data is required.
6. The response returns to the user through the web server and load balancer.

## Why These Additional Elements Are Needed

- **Second HAProxy load balancer:** Improves availability and removes the single-load-balancer failure point when automatic failover is configured.
- **Dedicated web server:** Handles web traffic and static files without competing directly with application and database workloads.
- **Dedicated application server:** Runs business logic independently, making it easier to maintain and scale the application.
- **Dedicated database server:** Isolates database workloads and allows database resources and access controls to be managed separately.

## Conclusion

This scaled-up infrastructure separates the main application components and introduces load-balancer redundancy. It improves maintainability, resource utilization, and availability. However, the web server, application server, and database server can still be single points of failure unless additional redundancy is implemented for those components.

## Diagram
https://github.com/alaa2026ali/holbertonschool-system_engineering-devops/blob/main/web_infrastructure_design/3-scale_up.drawio.png
