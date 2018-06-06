---
title: storm compile & install
date: 2017-04-19 21:21:11
categories:
  - jstorm
tags:
  - storm
---
I'm going to compile and install storm of master branch, in which the version of storm is 2.0.0-SNAPSHOT.

### compile
We choose [Apache Strom](http://storm.apache.org "Apache Storm") and [Alibaba Jstorm](http://jstorm.io "Alibaba Jstorm") for compiling.

1. compile Apache Storm
```bash
git clone https://github.com/apache/storm.git
cd storm
# build apache storm
mvn clean install -DskipTests

# create the binary distribution
cd storm-dist/binary
mvn clean package -DskipTests
ls final-package/target
tar zxvf final-package/target/apache-storm-2.0.0-SNAPSHOT.tar.gz -C /usr/local/
```

2. compile Alibaba JStorm
```bash
git clone https://github.com/alibaba/jstorm.git
cd jstorm
# build jstorm
mvn clean install -DskipTests

# build install tar
mvn package assembly:assembly -DskipTests
```

During creating the binary distribution of apache storm, there were some mistakes.
* Could not find gpg
  solution: ```brew install gpg```
* gpg: signing failed: private key couldn't be used
  solution: [Create a new GPG key](https://keyring.debian.org/creating-key.html)

### Setting up
