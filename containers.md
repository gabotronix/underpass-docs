# Docker Containers

#### Identifying Container Names and Published Ports

Container names are used to stop, restart, or remove containers. You can view the names, as well as their assigned ports in Portainer:

![portainer_container_list](https://user-images.githubusercontent.com/9207205/93723394-cda2f680-fbd0-11ea-9fbb-2c927366f9ba.png)

You can also issue the command below from SSH:
```
docker ps
```

Docker ports have the format, `2222:22`

The number before the _colon (:)_ represents the port that will be exposed to the public. That is the port to use in your SSH, VPN, or Proxy clients. The number after the _colon_ is used by Docker internally.

***