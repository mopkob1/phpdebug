# Container to debug php-scripts на php v.8x

#### **XDebug 3 only started working out of the box with [PhpStorm version 2020.3](https://www.jetbrains.com/phpstorm/whatsnew/2020-3/#version-2020-3-xdebug-3 "PHPstorm 2020.3")**

Reference article - [Configure Xdebug](https://www.jetbrains.com/help/phpstorm/configuring-xdebug.html "Configure Xdebug﻿")

The list of changes from XDebug 2 to 3 are available here for reference - [Upgrade Guide](https://xdebug.org/docs/upgrade_guide "Upgrade Guide")

## Configuring PhpStorm

Go to PhpStorm -> Settings -> Languages & Frameworks -> PHP -> Servers
1) Click "+"
2) Name ```ddebug``` (Same as serverName under PHP_IDE_CONFIG environment variable)
3) Host ```_```
4) Default 80
5) Debugger Xdebug
6) Check the checkbox next to "Use path mappings"
7) Modify the absolute path on the server to ```/var/www/html```

## Running the CLI Command
1) Add breakpoints file
2) In PhpStorm click the icon to "Start Listening for PHP Debug connections"
3) Run in the docker file:
```shell
 docker exec debug php index.php
```
4) Run without nginx:
```shell
SCRIPT_NAME=/index.php \
SCRIPT_FILENAME=/var/www/html/index.php \
REQUEST_METHOD=GET \
cgi-fcgi -bind -connect 127.0.0.1:9035
```

```shell
# Install cgi-fcgi:
sudo apt update && sudo apt install libfcgi0ldbl
```

## Troubleshooting
1) Check firewall or selinux if on linux
2) The configuration ```host.docker.internal``` only became available under Mac and Windows with Docker version `20.04`

Add to docker-compose follow lines:  
```shell
extra_hosts:
  - "host.docker.internal:host-gateway"
```

Change attrs:
```shell
mkdir -p /tmp/xdebug
chmod -R 777 /tmp/xdebug
```