# Write a Docker Compose File


## Task Description

Deploy an Apache HTTP Server container using Docker Compose to host static website content with predefined port and volume mappings.

## Requirements

* Create a Docker Compose file at the following path:

```
/opt/docker/docker-compose.yml
```

* Define a service that uses the following image:

```
httpd:latest
```

* Ensure the container name is set to:

```
httpd
```

* Configure port mapping:

```
Host Port: 6100
Container Port: 80
```

* Configure volume mapping between host and container:

```
Host Path: /opt/finance
Container Path: /usr/local/apache2/htdocs
```

* Perform all setup on App Server 3 in Stratos Datacenter.
* Do not alter any existing data in the mapped directories.

## Expected Result

* A valid Docker Compose file exists at the specified location.
* An `httpd` container is created and running via Docker Compose.
* The container serves content through host port `6100`.
* The static content directory is correctly mounted without data modification.

End of Task
