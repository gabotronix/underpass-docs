# Shadowsocks

#### Changing the Shadowsocks Password

The Shadowsocks password is defined in `/opt/underpass/.env`

Please change the Shadowsocks [password](https://www.lastpass.com/password-generator) immediately in order to prevent unauthorized access. You can do so by editing `.env` using your preferred text editor and changing the value of `SHADOWSOCKS_PASSWORD`

Please use a long alphanumeric password to avoid parsing errors (numbers and letters only).

Once done, recreate the container:
```
cd /opt/underpass
docker-compose up -d --force-recreate shadowsocks
```

***

#### Changing the Shadowsocks Port

The Shadowsocks ports are in `/opt/underpass/.env`. By default, the TCP and UDP ports for Shadowsocks are on `8388`. You can change `SHADOWSOCKS_TCP` and `SHADOWSOCKS_UDP` values to your desired port numbers. Recreate the container once done:
```
cd /opt/underpass
docker-compose up -d --force-recreate shadowsocks
```

***

_Download the Shadowsocks client from the [official site](https://shadowsocks.org/en/download/clients.html)_

***

#### Shadowsocks Client Settings

The first thing to do in the Shadowsocks client is to add a server.

![shadowsocks_server_settings](https://user-images.githubusercontent.com/9207205/94196774-71e5af80-fee7-11ea-8ebc-aff7898b2b5b.png)

For the `Server IP`, input your server's public IP.

For the `Server Port`, input 8388 (default), or the custom port that you assigned in `/opt/underpass/.env` for `SHADOWSOCKS_TCP`.

Input your new Shadowsocks `Password` as well.

For `Encryption`, select `aes-256-gcm` from the drop down. The encryption method is defined in `/opt/underpass/.env` under `SHADOWSOCKS_METHOD`.

Leave the rest of the settings to their defaults. Click `Apply` and `OK`.

#### Connecting to the Shadowsocks Server

The Shadowsocks icon resides in the system tray on Windows 10. Simply open the tray and right click on the Shadowsocks icon. From the context menu, go to `System Proxy`and click on `Global`.

![shadowsocks_client_global](https://user-images.githubusercontent.com/9207205/94197690-b6be1600-fee8-11ea-9515-a00336b0038f.png)

You'll know that you've successfully connected when the icon turns blue. It will also give you the server details when you hover over it.

![shadowsocks_client_connected](https://user-images.githubusercontent.com/9207205/94197932-0f8dae80-fee9-11ea-842f-6a1779e54889.png)
