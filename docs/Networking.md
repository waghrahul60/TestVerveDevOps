# Networking:
### 1.  Go through the steps a connection would take to serve a basic “hello world” HTML page, if that page was stored on disk on a traditional server with an iptables firewall running Nginx, and was accessed using HTTPS from a browser.

1.  Install Nginx
2.  Create the HTML Page
```
<!DOCTYPE html>
<html>
<head>
    <title>Hello World</title>
</head>
<body>
    <h1>Hello, World!</h1>
</body>
</html>

```
3.  Configure Nginx

Create an Nginx configuration file with the following content.

we can also setup https we need to provide ssl cert as well.

```
server {
    listen 80;
    server_name your-domain.com;

    location / {
        root /path/to/your/html/files;
        index index.html;
    }
}

```
Enable the site configuration and reload Nginx.

4.  Configure iptables Firewall Rules

Allow incoming traffic on ports 80 (HTTP) and 443 (HTTPS) using iptables rules

```
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

5.  Restart Nginx and iptables.
Restart Nginx to apply the configuration changes.

6.  Access the Page via Browser

### 2.  What is typically hosted in the 169.254.0.0/24 CIDR block in a cloud environment?

1.  The range from 169.254.0.0 to 169.254.255.255 is reserved for link-local addresses.
2.  Link-local addresses are used for communication within the same physical or logical network link.
3.  These addresses are commonly used for automatic private IP assignment when devices need to communicate without relying on external DHCP servers or manual configuration.
4.  In AWS The instance metadata service uses this ip.
5.  Docker containers in bridge mode use link-local addresses for communication within the same bridge network.

### 3.  How would you configure the security groups in a cloud environment to ensure that services could talk to one another within the same subnet, without using IP addresses (including CIDR Notation)?

### Amazon Web Services (AWS)
1.  In AWS, create a security group for your services within the same VPC.
2.  Assign this security group to all relevant instances or services that need to communicate service-A and Service-B.
3.  Define inbound rules in the security group to allow traffic from the same security group self-reference.
4.   For example, create an inbound rule that allows all traffic from the security group itself (using the security group ID).

### Google Cloud Platform (GCP)
1.  In GCP, use VPC firewall rules to control traffic between services.
2.  Create a custom firewall rule that allows traffic from the same subnet.
3.  Specify the source IP range as the subnet CIDR block e.g. 10.2.0.0/24.
4.  Apply this rule to the relevant instances or services like service-A and Service-B.