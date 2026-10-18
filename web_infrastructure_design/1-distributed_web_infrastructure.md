# 1. Distributed Web Infrastructure

## 1. Infrastructure Overview

The infrastructure consists of one HAProxy load balancer and two servers. Each server contains an Nginx web server, an application server, application files, and a MySQL database component.

The two servers support an Active-Active application setup. The databases use a Primary-Replica replication setup.

## 2. How It Works

1. A user enters `www.foobar.com` in their browser.
2. DNS resolves the domain name to the public IP address of the load balancer.
3. HAProxy receives incoming HTTP requests and distributes them between the two servers using the Round Robin algorithm.
4. Nginx receives the request on the selected server and forwards dynamic requests to the application server.
5. The application server executes the application code and processes the request.
6. The application communicates with the MySQL Primary database for write operations. Read operations may be directed to the Replica database.
7. The Primary database replicates its changes to the Replica.
8. The response is returned through Nginx and HAProxy to the user's browser.

## 3. Why These Additional Elements Are Added

- **HAProxy Load Balancer:** Distributes incoming traffic between the two servers, improves request distribution, and helps route traffic away from unhealthy application servers when health checks are configured.
- **Second Server:** Adds application capacity and allows the website to continue serving requests if one application server fails, provided the load balancer detects the failure.
- **Second Nginx Web Server:** Allows both servers to handle web requests.
- **Second Application Server:** Allows both application instances to process requests concurrently.
- **Application Files on Both Servers:** Each server needs the application code to execute requests independently.
- **MySQL Primary-Replica Replication:** Maintains a replicated copy of the database on another server, which can help with read scaling and recovery if properly configured.
- **Replica Database:** Provides a copy of the Primary database and can serve read queries when replication and application routing are configured appropriately.

## 4. Load Balancing Algorithm

The load balancer uses the Round Robin algorithm.

Round Robin distributes requests sequentially between the available backend servers. For example, the first request goes to Server 1, the second to Server 2, the third to Server 1, and so on.

This distributes requests across both servers. Health checks can help HAProxy avoid sending requests to unhealthy servers.

## 5. Active-Active vs Active-Passive

This infrastructure uses an Active-Active setup for the application servers.

- **Active-Active:** Both servers handle incoming requests at the same time.
- **Active-Passive:** One server handles requests while the other remains on standby and takes over when the active server fails.

Active-Active can improve resource utilization and request-handling capacity. However, the application must be designed to work correctly across multiple instances.

## 6. MySQL Primary-Replica Cluster

The Primary database handles write operations, such as inserting, updating, and deleting data.

The Replica receives database changes from the Primary through replication. It can serve read queries when the application is configured to use it.

Replication may be asynchronous, which means the Replica can temporarily lag behind the Primary.

### Difference Between the Primary and Replica

- **Primary:** The application sends write operations to this database. It can also handle read queries.
- **Replica:** The application can send read queries to this database. In this design, write operations are directed to the Primary.

If the Primary fails, the Replica does not automatically become the Primary unless a failover mechanism is configured. Applications must also be configured to use the promoted Primary.

## 7. Infrastructure Issues

### Single Points of Failure (SPOF)

- **HAProxy:** There is only one load balancer. If it fails, users may lose access to both application servers.
- **Primary Database:** If the Primary fails, write operations may become unavailable until recovery or failover occurs.
- **Shared Dependencies:** Any other single, critical dependency can cause an outage if it fails.

### Security Issues

- **No Firewall:** Unauthorized connections may reach servers or services that should not be publicly accessible.
- **No HTTPS:** HTTP traffic is not encrypted, which can expose sensitive information during transmission.
- **Database Exposure:** Database ports should not be publicly accessible. Access should be restricted to authorized application servers.

### No Monitoring

Without monitoring, it is difficult to detect server failures, high resource usage, application errors, database problems, or increased response times. Alerts and health checks are needed to detect problems quickly.


## 8. Diagram

(https://github.com/alaa2026ali/holbertonschool-system_engineering-devops/blob/main/web_infrastructure_design/1-distributed_web_infrastructure.drawio.png)
