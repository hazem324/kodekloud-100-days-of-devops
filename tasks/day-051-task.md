# Day 51 - Execute Rolling Updates in Kubernetes

## Task Description

Perform a rolling update on an existing Kubernetes deployment to apply a new Nginx image version without downtime.

## Requirements

* Target the following deployment:

```
nginx-deployment
```

* Update the container image to:

```
nginx:1.17
```

* Execute the update using the `kubectl` utility configured on `jump_host`.
* Ensure the update follows a rolling strategy (no full downtime).
* Verify that all updated pods are running successfully after the rollout.

## Expected Result

* The `nginx-deployment` uses the `nginx:1.17` image.
* The update completes without errors.
* All pods are in a running and ready state.
* No service disruption occurs during the rollout.

End of Task
