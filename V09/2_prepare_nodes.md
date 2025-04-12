# Setup on all Nodes

<!-- # Important

We need to modify the `/etc/hosts` file.

As user `student`, modify the `/etc/hosts` file

`sudo nano /etc/hosts`



TODO // /etc/cloud/templates/hosts.debian.tmpl


e.g. from:

```bash
127.0.0.1       bdlc-XX.labservices.ch bdlc-XX localhost

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

to:

```bash

127.0.0.1 localhost

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

We also need to change the dns suffix.

```bash
sudo vim /etc/netplan/50-cloud-init.yaml
```

e.g. from:

```bash
# This file is generated from information provided by the datasource.  Changes
# to it will not persist across an instance reboot.  To disable cloud-init's
# network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    version: 2
    ethernets:
        eth0:
            addresses:
            - 10.164.150.116/24
            match:
                macaddress: bc:24:11:67:d3:4d
            nameservers:
                addresses:
                - 1.1.1.1
                search:
                - ls.eee.intern
            routes:
            -   to: default
                via: 10.164.150.254
            set-name: eth0
```

to:

```bash
# This file is generated from information provided by the datasource.  Changes
# to it will not persist across an instance reboot.  To disable cloud-init's
# network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    version: 2
    ethernets:
        eth0:
            addresses:
            - 10.164.150.116/24
            match:
                macaddress: bc:24:11:67:d3:4d
            nameservers:
                addresses:
                - 1.1.1.1
                search:
                - labservices.ch
            routes:
            -   to: default
                via: 10.164.150.254
            set-name: eth0
```

Apply the changes with:

```bash
sudo netplan apply
```

And see if you can ping bdlc-16:

```bash
ping bdlc-16
PING bdlc-16.labservices.ch (10.164.150.116) 56(84) bytes of data.
64 bytes from 10.164.150.116 (10.164.150.116): icmp_seq=1 ttl=64 time=1.24 ms
^C # stop it with ctrl+c
``` -->

## JAVA Version

As we just use Spark, we can go to a newer Java versions now. We have to pick the default one. Execute:

```bash
sudo update-alternatives --config java
```

and choose the `/usr/lib/jvm/java-17-openjdk-amd64/bin/java` by providing the right number:

```text
There are 2 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                         Priority   Status
------------------------------------------------------------
  0            /usr/lib/jvm/java-17-openjdk-amd64/bin/java   1711      auto mode
* 1            /usr/lib/jvm/java-11-openjdk-amd64/bin/java   1111      manual mode
  2            /usr/lib/jvm/java-17-openjdk-amd64/bin/java   1711      manual mode

Press <enter> to keep the current choice[*], or type selection number: 0
```

The Java version should now be `17.0.14`:

```bash
java -version
```

```text
openjdk 17.0.14 2025-01-21
OpenJDK Runtime Environment (build 17.0.14+7-Debian-1deb12u1)
OpenJDK 64-Bit Server VM (build 17.0.14+7-Debian-1deb12u1, mixed mode, sharing)
```

## User

We are going to create a new user for the cluster setup. This user will be used for your project work.

As user `student`.

```bash
sudo adduser cluster
```

## Namenode & Datanode Directories

```bash
sudo mkdir -p /data/hdfscluster/namenode
sudo mkdir -p /data/hdfscluster/datanode
```

and give the `cluster` user the access rights to the folders:

```bash
sudo chown cluster:root -R /data/hdfscluster/
```

## TMP Directory for Spark

```bash
sudo mkdir /data/tmpcluster
sudo chown cluster:root -R /data/tmpcluster/
```

## Switching the User

Now, we will switch to this user. All following steps should be executed as user `cluster`.

```bash
# switch to the user cluster
su - cluster

# change to the home directory
cd ~

# verify "who you are" with
whoami
```

## Hadoop & Spark

Download Hadoop and Spark, extract it and rename.

```bash
cd ~

echo "Hadoop"
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.4.1/hadoop-3.4.1.tar.gz
tar -xvzf hadoop-3.4.1.tar.gz
mv hadoop-3.4.1 hadoop


echo "Spark"
wget https://dlcdn.apache.org/spark/spark-3.5.5/spark-3.5.5-bin-hadoop3.tgz
tar xvf spark-3.5.5-bin-hadoop3.tgz
mv spark-3.5.5-bin-hadoop3 spark

```
