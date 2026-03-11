# Day 42 - Create a Docker Network

## Task Description

Prepare a dedicated Docker network to support upcoming application deployments by creating a custom macvlan-based network.

## Requirements

* Create a Docker network with the following name:

```
media
```

* Use the following network driver:

```
macvlan
```

* Configure the network with:

  * Subnet:

```
172.168.0.0/24
```

* IP range:

```
172.168.0.0/24
```

* Perform the setup on App Server 2 in Stratos Datacenter.

## Expected Result

* A Docker network named `media` exists on App Server 2.
* The network uses the macvlan driver.
* The network is configured with the specified subnet and IP range.

## Notes

* The network is intended for future use and does not require containers to be attached at this stage.

End of Task
