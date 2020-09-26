# Squid

#### Creating Users for Squid:

By default, Squid only allows connections from an authenticated user, or from an SSH session that originated from the same server.

The **Squid configuration** files are located in `/opt/underpass/config/squid/`

In the _squid_ folder, edit the `users` file using your preferred text editor and use a [_passwd-generator_](https://hostingcanada.org/htpasswd-generator/) to create your own user-password combination.

![squid_password_gen](https://user-images.githubusercontent.com/9207205/93942673-78e3b500-fd63-11ea-969f-ebfd3b880abd.png)

From the passwd-generator homepage, input your desired username and password. For the `Mode`, select `Apache specific salted MD5`. Finally, click on the Create .htpasswd file button.

In the `users` file, remove the entry for `underpass` and replace it with your newly generated username-password combination.

Any change to the squid configuration folder will require you to recreate the `squid container`. The SSH commands below will do just that:
```
cd /opt/underpass/
docker-compose up -d --force-recreate squid
```

***

#### Changing the Squid Port

You can change the port for Squid by changing the `http_port` number from `/opt/underpass/config/squid/squid.conf`
```
http_port 3128
```

Recreate the container afterwards:
```
cd /opt/underpass
docker-compose up -d --force-recreate squid
```

You will then have to open the new port from the Docker host. For instance, if you changed the Squid port from 3128 to 3888, you'll have to issue these commands from SSH as root:
```
firewall-cmd --remove-service=squid --permanent
firewall-cmd --zone=public --add-port=3888/tcp --permanent
firewall-cmd --reload
```

***