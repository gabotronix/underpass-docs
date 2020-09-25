# Wireguard

#### Wireguard Usage and Configuration:

By default, Wireguard is configured to have 1 peer that can be used by multiple users and devices at the same time. If you wish to configure more than one peer, edit `WIREGUARD_PEERS=1` from `/opt/underpass/.env` and change the number as desired. Recreate the container afterwards:
```
cd /opt/underpass
docker-compose up -d --force-recreate wireguard
```

To retrieve the peer configuration, issue this command from SSH:
```
docker exec wireguard cat /config/peer1/peer1.conf
```

`peer1` refers to the first peer that was set up by Wireguard. If you changed `WIREGUARD_PEERS` to more than 1, then you'll also be able to retrieve the configuration for `peer2`, `peer3`, and so on by changing the command above to the corresponding peer number.

You can then copy-paste the config in your Wireguard client:
```
[Interface]
Address = 10.13.13.2
PrivateKey = 0NUF5UY4NnqkrXIiyxXxf+eWQSMMfzsSnDiEXxX/X1X=
ListenPort = 51820
DNS = 8.8.8.8

[Peer]
PublicKey = 2sv1xXxpXDn8Fmyhb6QNQxxXcl3PLmMj18qXxXNlnxx=
Endpoint = 1xx.2xxx.1xx.1xx:51820
AllowedIPs = 0.0.0.0/0, ::/0
```
![wireguard_client_settings](https://user-images.githubusercontent.com/9207205/94211972-7620c580-ff05-11ea-861e-1c6cd20466f1.png)

`Endpoint = 1xx.2xxx.1xx.1xx:51820` refers to your server's public IP address and port assignment.

***

#### Wireguard QR Code for Mobile

You can also retrieve the peer's QR Code from the Wireguard server so that you can easily pair it with your mobile device's Wireguard client. From SSH, issue the command below:
```
docker exec wireguard /app/show-peer 1
```

![wireguard_server_show_qr](https://user-images.githubusercontent.com/9207205/93795143-9170a400-fc6b-11ea-8db0-ebfdda1084ba.png)

***

#### Changing the UDP Port for Wireguard

By default, Wireguard uses port 51820 UDP. You can change the port by changing `WIREGUARD_PORT_UDP` in `/opt/underpass/.env`. Recreate the container afterwards:
```
cd /opt/underpass
docker-compose up -d --force-recreate wireguard
```