# Create a Docker Network

The Nautilus DevOps team needs to set up several docker environments for different applications. One of the team members has been assigned a ticket where he has been asked to create some docker networks to be used later. Complete the task based on the following ticket description:

* Create a docker network named as `media` on App Server 2 in Stratos DC.
* Configure it to use `macvlan` drivers.
* Set it to use subnet `172.168.0.0/24` and iprange `172.168.0.0/24`.

---

## Steps

### 1. Login into app server 2 and run the following command:

```sh
docker network create media -d macvlan --subnet=172.168.0.0/24 --ip-range=172.168.0.0/24
```

### 2. To verify, run the following command:

```sh
docker network ls
```

If you need to understand more about the `docker network` command, you can use `help`:

```sh
docker network --help
```

---

## Good to Know 

### Docker Networking

* **Purpose**: Enable communication between containers and external networks
* **Isolation**: Network segmentation for improved security
* **Service Discovery**: Containers can communicate using network-based connectivity
* **Flexibility**: Support multiple network types for different use cases

### Network Drivers

* **bridge**: Default driver for single-host container networking
* **host**: Containers share the host network stack
* **overlay**: Enables multi-host networking (Docker Swarm)
* **macvlan**: Assigns containers real MAC and IP addresses
* **none**: Completely disables networking for containers

### MACVLAN Driver

* **Purpose**: Make containers appear as physical devices on the network
* **MAC Addresses**: Each container receives a unique MAC address
* **Direct Network Access**: Containers communicate directly with the physical network
* **Use Case**: Ideal for legacy or network-dependent applications

### Network Configuration

* **Subnet**: Defines the network address range
* **IP Range**: Specifies assignable IPs for containers
* **Gateway**: Default route for outbound traffic
* **DNS**: Can be customized per network

