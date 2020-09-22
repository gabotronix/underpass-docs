# underpass-docs

### Initial Configuration

#### Portainer: _http://ip_of_server:9000_

![portainer_initial_setup](https://user-images.githubusercontent.com/9207205/93722499-d47a3b00-fbc9-11ea-8754-ddc698e6dd48.png)

Set an admin password (please set an extremely strong [password](https://www.lastpass.com/password-generator))

***

#### Pritunl: _https://ip_of_server:4433_

![pritunl_initial_setup](https://user-images.githubusercontent.com/9207205/93722506-e065fd00-fbc9-11ea-9e2f-8c249533c0d7.png)

The page will show a notification like _Your connection is not private_. This is due to Pritunl using a self-signed certificate. Proceed anyway.

Pritunl will then ask you to issue a command from SSH in order to retrieve the admin password. Issue the command below:
```
docker exec pritunl pritunl default-password
```

***

#### Pritunl Settings:

Once logged in to Pritunl, you will be asked to set a new admin username and password. Leave the rest of the settings to their defaults. Changing port `443`, for instance, may render Pritunl inaccessible.

The first thing to do after setting the admin user is to add an `Organization` in `Users > Add Organization`. An `Organization` is simply a name that you want for your group.

_Group_ refers to the VPN servers that you will be creating later on. Pritunl allows you to create multiple TCP or UDP OpenVPN servers. You are only limited by your server resources. However, only 2 ports have been set for Underpass - 1 for TCP and another for UDP.

Before you can start a VPN server, you'll be required to attach an `Organization` to it.

![pritunl_organization](https://user-images.githubusercontent.com/9207205/93812435-30a19580-fc84-11ea-9fa9-d9f59ac27aea.png)

***

#### Adding an OpenVPN Server

Add an OpenVPN server from `Servers > Add Server`. You'll then need to fill up the server settings. A tooltip will appear when you hover your mouse over an option.

![pritunl_server_settings](https://user-images.githubusercontent.com/9207205/93813071-1ddb9080-fc85-11ea-9cb1-2d6f8d574fe7.png)

_Note:_

`Port`: refers to the port that was defined from `PRITUNL_TCP` and `PRITUNL_UDP` in `/opt/underpass/.env`. It's `1194` by default.

_Please also note that `Enable WireGuard` is not supported by Underpass_
  
***

#### Change Shadowsocks Password

The Shadowsocks password is defined in `/opt/underpass/.env`

Please change the Shadowsocks password immediately in order to avoid unauthorized access. You can do so by editing `.env` using your preferred text editor and changing the value of `SHADOWSOCKS_PASSWORD=`

Once done, restart the container:
```
cd /opt/underpass
docker-compose restart shadowsocks
```

***

#### Create Users for Squid:

The **Squid configuration** files are located at `/opt/underpass/config/squid/`

In the _squid_ folder, edit the `users` file using your preferred text editor and use a [_passwd-generator_](https://hostingcanada.org/htpasswd-generator/) to create your own user-password combination. Refer to the `users` file in `/opt/underpass/squid/` for more info.

Any changes to the squid configuration will require you to recreate the container. Issue the commands below from SSH in order to do that:
```
cd /opt/underpass/
docker-compose up -d --force-recreate squid
```

#### Changing the Squid Port

You can change the port for Squid by changing the `http_port` number from `/opt/underpass/config/squid.conf`
```
# Squid normally listens to port 3128
http_port 3128
```

Recreate the container afterwards:
```
cd /opt/underpass
docker-compose up -d --force-recreate squid
```

You will then have to open the new port from the Docker host. For example, if you changed the Squid port from 3128 to 3888, issue these commands as root from SSH:
```
firewall-cmd --remove-service=squid --permanent
firewall-cmd --zone=public --add-port=3888/tcp --permanent
firewall-cmd --reload
```

***

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

#### Create Users for OpenSSH:

Users for OpenSSH are created via a YAML file. The file is located at `/opt/underpass/config/openssh/config.yml`

Instructions on how to create a user and generate a password are already in `config.yml`

***

#### Wireguard Usage:

By default, Wireguard is configured to have 1 peer that can be used by multiple users and devices at the same time. If you wish to configure more than one peer, edit `WIREGUARD_PEERS=1` from `/opt/underpass/.env` and change the number as desired. Restart the container afterwards:
```
cd /opt/underpass
docker-compose restart wireguard
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
You can also retrieve the peer's QR Code from the Wireguard server so that you can easily pair it with your mobile device's Wireguard client. From SSH, issue the command below:
```
docker exec wireguard /app/show-peer 1
```

![wireguard_server_show_qr](https://user-images.githubusercontent.com/9207205/93795143-9170a400-fc6b-11ea-8db0-ebfdda1084ba.png)

***

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

#### Nginx Proxy Manager

_Note: It is assumed that you already have enough knowledge about how DNS, Domains, and Subdomains work._

The web panel for Nginx Proxy Manager is in http://ip_of_your_server:8181

The initial login credentials are:
```
Email address: admin@example.com
Password: changeme
```

Once you're in, please change the Admin email address and password right away.

#### Adding a Proxy Host

Nginx Proxy Manager is used when you have a domain name, and you want it to point to a particular container that has web access (Pritunl, Portainer, Netdata, Nginx Proxy Manager, Heimdall, Droppy, Mongo-Express).

For example, you want `pritunl.underpass.cf` to point to your _Pritunl_ access URL at `https://ip_of_your_server:4433`

From the web panel, go to `Hosts > Proxy Hosts > Add Proxy Host`

![nginx_proxy_add](https://user-images.githubusercontent.com/9207205/93934268-7b3f1280-fd55-11ea-89c7-2e2dec8f8545.png)

You will then be asked the following:
```
Domain Names: we'll use pritunl.underpass.cf
Scheme: we'll use https
Forward Hostname/IP: the name of your container (pritunl *)
Forward Port: the port of the container that Docker uses internally (443 *)
Click on _Save_
```
_* [Refer to Identifying Container Names and Published Ports](https://github.com/gabotronix/underpass#identifying-container-names-and-published-ports). The number to the right of the colon sign is the one to look for._

![nginx_portainer_port_list](https://user-images.githubusercontent.com/9207205/93934302-88f49800-fd55-11ea-8e3c-d0daf2eceff8.png)

Once the entry is added, you'll be able to access Pritunl using the subdomain, `https://pritunl.underpass.cf` instead of `https://ip_of_your_server:4433`.

If you want to remove the _Your connection is not private_ warning from your web browser, you can do so by going to `SSL Certificates > Add SSL Certificate`. It may take a couple of minutes to register your SSL certificate.

![nginx_ssl_letsencrypt](https://user-images.githubusercontent.com/9207205/93936143-484a4e00-fd58-11ea-838f-3fe3c17edb99.png)

Once you have the SSL Certificate listed, go back to `Hosts > Proxy Hosts`, edit your domain/subdomain by clicking on the three dots on the right. From the `Edit Proxy Host` window, go to the `SSL` tab and select the SSL certificate that was registered earlier.

![nginx_proxy_ssl_pair](https://user-images.githubusercontent.com/9207205/93936363-abd47b80-fd58-11ea-9f96-35eb5371547b.png)

You can also click on `Force SSL` to activate automatic https access. Finally, click on _Save_. You should now be able to access your domain/subdomain without the web browser warning message.

***
