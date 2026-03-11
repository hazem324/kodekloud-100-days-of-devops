# Day 62 - Manage Secrets in Kubernetes

## Task Description

Securely store license information using a Kubernetes Secret and mount it into a running pod for verification.

## Requirements

### Secret

* Create a generic secret with the following name:

```
news
```

* Source file:

```
/opt/news.txt
```

* The secret must contain the license/password stored in `news.txt`.

### Pod

* Name:

```
secret-xfusion
```

* Container name:

```
secret-container-xfusion
```

* Image:

```
ubuntu:latest
```

* Use a sleep command to keep the container in a running state.
* Mount the created secret as a volume at:

```
/opt/demo
```

### Verification

* Exec into container:

```
secret-container-xfusion
```

* Confirm the secret file is available under:

```
/opt/demo
```

* Ensure the pod is in `Running` state before validation.

* Perform all operations using the `kubectl` utility configured on `jump_host`.

## Expected Result

* Secret `news` is successfully created from `/opt/news.txt`.
* Pod `secret-xfusion` is running.
* Secret is mounted inside the container at `/opt/demo`.
* License/password file is accessible within the container.

End of Task
