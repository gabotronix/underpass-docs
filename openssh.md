# Open-SSH Server

### Initial Configuration

#### Create Users for OpenSSH:

Users for [OpenSSH Server](https://gitlab.com/vlasov-y/openssh-server) are created via a YAML file. The file is at `/opt/underpass/config/openssh/config.yml`

`config.yml` already contains a sample user named, `underpass`

You can replace it with a new name. For the password, generate it from [here](https://www.mkpasswd.net/?type=crypt-sha512):
```
Password: your_desired_password
Type: crypt-sha512
```

Click on the `Hash` button. The page will generate a hash which you can then copy to `config.yml`

When pasting the hash in `password:`, please note that it is enclosed in single quotations `password: 'hash_here'`.

If you wish to create more users, simply copy-paste the entries below `users:`

***

#### Changing OpenSSH Port

You can change the port of OpenSSH by changing `SSH_PORT` in `/opt/underpass/.env`. By default, it's assigned to port 2222. Once you're done editing `.env`, restart the container:
```
cd /opt/underpass
docker-compose restart ssh
```

***

#### Using OpenSSH in HTTP Proxy Injector

The OpenSSH server is usually paired with Squid in order to create a tunnel between your device and your Underpass server.

![hpi_settings](https://user-images.githubusercontent.com/9207205/94207196-abbfb180-fef9-11ea-863b-4cc61a2e31a9.png)

From HTTP Proxy Injector, fill in the following settings:
- Host: the public IP address of your server
- Port: 2222 (or the port you set in )
- Username: the username you set in `/opt/underpass/config/openssh/config.yml`
- Password: the password you set in `/opt/underpass/config/openssh/config.yml` (_this is not the hash value, but the clear text password_)
- Mode: PF System or PF Portable *
- Proxy:Port - the public IP address of your server and the port that you assigned to Squid (it's 3128 by default)
- Start Tunnel: checked/marked x
- Generator: `CONNECT [host_port] [protocol][crlf]Host: google.com[crlf]X-Online-Host: google.com[crlf]X-Forwarded-For: google.com[crlf]Connection: Keep-Alive[crlf][crlf]`

**Notes**

_* PF System or PF Portable refers to your installation of Proxifier. It's a third-party software that you'll have to obtain by yourself._

_google.com is a sample website that's used as a stepping point to tunnel through. In the real world, it's a unique URL._

#### HTTP Proxy Connection Status

You'll know that you've successfully created the tunnel and connected to your SSH and Squid proxy servers if you get a notification in the system tray on Windows 10. On Android, the LOG section of HTTP Proxy will show an entry called `CONNECTED`.

![hpi_connected](https://user-images.githubusercontent.com/9207205/94208278-0fe37500-fefc-11ea-9bd7-0a0ce327b0e8.png)
