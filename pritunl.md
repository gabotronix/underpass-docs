# Pritunl OpenVPN Server

### Initial Configuration

#### Pritunl: _https://ip_of_server:4433_

![pritunl_initial_setup](https://user-images.githubusercontent.com/9207205/93722506-e065fd00-fbc9-11ea-9e2f-8c249533c0d7.png)

The page will show a notification like `Your connection is not private`. This is due to Pritunl using a self-signed certificate. Proceed anyway.

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

`Port`: refers to the port that was defined in `PRITUNL_TCP` and `PRITUNL_UDP` from `/opt/underpass/.env`. It's `1194` by default for both TCP and UDP ports.

_Please also note that `Enable WireGuard` is not supported by Underpass_
  
***

#### Adding Users and Download the OVPN Profile

You can create users from the `Users` page. The only field required to create a user is the `username`. The `PIN` and `email address` are optional. Once a user is created, you'll be able to download its `ovpn` profile.

The profile is contained in a `tar` archive, so make sure that you have the tool to extract the `ovpn` file from a `tar` file (7-zip or WinRAR).

***

#### VPN Clients

You can use OpenVPN or the Pritunl client to connect to the Pritunl VPN Server. There is no Pritunl client On mobile, but the OpenVPN client can be used.

_Download the Pritunl client: [https://client.pritunl.com/#install](https://client.pritunl.com/#install)_