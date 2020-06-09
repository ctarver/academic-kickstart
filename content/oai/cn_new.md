+++
title = "Installing the NEW Core Network"
date = 2019-06-19T08:41:25-06:00
draft = false
type = "docs"  # Do not modify.

# Show table of contents? true/false
toc = true

# Add menu entry to sidebar.

# Uncomment to customize menu title, e.g. to abbreviate page title.
linktitle = "NEW Core Network"

# Substitute `tutorial` with the name of your tutorials folder.
[menu.oai]
  # Define a parent ID if this page is a parent.
  #name = "YourParentID"
  
  # Reference a parent ID if this page is a child.
  parent = "Main Guide"
  
  # Order that this page appears in the menu.
  weight = 1
+++

There are many recent changes to openair-cn so I am updating my guide to account for this. The most 
We will use the latest commit of the [master branch of the openair-cn repository on Github](https://github.com/OPENAIRINTERFACE/openair-cn/) which is commit 96fb2aec8e94f6c78c1bed0b24ee82363e80e7d8 from June 12th, 2019. 
There is currently a [wiki on the Github](https://github.com/OPENAIRINTERFACE/openair-cn/wiki/OpenAirSoftwareSupport) that I recommend you read over. My guide is based on that. 

{{% alert warning %}}
I'M MAKING THIS GUIDE RIGHT NOW!
{{% /alert %}}

## Computer Setup
Setup a computer with Ubuntu. I am currently using [Ubuntu 16.04](http://releases.ubuntu.com/16.04/), but it shouldn't matter for the core network since we will be putting it on a virtual box anyway. I think Ubuntu 16.04 is the better choice here since it is more compatible with the eNB and you may want to do an all-in-one setup. 

### Virtualmachine Setup
We will create a kvm virtual machine running Ubuntu 18.04 to actually run the core network. 

Install virtual machine manager:
```bash
sudo apt install virt-manager 
```
Create the KVM:
```bash
sudo apt -y install uvtool  ## Fpr the cloud image of ubuntu to put into the KVM
uvt-simplestreams-libvirt sync release=bionic arch=amd64
uvt-kvm create --memory 4096 --disk 10 --cpu 2  test-install-openair-cn arch=amd64 release=bionic
uvt-kvm ssh test-install-openair-cn --insecure
```

Launch the "Virtual Machine Manager from the app launcher. The test-install-openair-cn should show up as an option. Start it. 
I like to start it, run `ifconfig` then ssh to the IP from the main host termial. The ssh from the host terminal is much nicer than the default terminal from the virtual machine manager.

### Download the repos
Clone the openair-cn and openair-cn-cups repos:
```bash
git clone https://github.com/OPENAIRINTERFACE/openair-cn.git
git clone https://github.com/OPENAIRINTERFACE/openair-cn-cups.git
```

## Install Everything
I like to get the installations done first then go configure everything. 
All of the build scripts are in the openair-cn/scripts directory.
```bash
cd openair-cn/scripts
```
### Install Cassandra Database for the HSS
There are issues with the current Cassandra script. First, add the public key:
```bash
wget -q -O - https://www.apache.org/dist/cassandra/KEYS | sudo apt-key add -
```


```bash
./build_cassandra --check-installed-software --force
```
### Install HSS
```bash
./build_hss_rel14 --check-installed-software --force
./build_hss_rel14 --clean
```
### Install MME
```bash
./build_mme --check-installed-software --force
./build_mme --clean
```
### Install SPGW
The SPGW is now in the openair-cn-cups repository. 
```bash
cd ~/openair-cn-cups/build/scripts
./build_spgwu -I -f
./build_spgwu -c -V -b Debug -j
./build_spgwc -I -f
./build_spgwc -c -V -b Debug -j
```
## Configure Everything
This is the fun part. 

Throughout this, I find it easier to have some GUI access to the filesystem on the KVM. In Ubunutu, open the file explorer on the host machine, hit "Connect to Server" at the bottom of the left nav-bar. I type "sftp://ubuntu@192.168.122.202/usr/local/etc/oai" to connect to the location where most of these config files will live. 

To make this easy, I also change the permisions on the folder where all the config files will be.

```bash
sudo mkdir /usr/local/etc/oai
sudo chmod 777 -R oai/
```

### Configure Cassandra 
If the ealier installation was good, you should be able to run the following command. 
```bash
nodetool status
```
Stop Cassandra and cleanup the log files before modifying the configuration. This is blatently stolen from the official guide. 
```bash
sudo service cassandra stop
sudo rm -rf /var/lib/cassandra/data/system/*
sudo rm -rf /var/lib/cassandra/commitlog/*
sudo rm -rf /var/lib/cassandra/data/system_traces/*
sudo rm -rf /var/lib/cassandra/saved_caches/*
```

Update /etc/cassandra/cassandra.yaml. HERE is my version. The summary of the changes is below:

* Line 10: change cluster_name to 'HSS Cluster'
* Line 273: `seeds: "127.0.0.1"
* Line 386: `listen_address: localhost`
* Line 444: `rpc_address: localhost`
* Line 705: `endpoint_snitch: GossipingPropertyFileSnitch`

Restart Cassandra:
```bash
sudo service cassandra start
```
### Add in your SIM Info
There are commands (./data_provisioning_users and ./data_provisioning_mme), to help with this.
I am using SIM cards provided by [opencells](https://open-cells.com/). They have a corenetwork sql file for the old hss that goes with their SIM cards. The commands below are for my SIM cards. Modify each as necessary.
```bash
cd
Cassandra_Server_IP='127.0.0.1'
cqlsh --file ../src/hss_rel14/db/oai_db.cql $Cassandra_Server_IP
./openair-cn/scripts/data_provisioning_users --apn default --apn2 internet --key 6874736969202073796d4b2079650a73 --imsi-first 208920100001100 --msisdn-first 33638020000 --mme-identity mme.ng4T.com --no-of-users 20 --realm ng4T.com --truncate True --verbose True --cassandra-cluster $Cassandra_Server_IP
```

### Add MME info to the database
```bash
./openair-cn/scripts/data_provisioning_mme --id 3 --mme-identity mme.ng4T.com --realm ng4T.com --ue-reachability 1 --truncate True  --verbose True -C $Cassandra_Server_IP
```

### Get Certificates:
```bash
cd  # Put yourself in your home dir
./openair-cn/src/hss_rel14/bin/make_certs.sh hss ng4t.com /usr/local/etc/oai                      # For HSS
sudo ./openair-cn/scripts/check_mme_s6a_certificate /usr/local/etc/oai/freeDiameter mme.ng4t.com  # For MME
```

### Copy Files
The configurations are found in openair-cn/etc. Here, I have mine. The mme, hss_rel14, and spgw.conf files (I added a .txt after them so the broswer would display them without trying to download them) go in "/usr/local/etc/oai/." The acl.conf and things that end in fd.conf go in “/usr/local/etc/oai/freeDiameter/.”
```bash
cp ~/openair-cn/etc/hss_rel14.conf   /usr/local/etc/oai
cp ~/openair-cn/etc/hss_rel14.json   /usr/local/etc/oai
cp ~/openair-cn/etc/mme.conf         /usr/local/etc/oai
cp ~/openair-cn-cups/etc/spgw_c.conf /usr/local/etc/oai
cp ~/openair-cn-cups/etc/spgw_u.conf /usr/local/etc/oai

cp ~/openair-cn/etc/acl.conf           /usr/local/etc/oai/freeDiameter
cp ~/openair-cn/etc/hss_rel14_fd.conf  /usr/local/etc/oai/freeDiameter
cp ~/openair-cn/etc/mme_fd.conf        /usr/local/etc/oai/freeDiameter
```
### Fill in the config files
The official guide sets up everything via declaring variables and then performing sed commands. 
I prefer to do this manually. I think it is valuable to open each file, see all the paramters, and to fill them in according to your setup.


### Note on the interfaces.
Nearly everything is done on virtual interfaces. The mme sets up it's virtual interfaces when we run the command to start the MME. 

The virtual interfaces for the SPGW are not set up automatically. These commands turn them on:
```
# Set up interfaces for SPGW-U
sudo ifconfig ens3:sxu 172.55.55.102 up   # SPGW-U SXab interface
sudo ifconfig ens3:s1u 192.168.248.159 up # SPGW-U S1U interface

# Set up interfaces for SPGW-C
sudo ifconfig ens3:sxc 172.55.55.101 up # SPGW-C SXab interface
sudo ifconfig ens3:s5c 172.58.58.102 up # SGW-C S5S8 interface
sudo ifconfig ens3:p5c 172.58.58.101 up # PGW-C S5S8 interface
sudo ifconfig ens3:s11 172.16.1.104 up  # SGW-C S11 interface
```



## Run Everything
We'll need four separate terminals. Start with the hss. I really like to do things in screens, so I don't need to ssh 3 seperate times. 

```bash
screen -S hss # Create a named screen session for the HSS.
oai_hss -j /usr/local/etc/oai/hss_rel14.json  # This is run inside the screen session
```
The HSS will start, then you can exit the screen by hitting ```CTRL + A + D```. 

Then the mme:
```bash
screen -S mme # Create a named screen session for the MME.
cd ~/openair-cn/scripts
./run_mme --set-virt-if
```
The MME will start, then you can exit the screen by hitting ```CTRL + A + D```. 

And now the spgw:
```bash
screen -S spgw_c # Create a named screen session for the SPGW.
cd ~/openair-cn-cups/build/scripts
sudo spgwc -c /usr/local/etc/oai/spgw_c.conf
```
The SPGW_C will start, then you can exit the screen by hitting ```CTRL + A + D```. 
```bash
screen -S spgw_u # Create a named screen session for the SPGW.
cd ~/openair-cn-cups/build/scripts
sudo spgwu -c /usr/local/etc/oai/spgw_u.conf
```
The SPGW_U will start, then you can exit the screen by hitting ```CTRL + A + D```. 

### Monitoring 
You can see all your screens with 
```bash
screen -ls
```

To attach to any and see the current console outputs, 
``` bash
screen -r hss # use hss, mme, spgw_u, or spgw_c
```

### Teardown
Once you are ready to quit, attach to each screen. 
Exit the application via ```CTRL + C```
Then kill the screen via ```CTRL + A ``` then press ```K``` .  Press `y` when it asks if you are serious about quitting. 
