### Setup

Use docker-compose file to set up the environment.

#### Prerequisites
* Install docker
* Install docker-compose
* Install composer
* Install [modman](https://github.com/colinmollenhour/modman)

#### Project Run
```shell
mkdir -p gambio-project/src
cd gambio-project
# copy gambio GX4_v4.x.x.x\Shopsystem\Dateien contents into src folder
cd src
modman init
modman clone https://github.com/shopgate/cart-integration-gambiogx.git
cd .modman/cart-integration-gambiogx
composer update
modman repair
# copy compose file outside of src folder
cp development/docker-compose.yml ../../../
cd ../../../
docker-composer up -d
```

### DB IP Address

```shell
# figure out the name of mySql container
docker ps
docker inspect [MYSQL CONTAINER NAME] | grep IPAddress
# e.g.  "IPAddress": "172.27.0.2",
```
Depending on the OS you might need the IP address when installing Gambio, it will be entered like this `172.27.0.2:3306`

### Permissions
Because of [bind-mount] we need to adjust the user group:
```shell
chgrp -R www-data src
```
Add permissions to SDK to write files:
```shell
chmod g+w -R src/.modman/cart-integration-gambiogx/src/shopgate/vendor/shopgate/cart-integration-sdk
```
Also use the [permissions](permissions.md) file to adjust `src` file permissions for Gambio to work.

### Resources
* Your docker container setup [Dockware]
* Using [bind-mount]

[Dockware]: https://dockware.io/getstarted
[bind-mount]: https://docs.dockware.io/tips-and-tricks/how-to-use-bind-mounting#3-set-permissions
