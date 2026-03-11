# Day 59 - Troubleshoot Deployment issues in Kubernetes

## Task Description

Investigate and resolve issues causing the Redis deployment to fail, restoring all pods to a healthy running state.

## Requirements

* Inspect the following deployment:

```
redis-deployment
```

* Identify the root cause preventing pods from running.
* Correct the misconfiguration or error without recreating the deployment from scratch unless required.
* Ensure all associated pods transition to a healthy running state.
* Perform all operations using the `kubectl` utility configured on `jump_host`.

## Expected Result

* The deployment `redis-deployment` is functioning correctly.
* All pods managed by the deployment are in a running and ready state.
* No configuration errors remain affecting pod startup.

End of Task
