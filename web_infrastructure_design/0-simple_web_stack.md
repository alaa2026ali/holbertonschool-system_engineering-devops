# 0. Simple web stack

## Infrastructure Overview

The infrastructure consists of one server hosting a web server (Nginx), an application server, the application codebase, and a MySQL database.

### 1. How It Works

1. A user enters `www.foobar.com` in their browser.
2. The DNS system resolves the domain name using a DNS A record pointing to the server IP address `8.8.8.8`.
3. The user's browser sends an HTTP or HTTPS request to the server.
4. Nginx receives the request and forwards dynamic requests to the application server.
5. The application server executes the application code and communicates with the MySQL database when data is needed.
6. The application server returns the response through Nginx to the user's browser.

### 2. Infrastructure Components

- **Server:** A physical or virtual machine that provides services to clients.
- **Domain name:** `foobar.com` provides a human-readable address for accessing the website.
- **DNS record:** An A record maps `www.foobar.com` to the IPv4 address `8.8.8.8`.
- **Web server (Nginx):** Receives HTTP/HTTPS requests and serves static content or forwards requests to the application server.
- **Application server:** Executes application logic and generates dynamic responses.
- **Application files:** The codebase used by the application server to run the website.
- **Database (MySQL):** Stores and manages application data.
- **Communication protocol:** HTTP or HTTPS is used between the browser and the web server. HTTPS encrypts traffic between them.

### 3. Infrastructure Issues

- **Single Point of Failure (SPOF):** If the server fails, the entire website becomes unavailable because all components run on one server.
- **Downtime during maintenance:** Restarting the web server or deploying new code may temporarily make the website unavailable.
- **Limited scalability:** A single server has limited processing power and resources. It may not handle a large increase in incoming traffic.

### Diagram

(https://github.com/alaa2026ali/holbertonschool-system_engineering-devops/blob/main/web_infrastructure_design/0-simple_web_stack.png.drawio.png)
