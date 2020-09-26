# Portainer

### Initial Configuration

#### Portainer: _http://ip_of_server:9000_

![portainer_initial_setup](https://user-images.githubusercontent.com/9207205/93722499-d47a3b00-fbc9-11ea-8754-ddc698e6dd48.png)

Set an admin password (please set an extremely strong [password](https://www.lastpass.com/password-generator))

***

#### Container List, State, and Quick Actions

Once you're in the Portainer Dashboard, you'll want to go to `Containers` to check on their state. Notice the `Quick Actions` column as well.

![portainer_container_list_quick_actions](https://user-images.githubusercontent.com/9207205/94192321-5c6d8700-fee1-11ea-99c0-0a458c02997f.png)

***

#### Logs

The `Quick Actions` column allows you to view the logs of a particular container/service in realtime.

![portainer_container_logs](https://user-images.githubusercontent.com/9207205/94192701-e4539100-fee1-11ea-8d4f-dfe67e5fd19e.png)

***

#### Stats

From `Quick Actions`, you can also view how much RAM and CPU a particular container is using.

![portainer_container_stats](https://user-images.githubusercontent.com/9207205/94192912-2d0b4a00-fee2-11ea-81ba-66aa23627e86.png)

***

#### Console

If you need to access a container's terminal, such as when creating a user for Dante, you can do so from the container console.

![portainer_container_console](https://user-images.githubusercontent.com/9207205/94193478-fd107680-fee2-11ea-9bc2-08f4e6f50fe6.png)

***

#### Container Details

You can also click on the container name and access the Logs, Stats, and Console from there.

![portainer_container_details](https://user-images.githubusercontent.com/9207205/94193706-3fd24e80-fee3-11ea-81f6-cd3bcb58ba57.png)

***

#### Images
The `Images` page allows you to remove an unused Docker image, or upgrade an existing image.

![portainer_image_list](https://user-images.githubusercontent.com/9207205/94193941-93449c80-fee3-11ea-9cea-0a4fc06e8c46.png)

***

#### Volumes
The `Volumes` page gives you a list of the persistent storage that the containers are using. Persistent storage means that the data is retained even if a container is removed or recreated.

For instance, if you create a `Server` or a `User` from the Pritunl web panel, the data is stored in the `underpass_mongodb` volume. You can remove or delete the `pritunl container` all you want, but your login credentials, users, and VPN servers will remain intact. However, if you delete the `underpass_mongodb` volume, all your data is lost as well.

![portainer_volumes](https://user-images.githubusercontent.com/9207205/94195431-93459c00-fee5-11ea-9041-bd32333333b3.png)

The `Volumes` page also allows you to see the exact location of the volumes on the Docker host. If you want to make a backup of your persistent storage, you can find them in `/var/lib/docker/volumes`.