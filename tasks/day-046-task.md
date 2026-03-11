# Day 47 - Deploy an App on Docker Containers

## Task Description

Set up a complete containerized stack using Docker Compose to deploy a web application and its database for pre-production testing.

## Requirements

* Create a Docker Compose file at the following path:

```
/opt/data/docker-compose.yml
```

* Perform the setup on App Server 3 in Stratos Datacenter.
* Define two services: **web** and **db**.

### Web Service

* Container name:

```
php_host
```

* Image:

```
php (any apache-based tag)
```

* Port mapping:

```
Host Port: 3000
Container Port: 80
```

* Volume mapping:

```
Host Path: /var/www/html
Container Path: /var/www/html
```

### Database Service

* Container name:

```
mysql_host
```

* Image:

```
mariadb (any tag, preferably latest)
```

* Port mapping:

```
Host Port: 3306
Container Port: 3306
```

* Volume mapping:

```
Host Path: /var/lib/mysql
Container Path: /var/lib/mysql
```

* Environment configuration:

  * Database name:

```
database_host
```

* Create a non-root database user with a strong, complex password.

## Expected Result

* A valid Docker Compose file exists at the specified location.
* Two containers (`php_host` and `mysql_host`) are created and running.
* The web application is accessible via host port `3000`.
* The database service is reachable on port `3306`.
* Data persistence is ensured through the configured volumes.

## Notes

* All existing containers will be removed when the stack is redeployed using the compose file.

End of Task
