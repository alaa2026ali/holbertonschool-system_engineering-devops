# 2. Secured and Monitored Web Infrastructure

## 1. Infrastructure Overview

This infrastructure hosts `www.foobar.com` using HAProxy, two application servers, and a MySQL Primary database.

It includes three firewalls, an SSL certificate for HTTPS, and three monitoring clients that collect infrastructure data and send it to a monitoring platform such as Sumo Logic.

## 2. How It Works

1. The user accesses `https://www.foobar.com`.
2. The first firewall allows authorized incoming HTTPS traffic and blocks unauthorized connections.
3. HAProxy receives the HTTPS request and uses the SSL certificate to establish an encrypted connection with the client.
4. HAProxy distributes requests between the two application servers.
5. Each application server has a firewall that restricts access to authorized ports and services.
6. Nginx receives requests and forwards dynamic requests to the application server.
7. The application server communicates with the MySQL Primary database for database operations.
8. Three monitoring clients collect metrics and logs from the infrastructure and send the data to the monitoring platform.
9. The monitoring platform analyzes the collected data and can generate alerts when configured thresholds or conditions are met.

## 3. Why These Additional Elements Are Added

- **Three Firewalls:** Firewalls control network traffic and help prevent unauthorized access to the load balancer, application servers, and database.
- **SSL Certificate:** The certificate enables HTTPS and helps the browser verify the website's identity.
- **HTTPS:** HTTPS encrypts traffic between the browser and the HTTPS endpoint, protecting sensitive information from being read or modified in transit.
- **Three Monitoring Clients:** Monitoring clients collect data from the infrastructure components and forward it to a centralized monitoring service.
- **Monitoring Platform:** A service such as Sumo Logic centralizes logs and metrics, helps identify errors and performance issues, and supports alerting.

## 4. What Are Firewalls For?

Firewalls filter network traffic according to configured security rules. They allow authorized connections and block unauthorized ones.

For example:
- The public firewall allows HTTPS traffic on port 443.
- Application server firewalls allow traffic from authorized sources, such as HAProxy.
- Database access is restricted to authorized application servers and administrative connections.

## 5. Why Is Traffic Served Over HTTPS?

HTTPS uses TLS encryption to protect data transmitted between the browser and the website's HTTPS endpoint.

It provides:
- **Confidentiality:** Prevents unauthorized parties from reading encrypted traffic.
- **Integrity:** Helps detect unauthorized modification of traffic.
- **Authentication:** Allows the browser to verify the server's identity using its certificate.

## 6. How Monitoring Works

Monitoring collects and analyzes information about the health, performance, and availability of infrastructure components.

The monitoring clients collect metrics and logs from servers and services. They send this data to a centralized monitoring platform, such as Sumo Logic, where it can be searched, analyzed, visualized, and used to trigger alerts.

Examples of monitored information include CPU usage, memory usage, disk usage, application errors, request rates, and response times.

### Monitoring Web Server QPS

To monitor web server QPS (Queries Per Second), configure Nginx to expose or record request counts and collect the relevant metrics using a monitoring agent or exporter.

Calculate QPS by counting the requests received during a time interval and dividing the count by the number of seconds in that interval.

For example, if Nginx receives 600 requests in 60 seconds:

QPS = 600 / 60 = 10 requests per second.

A monitoring dashboard can display QPS over time and generate alerts when request rates exceed configured thresholds.

## 7. Infrastructure Issues

### SSL Termination at the Load Balancer

If SSL/TLS terminates at HAProxy, the connection between the browser and HAProxy is encrypted. However, traffic between HAProxy and the backend servers may be unencrypted if HTTP is used.

This can expose sensitive information inside the network. To protect backend traffic, use HTTPS between HAProxy and the backend servers as well, or use another appropriately secured encrypted connection.

The load balancer also becomes responsible for managing the certificate and its private key.

### Only One MySQL Server Accepts Writes

The Primary database is a single point of failure for write operations. If it fails, writes may become unavailable until recovery or failover occurs.

A replicated database and a properly configured failover mechanism can improve availability, but replication alone does not guarantee automatic failover or prevent data loss.

### All Servers Have the Same Components

Running the database, web server, and application server on every server can cause several problems:

- **Resource contention:** The database and application compete for CPU, memory, disk, and other resources.
- **Complexity:** Database replication, consistency, and configuration become harder to manage.
- **Limited flexibility:** Scaling the application and database independently becomes difficult.
- **Larger failure impact:** A server failure may affect several services at once.

Separating the web, application, and database tiers can make scaling, security, maintenance, and resource management easier.

## 8. Diagram

[[[[Insert the URL of the infrastructure diagram here.]
](https://github.com/alaa2026ali/holbertonschool-system_engineering-devops/blob/main/web_infrastructure_design/2-secured_and_monitored_web_infrastructure.drawio.png)](https://github.com/alaa2026ali/holbertonschool-system_engineering-devops/blob/main/web_infrastructure_design/3-scale_up.drawio.png)
](https://github.com/alaa2026ali/holbertonschool-system_engineering-devops/blob/main/web_infrastructure_design/2-secured_and_monitored_web_infrastructure.drawio.png)
