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
Command 'java' not found, but can be installed with:

apt install default-jre
apt install openjdk-11-jre-headless
apt install openjdk-8-jre-headless
```

### Execute the following command to install the default Java Runtime Environment (JRE), which will install the JRE from OpenJDK 11:

```sh
$ sudo apt install default-jre
```

### The JRE will allow you to run almost all Java software.

Verify the installation with:

```sh
$ java -version
```

### You’ll see the following output:

```sh
openjdk version "11.0.7" 2020-04-14
OpenJDK Runtime Environment (build 11.0.7+10-post-Ubuntu-2ubuntu218.04)
OpenJDK 64-Bit Server VM (build 11.0.7+10-post-Ubuntu-2ubuntu218.04, mixed mode, sharing)
```

### You may need the Java Development Kit (JDK) in addition to the JRE in order to compile and run some specific Java-based software. To install the JDK, execute the following command, which will also install the JRE:

```sh
$ sudo apt install default-jdk
```

### Verify that the JDK is installed by checking the version of javac, the Java compiler:

```sh
$ javac -version
```

## Install Maven

### Execute the following command to install maven

```sh
$ sudo apt install maven
```
### Verify Maven installation

```sh
$ mvn -version
```

### You’ll see the following output:

```sh
Apache Maven 3.6.0
Maven home: /usr/share/maven
Java version: 11.0.10, vendor: Ubuntu, runtime: /usr/lib/jvm/java-11-openjdk-amd64
Default locale: en, platform encoding: UTF-8
OS name: "linux", version: "5.4.0-1037-gcp", arch: "amd64", family: "unix"
```


for more information about JAVA installation visit https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-18-04
