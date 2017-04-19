---
title: storm compile & install
date: 2017-04-19 21:21:11
tags:
---
I'm going to compile and install storm of master branch, in which the version of storm is 2.0.0-SNAPSHOT.

### compile
```bash
cd storm
mvn clean install -DskipTests

# create the binary distribution
cd storm-dist/binary
mvn clean package -DskipTests
ls final-package/target
tar zxvf final-package/target/apache-storm-2.0.0-SNAPSHOT.tar.gz -C /usr/local/
```
During creating the binary distribution, there were some mistakes.
* Could not find gpg
  solution: ```brew install gpg```
* gpg: signing failed: private key couldn't be used
  solution: [Create a new GPG key](https://keyring.debian.org/creating-key.html)

### Setting up
