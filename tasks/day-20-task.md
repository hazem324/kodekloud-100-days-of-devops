Perfect, I will format this one in the same clean style (not keeping the original wording):

# PHP Application Deployment Task – Nautilus Project

## Task Description

The Nautilus team is preparing to deploy a new PHP-based application on the Stratos DC infrastructure.
The application must be served using Nginx and PHP-FPM on App Server 1.

---

## Requirements

### 1. Web Server Setup

* Install **Nginx** on App Server 1.
* Configure Nginx to:

  * Listen on port **8092**
  * Use document root:

    ```
    /var/www/html
    ```

---

### 2. PHP-FPM Setup

* Install **PHP-FPM version 8.2** on App Server 1.
* Configure PHP-FPM to use the Unix socket:

  ```
  /var/run/php-fpm/default.sock
  ```
* Create parent directories if they do not exist.

---

### 3. Integration

* Configure Nginx to forward PHP requests to PHP-FPM using the above socket.

---

### 4. Testing

From the Jump Host, verify using:

```
curl http://stapp01:8092/index.php
```

---

## Expected Result

* Nginx serves the application on port 8092.
* PHP files are correctly processed by PHP-FPM.
* The curl command returns valid PHP output.

---

## Notes

* Do not modify `index.php` or `info.php`.
* Files are already present under `/var/www/html`.

---

End of Task

Send the next task when ready.
