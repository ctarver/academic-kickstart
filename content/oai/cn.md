+++
title = "Installing the Core Network"
date = 2018-12-10T08:41:25-06:00
draft = false
type = "docs"  # Do not modify.

# Show table of contents? true/false
toc = true

# Add menu entry to sidebar.

# Uncomment to customize menu title, e.g. to abbreviate page title.
linktitle = "Core Network"

# Substitute `tutorial` with the name of your tutorials folder.
[menu.oai]
  # Define a parent ID if this page is a parent.
  #name = "YourParentID"
  
  # Reference a parent ID if this page is a child.
  parent = "Main Guide"
  
  # Order that this page appears in the menu.
  weight = 1
+++

There are many recent changes to openair-cn that make many current guides to installing the openair-cn repository difficult to follow. I will document the steps here for how I am building my openair-cn.

We will use the latest commit of the [develop branch of the openair-cn repository on Github](https://github.com/OPENAIRINTERFACE/openair-cn/tree/develop) which is commit 1c3730e0623be220aa680ba87387ddd5cb96ac69. There is currently a [wiki on the Github](https://github.com/OPENAIRINTERFACE/openair-cn/wiki/Basic-Deployment-of-vEPC) that I recommend you read over. My guide is based on that. 

{{% alert warning %}}
This guide currently doesn't work fully. I am in the process of developing it and debugging my setup. Please see my [current debugging page]({{< ref "oai/debugging.md" >}}) for info on where this is breaking. 
{{% /alert %}}

## Computer Setup
Setup a computer with Ubuntu. I am currently using [Ubuntu 16.04](http://releases.ubuntu.com/16.04/), but it shouldn't matter for the core network since we will be putting it on a virtual box anyway. I think Ubuntu 16.04 is the better choice here since it is more compatible with the eNB and you may want to do an all-in-one setup. 

### Virtualbox Setup
We will create a virtualbox running Ubuntu 18.04 to actually run the core network. 
* [Download an iso of Ubuntu 18.04](http://releases.ubuntu.com/18.04/)
* Install virtualbox. Open a terminal and put in the following:
```bash
sudo apt install virtualbox
```

* Launch Virtualbox from the app launcher. A GUI will pop up. Click "New." Name your virtualbox something like "Ubuntu1804." Make sure that "Type" is set to "Linux," and "Version" is set to "Ubuntu (64 bit)." Click the "Next" button.

* Set the Memory size to 8192 MB. This is what the openair-cn wiki uses. Click "Next."

* Choose "Create a virtual hard disk now." Click "Create." Set the disk file type to "VDI" and click "Next." Keep it "Dynamically allocated." Give it 40 GB of storage. This is what the openair-cn uses. Click "Create." This will close the wizard for creating the new virtualbox.

* Right-click on your new virtualbox and choose "Settings." Under "System" click the "Processor" tab. Set the number of processors to 4. Choose "Network" on the left-hand menu. Under Adapter 1, enable the network adapter and attach it to the bridged adapter. Choose the network interface that is the main ethernet interface for your computer. Do  this for all 4 network adapters. This has the effect of your virtualbox having 4 seperate network interfaces, each with a unique IP address assigned by your router.

* Launch the virtualbox and select the iso for ubuntu 18.04 that you previously downloaded. Install this with the minimal option. 

## Before Installing the Core Network
Before we install the core network, we need to properly setup our virtualbox. This will consist of fixing the FQDN, setting up the virtual networking, installing the proper kernel for the SPGW, and downloading the latest version of the core network from the github repo.

### Specify the FQDN
```bash
sudo vim /etc/hosts
```
Make it look like this:
```
127.0.0.1   localhost
127.0.1.1   mme.OpenAir5G.Alliance   mme
127.0.1.1   hss.OpenAir5G.Alliance   hss
```
### Setup network
run ifconfig and look at the output. Mine looks like:
```
enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.121  netmask 255.255.255.0  broadcast 192.168.1.255
        ether 08:00:27:fb:08:8d  txqueuelen 1000  (Ethernet)
        RX packets 351529  bytes 124483207 (124.4 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 62633  bytes 4779842 (4.7 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp0s8: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.138  netmask 255.255.255.0  broadcast 192.168.1.255
        ether 08:00:27:ff:da:c4  txqueuelen 1000  (Ethernet)
        RX packets 282342  bytes 28093348 (28.0 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 30171  bytes 2377764 (2.3 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp0s9: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.120  netmask 255.255.255.0  broadcast 192.168.1.255
        ether 08:00:27:d4:d7:42  txqueuelen 1000  (Ethernet)
        RX packets 310038  bytes 69408055 (69.4 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 29684  bytes 2346941 (2.3 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp0s10: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.102  netmask 255.255.255.0  broadcast 192.168.1.255
        ether 08:00:27:cd:3c:98  txqueuelen 1000  (Ethernet)
        RX packets 782048  bytes 550640279 (550.6 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 387804  bytes 37041353 (37.0 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 426942  bytes 35904185 (35.9 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 426942  bytes 35904185 (35.9 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

We need to add a few virtual things. Here's the commands I use:
```bash
sudo ifconfig enp0s10:0 172.66.1.113 up  # For the HSS side of S6a
sudo ifconfig enp0s10:11 172.66.1.111 up # For the MME side of S6a
```
After this, my ifconfig looks like:
```
enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.121  netmask 255.255.255.0  broadcast 192.168.1.255
        ether 08:00:27:fb:08:8d  txqueuelen 1000  (Ethernet)
        RX packets 351529  bytes 124483207 (124.4 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 62633  bytes 4779842 (4.7 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp0s8: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.138  netmask 255.255.255.0  broadcast 192.168.1.255
        ether 08:00:27:ff:da:c4  txqueuelen 1000  (Ethernet)
        RX packets 282342  bytes 28093348 (28.0 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 30171  bytes 2377764 (2.3 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp0s9: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.120  netmask 255.255.255.0  broadcast 192.168.1.255
        ether 08:00:27:d4:d7:42  txqueuelen 1000  (Ethernet)
        RX packets 310038  bytes 69408055 (69.4 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 29684  bytes 2346941 (2.3 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp0s10: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.102  netmask 255.255.255.0  broadcast 192.168.1.255
        ether 08:00:27:cd:3c:98  txqueuelen 1000  (Ethernet)
        RX packets 782048  bytes 550640279 (550.6 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 387804  bytes 37041353 (37.0 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp0s10:0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.66.1.113  netmask 255.255.0.0  broadcast 172.66.255.255
        ether 08:00:27:cd:3c:98  txqueuelen 1000  (Ethernet)

enp0s10:11: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.66.1.111  netmask 255.255.0.0  broadcast 172.66.255.255
        ether 08:00:27:cd:3c:98  txqueuelen 1000  (Ethernet)

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 426942  bytes 35904185 (35.9 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 426942  bytes 35904185 (35.9 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

### Install Kernel 4.9.X
This is needed for the SPGW. It's best to get this started soon because it'll take ~1hr to do. You can work on other steps while the kernel compiles.

```bash
sudo apt install libncurses5-dev libncursesw5-dev bc binutils gcc libssl-dev make autoconf libelf-dev
cd /usr/src; sudo wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.9.108.tar.xz
sudo tar xf linux-4.9.108.tar.xz && cd linux-4.9.108/
sudo make olddefconfig && sudo make -j`nproc`
sudo make modules_install && sudo make install
```

### Download the repo
Clone the openair-cn repo:
```bash
git clone https://github.com/OPENAIRINTERFACE/openair-cn.git
cd openair-cn
git checkout develop
```

## HSS
The HSS install requires us to install a database. We use Cassandra via the install scripts provided in the openair-cn repo. Once this is done, we install the other necessary depedencies for the hss_rel14 then install the actual hss_rel14. Then we populate the HSS cql database with our user info. 

### Cassandra
This is the database that will hold the user info. The old version used mysql, but now this is used.

This bit is exactly as in the [openair-cn wiki](https://github.com/OPENAIRINTERFACE/openair-cn/wiki/Basic-Deployment-of-vEPC#install-cassandra).
Install the dependencies: 
```bash
cd scripts
./build_cassandra --check-installed-software --force
```

Verify that Cassandra is installed and running:
```bash
nodetool status
```
My output looked like this:
```bash
Datacenter: datacenter1
=======================
Status=Up/Down
|/ State=Normal/Leaving/Joining/Moving
--  Address    Load       Tokens  Owns (effective)  Host ID                               Rack
UN  127.0.0.1  51.66 KB   256     100.0%            b4ebbe2a-4843-4bfe-922c-532dd99d182b  rack1
```
Stop Cassandra and cleanup the log files before modifying the configuration

```bash
sudo service cassandra stop
sudo rm -rf /var/lib/cassandra/data/system/*
sudo rm -rf /var/lib/cassandra/commitlog/*
sudo rm -rf /var/lib/cassandra/data/system_traces/*
sudo rm -rf /var/lib/cassandra/saved_caches/*
```

Update the config file:
```bash
sudo vim /etc/cassandra/cassandra.yaml
```
Change the cluster name to "HSS Cluster" and the endpoint_snitch to "GossipingPropertyFileSnitch".

Start the Cassandra Service.
```bash
sudo service cassandra start
```
### Install HSS software dependencies
Install the dependencies for the hss_rel_14.
```bash
./build_hss_rel14 --check-installed-software --force
```

### Build HSS
```bash
./build_hss_rel14 --clean
```

### Populate the user tables
I am using SIM cards provided by [opencells](https://open-cells.com/). They have a corenetwork sql file for the old hss that goes with their SIM cards. The commands below are for my SIM cards. Modify each as necessary.
```bash
Cassandra_Server_IP='127.0.0.1'
cqlsh --file ../src/hss_rel14/db/oai_db.cql $Cassandra_Server_IP
./data_provisioning_users --apn default --apn2 internet --key 6874736969202073796d4b2079650a73 --imsi-first 208920100001100 --msisdn-first 33638020000 --mme-identity mme.OpenAir5G.Alliance --no-of-users 20 --realm OpenAir5G.Alliance --truncate True --verbose True --cassandra-cluster $Cassandra_Server_IP
./data_provisioning_mme --id 1 --mme-identity mme.OpenAir5G.Alliance --realm OpenAir5G.Alliance --ue-reachability 1 --truncate True  --verbose True
```

## MME
### Install software dependencies
```bash
cd ~/openair-cn/scripts
./build_mme --check-installed-software --force
```

### Build MME
```bash
./build_mme --clean
```

## SPGW
### Install software dependencies
```bash
cd ~/openair-cn/scripts
./build_spgw --check-installed-software --force
```

### Build SPGW
```bash
./build_spgw --clean
```

## Configure Everything
The configurations are found in openair-cn/etc. Here, I have mine. The mme, hss_rel14, and spgw.conf files (I added a .txt after them so the broswer would display them without trying to download them) go in "/usr/local/etc/oai/." The acl.conf and things that end in fd.conf go in "/usr/local/etc/oai/freeDiameter/."

```bash
cp ../etc/hss_rel14.conf /usr/local/etc/oai
cp ../etc/hss_rel14.json /usr/local/etc/oai
cp ../etc/mme.conf /usr/local/etc/oai
cp ../etc/spgw.conf /usr/local/etc/oai

cp ../etc/acl.conf /usr/local/etc/oai/freeDiameter
cp ../etc/hss_rel14_fd.conf /usr/local/etc/oai/freeDiameter
cp ../etc/mme_fd.conf /usr/local/etc/oai/freeDiameter
```

I recomend copying the original files then going in and filling in the settings. One good way to do this is to use a tool such as [BeyondCompare](https://www.scootersoftware.com/). You can compare the files in /usr/local/etc/oai with my configuration files below.

* [cassandra.conf](../cassandra.conf.txt)
* [hss_rel14.conf](../hss_rel14.conf.txt)
* [hss_rel14.json](../hss_rel14.json)
* [mme.conf](../mme.conf.txt)
* [spgw.conf](../spgw.conf.txt)
* [acl.conf](../acl.conf.txt)
* [hss_rel14_fd.conf](../hss_rel14_fd.conf.txt)
* [mme_fd.conf](../mme_fd.conf.txt)

### Get FreeDiameter Certificates

```bash
cd ~/openair-cn/scripts
sudo ./check_mme_s6a_certificate /usr/local/etc/oai/freeDiameter mme.OpenAir5G.Alliance
../src/hss_rel14/bin/make_certs.sh hss OpenAir5G.Alliance /usr/local/etc/oai
```

### Load the OP key
```bash
oai_hss -j /usr/local/etc/oai/hss_rel14.json --onlyloadkey
```

This doesn't compute the OPc I want (I want it to match the OpenCells sql database). To fix this, we'll manually update the OPc in the database. 

```sql
cqlsh
USE vhss;
UPDATE users_imsi SET OPc='504f20634f6320504f50206363500a4f'WHERE imsi='208920100001101';
```
You'll need to do the above UPDATE for every imsi that you are using. I'd like it if I could put an OPc in hss_rel24.json instead of the OP. I'd also like to find a way to do the UPDATE cql command over the entire column. 

## Run Everything
{{% alert warning %}}
Before running everything, make sure you are using the 4.9.108 kernel we installed. 
{{% /alert %}}
We'll need three separate terminals. Start with the hss.
```bash
oai_hss -j /usr/local/etc/oai/hss_rel14.json
```
Then the mme:
```bash
cd ~/openair-cn/scripts
./run_mme 
```
And now the spgw:
```bash
cd ~/openair-cn/scripts
sudo -E ./run_spgw
```
