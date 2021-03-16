# Install YCSB and configure for MongoDB

## Download the latest release of YCSB.

```sh
$ curl -O --location https://github.com/brianfrankcooper/YCSB/releases/download/0.17.0/ycsb-0.17.0.tar.gz
$ tar xfvz ycsb-0.17.0.tar.gz 
$ cd ycsb-0.17.0
```
## Install JAVA.

### Check if Java is already installed

```sh
$ java -version
```
### If Java is not currently installed, you’ll see the following output:

```sh
$ Command 'java' not found, but can be installed with:

$ apt install default-jre
$ apt install openjdk-11-jre-headless
$ apt install openjdk-8-jre-headless
```
