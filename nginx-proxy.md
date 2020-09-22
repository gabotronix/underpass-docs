# Nginx Proxy Manager

### Initial Configuration

_Note: It is assumed that you already know about how DNS, Domains, and Subdomains work._

The web panel for [Nginx Proxy Manager](https://github.com/jlesage/docker-nginx-proxy-manager) is at http://ip_of_your_server:8181

The initial login credentials are:
```
Email address: admin@example.com
Password: changeme
```

Once you're in, please change the Admin email address and password right away.

***

#### Adding a Proxy Host

Nginx Proxy Manager is used when you have a domain name, and you want it to point to a particular container that has web access:

- Pritunl - https://ip_of_server:4433
- Portainer - http://ip_of_server:9000
- Netdata - http://ip_of_server:19999
- Heimdall - http://ip_of_server:85
- Droppy - http://ip_of_server:8989
- Mongo-Express - http://ip_of_server:8081
- Nginx Proxy Manager - http://ip_of_server:8181

For example, you want `pritunl.underpass.cf` to point to your _Pritunl_ access URL at `https://ip_of_server:4433`

From the web panel, go to `Hosts > Proxy Hosts > Add Proxy Host`

![nginx_proxy_add](https://user-images.githubusercontent.com/9207205/93934268-7b3f1280-fd55-11ea-89c7-2e2dec8f8545.png)

You will then be asked for the following info:
```
Domain Names: pritunl.underpass.cf
Scheme: https
Forward Hostname/IP: the name of the container (pritunl*)
Forward Port: the port of the container that Docker uses internally (443*)
Click on _Save_
```
_* [Refer to Identifying Container Names and Published Ports](https://github.com/gabotronix/underpass-docs/blob/draft/containers.md). The number to the right of the colon is the one to look for._

![nginx_portainer_port_list](https://user-images.githubusercontent.com/9207205/93934302-88f49800-fd55-11ea-8e3c-d0daf2eceff8.png)

Once the entry is added, you'll be able to access Pritunl using the subdomain, `https://pritunl.underpass.cf`

If you want to remove the `Your connection is not private` warning from your web browser, you can do so by going to `SSL Certificates > Add SSL Certificate`. It may take a couple of minutes to register the SSL certificate.

![nginx_ssl_letsencrypt](https://user-images.githubusercontent.com/9207205/93936143-484a4e00-fd58-11ea-838f-3fe3c17edb99.png)

Once you have the SSL Certificate listed, go back to `Hosts > Proxy Hosts` and edit your domain/subdomain by clicking on the three dots on the right. From the `Edit Proxy Host` window, go to the `SSL` tab and select the SSL certificate that was registered earlier.

![nginx_proxy_ssl_pair](https://user-images.githubusercontent.com/9207205/93936363-abd47b80-fd58-11ea-9f96-35eb5371547b.png)

You can also click on `Force SSL` to activate automatic https access.

Finally, click on _Save_. You should now be able to access your domain/subdomain without the web browser warning message.

***