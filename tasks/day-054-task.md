# Day 54 - Kubernetes Shared Volumes

## Task Description

Deploy a multi-container pod that uses a shared `emptyDir` volume to allow data exchange between containers.

## Requirements

* Create a pod with the following name:

```
volume-share-nautilus
```

### Volume Configuration

* Define a volume with:

```
Name: volume-share
Type: emptyDir
```

### Container 1

* Name:

```
volume-container-nautilus-1
```

* Image:

```
debian:latest
```

* Mount volume at:

```
/tmp/news
```

* Run a sleep command to keep the container in a running state.

### Container 2

* Name:

```
volume-container-nautilus-2
```

* Image:

```
debian:latest
```

* Mount volume at:

```
/tmp/demo
```

* Run a sleep command to keep the container in a running state.

### Validation

* Exec into:

```
volume-container-nautilus-1
```

* Create a file:

```
/tmp/news/news.txt
```

* Confirm the same file is visible at:

```
/tmp/demo/news.txt
```

inside `volume-container-nautilus-2`.

* Use `kubectl` configured on `jump_host` to perform all operations.

## Expected Result

* The pod `volume-share-nautilus` is running with two containers.
* Both containers mount the same `emptyDir` volume.
* A file created in one container is accessible from the other container via their respective mount paths.

End of Task
