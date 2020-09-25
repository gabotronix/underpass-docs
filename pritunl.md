# Pritunl OpenVPN Server

### Initial Configuration

#### Pritunl: _https://ip_of_server:4433_

The page will show a notification like `Your connection is not private`. This is due to Pritunl using a self-signed certificate. Proceed anyway.

![pritunl_warning](https://user-images.githubusercontent.com/9207205/94320962-e04a7080-ffc0-11ea-9fa3-916f072f46c6.png)

***

#### Retrieving the Pritunl Admin Credentials

Pritunl will then ask you to issue a command from SSH in order to retrieve the admin password. Issue the command below:

![pritunl_initial_setup](https://user-images.githubusercontent.com/9207205/93722506-e065fd00-fbc9-11ea-9e2f-8c249533c0d7.png)
```
docker exec pritunl pritunl default-password
```

***

#### Pritunl Settings:

Once logged in to Pritunl, you will be asked to set a new admin username and password.

You may also add your IPv6 if your VPS host provided you with one.

Leave the rest of the settings to their defaults and click on `Save`. Changing port `443`, for instance, may render Pritunl inaccessible.

![pritunl_admin_settings](https://user-images.githubusercontent.com/9207205/94319475-49c88000-ffbd-11ea-87d5-f1d38575276f.png)

***

#### Organizations

The first thing to do after setting the admin user is to add an `Organization` in `Users > Add Organization`. An `Organization` is simply a name that you want for your group.

_Group_ refers to the VPN servers that you will be creating later on. Pritunl allows you to create multiple TCP or UDP OpenVPN servers. You are only limited by how beefy your server is.

However, only 2 ports have been set for this installation - 1 for TCP and another for UDP.

Before you can start a VPN server, you'll be required to attach an `Organization` to it.

![pritunl_organization](https://user-images.githubusercontent.com/9207205/93812435-30a19580-fc84-11ea-9fa9-d9f59ac27aea.png)

***

#### Creating an OpenVPN Server

Add an OpenVPN server from `Servers > Add Server`. You'll then need to fill up the server settings. A tooltip will appear when you hover your mouse over an option.

![pritunl_server_settings](https://user-images.githubusercontent.com/9207205/93813071-1ddb9080-fc85-11ea-9cb1-2d6f8d574fe7.png)

_Note:_

`Port`: refers to the port that was defined in `PRITUNL_TCP` and `PRITUNL_UDP` from `/opt/underpass/.env`. It's `1194` by default for both TCP and UDP ports.

_Please also note that `Enable WireGuard` is not supported by Underpass_
  
***

#### Adding Users and Downloading the OVPN Profile

You can create users from the `Users` page. The only fields required to create a user are the `Name` and `Organization`. The `Pin` and `Email` are optional.

![pritunl_add_users](https://user-images.githubusercontent.com/9207205/94319759-ee4ac200-ffbd-11ea-8805-448316d8b2df.png)

Once a user is created, you'll be able to download its `ovpn` profile. The profile is contained in a `tar` archive, so make sure that you have a tool to extract the `ovpn` file from a `tar` file (7-zip, WinRAR, etc).

![pritunl_profile_download](https://user-images.githubusercontent.com/9207205/94319861-25b96e80-ffbe-11ea-8548-3e2ba8debf0b.png)

***

#### Changing Pritunl OpenVPN Ports

By default, the Pritunl OpenVPN server listens on ports `1194 TCP` and `1194 UDP`. You can change them to different port numbers by editing `/opt/underpass/.env`"
```
PRITUNL_TCP=1194
PRITUNL_UDP=1194
```

Recreate the `pritunl container` afterwards:
```
cd /opt/underpass
docker-compose up -d --force-recreate pritunl
```

#### Changing Ports from the Pritunl Web Panel

You'll need to change the ports from your Pritunl `Servers` panel as well. In order to do that, you have to stop the server and access the server settings by clicking on the VPN server's name.

![pritunl_server_edit](https://user-images.githubusercontent.com/9207205/94320022-7fba3400-ffbe-11ea-87a8-2d66f78d4ef0.png)

You can then change the port from the Server Settings window. Start the server again after clicking on the `Save` button.

![pritunl_server_settings](https://user-images.githubusercontent.com/9207205/94320329-4209db00-ffbf-11ea-873c-b9ac57d7a50f.png)

Changing ports also means that your old `ovpn` files won't work anymore. You'll have to download your new VPN profile from the `Users` panel.

![pritunl_profile_download](https://user-images.githubusercontent.com/9207205/94319861-25b96e80-ffbe-11ea-8548-3e2ba8debf0b.png)

***

#### Pritunl VPN and the Squid Configuration

Squid allows the OpenVPN TCP port to connect to it via the `http-proxy` and `http-proxy-user-pass` directive in the `ovpn` config.

In `/opt/underpass/config/squid/squid.conf`, you'll have to replace the default OpenVPN TCP port `1194` with the new port number that you assigned to `PRITUNL_TCP`. Issue the command below from your SSH terminal:
```
sed -i 's|1194|YOUR_SSH_PORT|' /opt/underpass/config/squid/squid.conf
```
Where `YOUR_SSH_PORT` is your new port number for OpenVPN TCP.

Once done, recreate the `pritunl container`:
```
cd /opt/underpass
docker-compose up -d --force-recreate pritunl
```

***

#### VPN Clients

You can use OpenVPN or the Pritunl client to connect to the Pritunl VPN Server. There is no Pritunl client on mobile, but the OpenVPN client is 100% compatible.

_Download the Pritunl client: [https://client.pritunl.com/#install](https://client.pritunl.com/#install)_