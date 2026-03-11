# Day 64 - Fix Python App Deployed on Kubernetes Cluster

## Task Description

Investigate and correct configuration issues preventing a Python Flask application from running properly and being accessible via NodePort.

## Requirements

* Inspect the existing deployment:

```
python-deployment-xfusion
```

* Image in use:

```
poroko/flask-demo-appimage
```

* Do not recreate the deployment unless necessary; correct misconfigurations.

### Service Configuration

* Ensure the service exposes the application using:

```
Type: NodePort
```

* Set:

```
nodePort: 32345
```

* Set:

```
targetPort: 5000
```

(Flask default application port)

* Verify container port matches the Flask default port.
* Ensure pods are running and ready.
* Perform all operations using the `kubectl` utility configured on `jump_host`.

## Expected Result

* Deployment `python-deployment-xfusion` is running without errors.
* Service correctly maps traffic to the Flask container.
* Application is accessible via NodePort `32345`.
* All pods are in a running and ready state.

End of Task
