# Dante SOCKS5 Proxy Server

The Dante SOCKS configuration file is at `/opt/underpass/config/dante/sockd.conf`

By default, Dante requires authentication to be able to successfully connect to SOCKS5 port 1080. You won't be able to connect until you create a user.

If you wish to open your SOCKS5 service to the public, comment out `socksmethod: username` in `sockd.conf` under the `socks pass {}` directive, like in the example below: 
```
socks pass {
    from: 0.0.0.0/0 to: 0.0.0.0/0
    #socksmethod: username
    log: error
} 
```

You'll then have to recreate the `dante container`:
```
cd /opt/underpass
docker-compose up -d --force-recreate dante
```

_Please note that you might get your server in trouble if you open your SOCKS5 proxy service to anyone._

***

#### Create Users for Dante SOCKS:

To create a SOCKS5 user, issue the command below from SSH:
```
docker exec -it dante adduser -s /sbin/nologin username
```
Where: `username` is the name of the user that you want to add. You will then be asked to input a password.

#### Dante Console from Portainer

User creation can also be done from Portainer. From the `dante` Console, the command to use is:
```
adduser -s /sbin/nologin username
```

![dante_portainer_console](https://user-images.githubusercontent.com/9207205/93722750-9b42ca80-fbcb-11ea-8743-198959cbc53f.png)

From the image above, the name of the user that will be created created is `user1`.

User records will remain even if the Dante container is destroyed/deleted/removed.

***

#### Changing the SOCKS5 Port

You can change the port for Dante by changing the line, `internal: 0.0.0.0 port = 1080` in `/opt/underpass/config/dante/sockd.conf`

Recreate the container afterwards:
```
cd /opt/underpass
docker-compose up -d --force-recreate dante
```

You will then have to open the new port from the Docker host. For example, if you changed the SOCKS5 port from 1080 to 1090, you'll have to issue these commands from SSH as root:
```
firewall-cmd --remove-port=1080/tcp --permanent
firewall-cmd --zone=public --add-port=1090/tcp --permanent
firewall-cmd --reload
```

***

#### Using SOCKS5 in OpenVPN

Dante allows OpenVPN to connect to it via the `socks-proxy` directive in the `ovpn` config.

A sample [ovpn configuration file](https://github.com/gabotronix/underpass/blob/master/config/openvpn/sample_config.ovpn) is provided in `/opt/underpass/config/openvpn/sample_config.ovpn`:
```
socks-proxy ip_of_server socks5_port socks.txt
```

Where `ip_of_server` is the public IP address of your server and `socks5_port` is the port that was set in `/opt/underpass/config/dante/sockd.conf`. By default, it's port `1080` TCP.

The username and password must be placed in a text file that's in the same folder as the `ovpn` configuration file.

On Windows, the OpenVPN configuration file is in `C:\Users\Your_Username\OpenVPN\config`

Contents of `socks.txt`:
```
SOCKS5 user
SOCKS5 password
```