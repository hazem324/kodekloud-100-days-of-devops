# Day 20 - Configure Nginx + PHP-FPM Using Unix Sock

## Task Description

The Nautilus team is preparing to deploy a new PHP-based application on the Stratos DC infrastructure.

---

## Requirements

### 1. Nginx Setup

* Install **Nginx** on App Server 1.
* Configure Nginx to:

  * Listen on port:

    ```
    8092
    ```
  * Use document root:

    ```
    /var/www/html
    ```

---

### 2. PHP-FPM Setup

* Install **PHP-FPM 8.2** on App Server 1.
* Configure the Unix socket:

  ```
  /var/run/php-fpm/default.sock
  ```
* Create parent directories if required.

---

### 3. Integration

* Configure Nginx to forward PHP requests to PHP-FPM.

---

### 4. Testing

From the Jump Host, run:

```
curl http://stapp01:8092/index.php
```

---

## Expected Result

* Nginx and PHP-FPM work together correctly.
* PHP files are processed and returned successfully.
* The curl command produces valid output.

---

## Notes

* Do not modify `index.php` or `info.php`.
* Files already exist under `/var/www/html`.

---

End of Task
