# Day 52 - Revert Deployment to Previous Version in Kubernetes

## Task Description

Revert a Kubernetes deployment to its previous stable revision after a faulty release.

## Requirements

* Target the following deployment:

```
nginx-deployment
```

* Roll back the deployment to the immediately previous revision.
* Perform the operation using the `kubectl` utility configured on `jump_host`.
* Ensure the rollback completes successfully without manual pod recreation.

## Expected Result

* The deployment `nginx-deployment` is reverted to its prior revision.
* New pods are created based on the previous configuration.
* All pods are running and stable after the rollback.

End of Task
