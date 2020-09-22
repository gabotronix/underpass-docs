# Squid

### Initial Configuration

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