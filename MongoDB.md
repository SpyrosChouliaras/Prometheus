# Install MongoDB in Ubuntu bionic 18.04


## System Update

```sh
$ sudo apt update
```
## Import the public key used by the package management system.

```sh
$ wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
```

## Create a list file for MongoDB

```sh
$ echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
```

## Reload local package database.


```sh
$ sudo apt-get update
```
## Install the MongoDB packages.

```sh
$ sudo apt-get install -y mongodb-org
```
## Start MongoDB.

```sh
$ sudo systemctl start mongod
```

## See MongoDB status.

```sh
$ sudo systemctl status mongod
```

## Restart MongoDB.
```sh
$ sudo systemctl restart mongod
```

## Stop MongoDB.
```sh
$ sudo systemctl stop mongod
```




