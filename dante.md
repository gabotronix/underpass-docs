# Dante SOCKS5 Proxy Server

### Initial Configuration

#### Create Users for Dante SOCKS:

The **Dante SOCKS configuration** is located at `/opt/underpass/config/dante/sockd.conf`

By default, it requires authentication to be able to successfully connect.

If you want to risk opening your SOCKS5 service to the public, comment out the line in `sockd.conf` that's inside the `socks pass {}` directive: 
```
#socksmethod: username
```

To create a SOCKS5 user, issue the command below from SSH:
```
docker exec -it dante adduser -s /sbin/nologin username
```
Where: `username` is the name of the user that you want to add. You will then be asked to input a password.

User creation can also be done from Portainer:

![dante_portainer_console](https://user-images.githubusercontent.com/9207205/93722750-9b42ca80-fbcb-11ea-8743-198959cbc53f.png)

User records will persist even if the container is destroyed/deleted/removed.

***

#### Changing the SOCKS5 Port

You can change the port for Dante by changing the line, `internal: 0.0.0.0 port = 1080` in `/opt/underpass/config/dante/sockd.conf`

Recreate the container afterwards:
```
cd /opt/underpass
docker-compose up -d --force-recreate dante
```

You will then have to open the new port from the Docker host. For example, if you changed the SOCKS5 port from 1080 to 1090, issue these commands from SSH as root:
```
firewall-cmd --remove-port=1080/tcp --permanent
firewall-cmd --zone=public --add-port=1090/tcp --permanent
firewall-cmd --reload
```

***