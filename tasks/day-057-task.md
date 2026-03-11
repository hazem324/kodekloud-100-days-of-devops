# Day 57 - Print Environment Variables

## Task Description

Create a Kubernetes pod that prints predefined greeting environment variables and exits without restarting.

## Requirements

* Create a pod with the following name:

```
print-envars-greeting
```

* Configure a container with:

  * Name:

```
print-env-container
```

* Image:

```
bash
```

* Define the following environment variables:

  * `GREETING` =

```
Welcome to
```

* `COMPANY` =

```
Stratos
```

* `GROUP` =

```
Ltd
```

* Use the exact container command:

```
["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
```

* Set pod restart policy to:

```
Never
```

* Perform all operations using the `kubectl` utility configured on `jump_host`.

## Expected Result

* The pod `print-envars-greeting` runs and prints the concatenated greeting message.
* The pod completes execution without entering a restart loop.
* The output is viewable using:

```
kubectl logs -f print-envars-greeting
```

End of Task
