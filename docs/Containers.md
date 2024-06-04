# Containers:
### 1. Provide the basic items you would need to add to a Dockerfile to create a container that could serve a “Hello world” static index.html page to a client over HTTPS (specific command lines aren’t needed, just the components required is fine)


In order to deploy we required the following things.
To deploy html web app we required a webserver.

1. Create Dockerfile
```
# Use the official Nginx image as a base
# Insted on nginx we can use specific nginx based images like nginx:alpine
FROM nginx

# Copy custom Nginx config
COPY nginx.conf /etc/nginx/nginx.conf

# Copy SSL certificates and keys
COPY ssl/server.crt /etc/ssl/certs/server.crt
COPY ssl/server.key /etc/ssl/private/server.key

# Copy the static HTML file
COPY index.html /usr/share/nginx/html/index.html

# Expose port 443 for HTTPS
EXPOSE 443

# Start Nginx server
CMD ["nginx", "-g", "daemon off;"]

```

as we want HTTPS we need to expose port 443 and we need to provide an SSL certificate.


2. nginx.conf file : 
inside nginx.conf file we can provide index.html file path, SSL, and server settings.

```
events {
    worker_connections 1024;
}

http {
    server {
        listen 443 ssl;
        server_name localhost;

        ssl_certificate /etc/ssl/certs/server.crt;
        ssl_certificate_key /etc/ssl/private/server.key;

        location / {
            root /usr/share/nginx/html;
            index index.html;
        }
    }
}

```

3.  index.html

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

place all three files in same directory

```
└── W_DockerTest
    ├── Dockerfile
    ├── index.html
    ├── nginx.conf
    └── ssl
        ├── server.crt
        └── server.key

```

Build an image and then run a container using that image

```
docker build -t hello-world-nginx.
docker run -d -p 443:443 --name my-nginx-container hello-world-nginx
```

### 2. How would you configure a container build system or script for an application that uses several public artifacts, in such a way that you would not have to rely on public repositories to pull down those artifacts with each build?

1.  Set up a private artifact repository to achieve this we can host our own repository using Nexus, Artifactory, or Docker Registry. then we can store the required artifacts in our private repository.
2.  Use multi-stage Docker builds to separate the artifact download phase from the final image. In the first stage, download the artifacts from repositories.
In the 2nd stage, copy the downloaded artifacts into the final image.
3.  We can create a custom base Docker image that includes the necessary artifacts.
4.  we can Download the artifacts and include them directly in our project repository.


### 3. What are the pros and cons of running a replicated set of MySQL databases with, or without, containerization?

### Running Replicated MySQL Databases Without Containerization:

### Pros:
1.  Setting up the MySQL server is easy. Familiarity with the troubleshooting processes.
2.  Replication ensures data redundancy and availability.
3.  Improved performance for read-intensive applications by creating read replicas.

### Cons:
1.  Configuring replication involves manual steps.
2.  Monitoring and maintaining replicas can be time-consuming.
3.  Ensuring consistent backups across replicas can be tricky. Backing up replicas requires additional considerations (e.g., locking tables, and ensuring consistency).

### Running Replicated MySQL Databases with Containerization (Docker):

### Pros:
1.  Containers encapsulate the entire database environment, including dependencies. Easy to move replicas across different hosts or cloud providers.
2.  Containers share the host OS kernel, reducing resource overhead. So, efficient utilization of server resources.
3.  Containers ensure consistent versions of MySQL and other components.

### Cons:
1.  Requires a strong understanding of container networking and storage.
2.  Setting up networking between containers and across hosts can be challenging.
3.  Requires configuring DNS, load balancers, or service discovery.
4. Troubleshooting will also become hard.


### 4. What is the underlying concept that differentiates containers from virtual machines?

### Containers:
1.  Containers virtualize software layers above the operating system level.
2.  They run on the same host operating system and share its kernel.
3.  Containers are lightweight, and efficient for specific services or applications.
4.  Isolation is provided at the user mode level, allowing multiple containers to coexist on the same OS.
5.  Inside containers only one process should be running.
6.  You can run containers on any of the host operating systems.

### Virtual Machines (VMs):
1.  Virtual Machines an entire machines with the hardware layers.
2.  Each VM runs a complete operating system, including its own kernel.
3.  VMs are more resource-intensive due to running separate OS instances.
4.  VMs provide stronger isolation, making them suitable for scenarios requiring strict security boundaries.