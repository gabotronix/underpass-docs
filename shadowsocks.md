# Shadowsocks

### Change Shadowsocks Password

The Shadowsocks password is defined in `/opt/underpass/.env`

Please change the Shadowsocks password immediately in order to avoid unauthorized access. You can do so by editing `.env` using your preferred text editor and changing the value of `SHADOWSOCKS_PASSWORD=`

Once done, restart the container:
```
cd /opt/underpass
docker-compose restart shadowsocks
```

***

_Download the Shadowsocks client from the [official site](https://shadowsocks.org/en/download/clients.html)_
