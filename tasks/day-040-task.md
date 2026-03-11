# Day 40 - Docker EXEC Operations

## Task Description

Complete the pending setup by installing and configuring the Apache web server inside an existing container on the application server.

## Requirements

* Work inside the running container named:

```
kkloud
```

* Install the Apache web server package:

```
apache2
```

* Update Apache configuration to listen on the following port:

```
8084
```

* Ensure Apache listens on all interfaces (no IP or hostname restriction).
* Verify that the Apache service is running inside the container.
* Leave the container in a running state after completion.

## Expected Result

* Apache is installed inside the container.
* Apache is configured to listen on port `8084` on all interfaces.
* The Apache service is active and running.
* The `kkloud` container remains running.

## Notes

* Default HTTP port must not be used.
* No binding to a specific IP address or hostname is allowed.

End of Task
