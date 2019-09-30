+++
title = "Issues I Had and How I Fixed Them"
date = 2018-12-13T10:41:25-06:00
draft = false
type = "docs"  # Do not modify.

# Show table of contents? true/false
toc = true

# Add menu entry to sidebar.

# Uncomment to customize menu title, e.g. to abbreviate page title.
linktitle = "Issues I Had"

# Substitute `tutorial` with the name of your tutorials folder.
[menu.oai]
  # Define a parent ID if this page is a parent.
  #name = "YourParentID"
  
  # Reference a parent ID if this page is a child.
  parent = "Main Guide"
  
  # Order that this page appears in the menu.
  weight = 4
+++

On this page, I document the issues I had when setting things up, and the steps I took to overcome them.

## MME Loading 0.0.0.0 IP addresses

### Problem
In the mme log, I was noticing that many of the interfaces were loading as 0.0.0.0 even though I had IP addresses listed for these interfaces. Below is an exerpt from the beginning of a log where you can see that.
```
[0m[0;36m000028 00000:167141 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1046    - S1-MME:
[0m[0;36m000029 00000:167144 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1047        port number ......: 36412
[0m[0;36m000030 00000:167147 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1048    - IP:
[0m[0;36m000031 00000:167150 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1049        s1-MME iface .....: (null)
[0m[0;36m000032 00000:167154 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1050        s1-MME ip ........: 0.0.0.0
[0m[0;36m000033 00000:167158 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1051        s11 MME iface ....: (null)
[0m[0;36m000034 00000:167161 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1052        s11 MME port .....: 2123
[0m[0;36m000035 00000:167164 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1053        s11 MME ip .......: 0.0.0.0
[0m[0;36m000036 00000:167168 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1054        s10 MME iface ....: (null)
[0m[0;36m000037 00000:167171 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1055        s10 MME port .....: 2123
[0m[0;36m000038 00000:167174 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1056        s10 MME ip .......: 0.0.0.0
[0m[0;36m000039 00000:167177 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1057    - ITTI:
[0m[0;36m000040 00000:167181 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1058        queue size .......: 2000000 (bytes)
[0m[0;36m000041 00000:167184 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1059        log file .........: (null)
[0m[0;36m000042 00000:167187 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1060    - SCTP:
[0m[0;36m000043 00000:167190 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1061        in streams .......: 8
[0m[0;36m000044 00000:167193 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1062        out streams ......: 8
[0m[0;36m000045 00000:167196 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1063    - GUMMEIs (PLMN|MMEGI|MMEC):
[0m[0;36m000046 00000:167200 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1066                208.92 |4|1 
[0m[0;36m000047 00000:167203 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1069    - TAIs : (mcc.mnc:tac)
[0m[0;36m000048 00000:167207 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1072    - TAI list type one PLMN consecutive TACs
[0m[0;36m000049 00000:167210 7F3FDCF002C0 INFO  CONFIG nair-cn/src/mme_app/mme_config.c:1084                208. 92:1
```

### Solution
So the issue was that I was commenting out the S10 interface since I wasn't intending to use it. YOU CAN'T COMMENT OUT ANY INTERFACE LIKE THAT! Below is the correct version of the mme.conf
```
    NETWORK_INTERFACES : 
    {
        # MME binded interface for S1-C or S1-MME  communication (S1AP), can be ethernet interface, virtual ethernet interface, we don't advise wireless interfaces
        MME_INTERFACE_NAME_FOR_S1_MME   = "enp0s10";    # YOUR NETWORK CONFIG HERE
        MME_IPV4_ADDRESS_FOR_S1_MME     = "192.168.1.102/24";      # CIDR, YOUR NETWORK CONFIG HERE

        # MME binded interface for S11 communication (GTPV2-C)
        MME_INTERFACE_NAME_FOR_S11      = "enp0s9:11";       # YOUR NETWORK CONFIG HERE
        MME_IPV4_ADDRESS_FOR_S11        = "172.16.1.111/24";         # CIDR, YOUR NETWORK CONFIG HERE
        MME_PORT_FOR_S11                = 2123;                                 # YOUR NETWORK CONFIG HERE


        #S10 Interface BEFORE!!!!!!!!!!!!!!!!!
        #MME_INTERFACE_NAME_FOR_S10      = "@MME_INTERFACE_NAME_FOR_S10@";       # YOUR NETWORK CONFIG HERE
        #MME_IPV4_ADDRESS_FOR_S10        = "@MME_IPV4_ADDRESS_FOR_S10@";         # CIDR, YOUR NETWORK CONFIG HERE
        #MME_PORT_FOR_S10                = 2123;                                 # YOUR NETWORK CONFIG HERE
        
        #S10 Interface AFTER!!!!!!!!!!!!!!!!! DON'T COMMENT OUT
        MME_INTERFACE_NAME_FOR_S10      = "enpos9";       # YOUR NETWORK CONFIG HERE
        MME_IPV4_ADDRESS_FOR_S10        = "192.168.1.120/24";         # CIDR, YOUR NETWORK CONFIG HERE
        MME_PORT_FOR_S10                = 2123;                                 # YOUR NETWORK CONFIG HERE   
    };
```

## MME Not Getting the correct IP Address of the SPGW.
### Problem
The MME wasn't able to communicate with the SPGW. 
```
[0m[0;32m001137 00220:908878 7F3FCE7FC700 DEBUG MME-AP -cn/src/mme_app/mme_app_bearer.c:0948         Received S11_CREATE_SESSION_RESPONSE from S+P-GW
[0m[0;36m001138 00220:908888 7F3FCE7FC700 INFO  NAS-EM ir-cn/src/nas/emm/emm_data_ctx.c:0663         EMM-CTX - get UE id 1 context 0x7f3f44002e00
[0m[0;33m001139 00220:908895 7F3FCE7FC700 WARNI MME-AP -cn/src/mme_app/mme_app_bearer.c:0982         Received S11_CREATE_SESSION_RESPONSE REJECTION with cause value 72 for ue 1from S+P-GW. 
```

### Solution
I had set this up incorrectly. In the old openair-cn versions, you would put this in your mme.conf and be done.
```
S-GW : 
{
    # S-GW binded interface for S11 communication (GTPV2-C), if none selected the ITTI message interface is used
    SGW_IPV4_ADDRESS_FOR_S11                = "172.16.1.114/24";                   # YOUR NETWORK CONFIG HERE
};
```
I noticed that there was a new section called "WRR_LIST_SELECTION" that had an option for the S11 IP address. I didn't know what this was and it looked confusing. I hoped that I could use the old way, and it would support it. Nope. This was the give away in the mme log.
```
[0m[0;32m001074 00214:906786 7F3FCE7FC700 DEBUG MME-AP /mme_app/mme_app_wrr_selection.c:0096            Service lookup tac-lb01.tac-hb00.tac.epc.mnc092.mcc208.3gppnetwork.org returned 0.0.0.0
[0m[0;32m001075 00214:906788 7F3FCE7FC700 DEBUG MME-AP mme_app/mme_app_itti_messaging.c:0344            Sending CSR for imsi (2) 208920100001101
[0m[0;37m001076 00214:906795 7F3FCE7FC700 TRACE MME-AP mme_app/mme_app_itti_messaging.c:0346         Leaving mme_app_send_s11_create_session_req() (rc=0)
[0m[0;37m001077 00214:906797 7F3FCE7FC700 TRACE MME-AP -cn/src/mme_app/mme_app_bearer.c:0190      Leaving mme_app_handle_nas_pdn_connectivity_req() (rc=0)
[0m[0;37m001078 00214:906809 7F3FCF7FE700 TRACE S11    rc/s11/s11_mme_session_manager.c:0070    Entering s11_mme_create_session_request()
[0m[0;32m001079 00214:906816 7F3FCF7FE700 DEBUG GTPv2- /nwgtpv2c-0.11/src/NwGtpv2cMsg.c:0096      Created message 0x7f3f40001340!
[0m[0;37m001080 00214:906830 7F3FCF7FE700 TRACE GTPv2- 2-c/nwgtpv2c-0.11/src/NwGtpv2c.c:1498      Entering nwGtpv2cProcessUlpReq()
[0m[0;32m001081 00214:906833 7F3FCF7FE700 DEBUG GTPv2- 2-c/nwgtpv2c-0.11/src/NwGtpv2c.c:1502         Received initial request from ulp
[0m[0;37m001082 00214:906835 7F3FCF7FE700 TRACE GTPv2- 2-c/nwgtpv2c-0.11/src/NwGtpv2c.c:0553         Entering nwGtpv2cHandleUlpInitialReq()
[0m[0;32m001083 00214:906838 7F3FCF7FE700 DEBUG GTPv2- nwgtpv2c-0.11/src/NwGtpv2cTrxn.c:0214            Created transaction 0x7f3f400047c0
[0m[0;33m001084 00214:906840 7F3FCF7FE700 WARNI GTPv2- 2-c/nwgtpv2c-0.11/src/NwGtpv2c.c:0573            Request message received on non-existent teid 0x40110050 from peer 0x0 received! Discarding.
[0m[0;37m001085 00214:906844 7F3FCF7FE700 TRACE GTPv2- 2-c/nwgtpv2c-0.11/src/NwGtpv2c.c:0481            Entering nwGtpv2cCreateLocalTunnel()
[0m[0;32m001086 00214:906847 7F3FCF7FE700 DEBUG GTPv2- 2-c/nwgtpv2c-0.11/src/NwGtpv2c.c:0483               Creating local tunnel with teid '0x50001140' and peer IP 0.0.0.0
[0m[0;37m001087 00214:906850 7F3FCF7FE700 TRACE GTPv2- 2-c/nwgtpv2c-0.11/src/NwGtpv2c.c:0502            Leaving nwGtpv2cCreateLocalTunnel() (rc=0)
[0m[0;37m001088 00214:906857 7F3FCF7FE700 TRACE GTPv2- 2-c/nwgtpv2c-0.11/src/NwGtpv2c.c:1694            Entering nwGtpv2cStartTimer()
[0m[0;32m001089 00214:906866 7F3FCF7FE700 DEBUG GTPv2- 2-c/nwgtpv2c-0.11/src/NwGtpv2c.c:1748               Started timer 0x7f3f40001150 for info 0x0x7f3f40004890!
[0m[0;37m001090 00214:906869 7F3FCF7FE700 TRACE GTPv2- 2-c/nwgtpv2c-0.11/src/NwGtpv2c.c:1754            Leaving nwGtpv2cStartTimer() (rc=0)
[0m[0;37m001091 00214:906872 7F3FCF7FE700 TRACE GTPv2- 2-c/nwgtpv2c-0.11/src/NwGtpv2c.c:0614         Leaving nwGtpv2cHandleUlpInitialReq() (rc=0)
[0m[0;37m001092 00214:906874 7F3FCF7FE700 TRACE GTPv2- 2-c/nwgtpv2c-0.11/src/NwGtpv2c.c:1550      Leaving nwGtpv2cProcessUlpReq() (rc=0)
[0m[0;37m001093 00214:906877 7F3FCF7FE700 TRACE S11    rc/s11/s11_mme_session_manager.c:0163    Leaving s11_mme_create_session_request() (rc=0)
[0m[0;32m001094 00214:906885 7F3FD4F16700 DEBUG UDP    /src/udp/udp_primitives_server.c:0090    Looking for task 8
[0m[0;31m001095 00214:906888 7F3FD4F16700 ERROR UDP    /src/udp/udp_primitives_server.c:0313    Failed to retrieve the udp socket descriptor associated with task 8
```

So what I did was copy the wrr id from the log into the config file. So my config looked like this:
```
    WRR_LIST_SELECTION = (
        {ID="tac-lb01.tac-hb00.tac.epc.mnc092.mcc208.3gppnetwork.org" ;      SGW_IPV4_ADDRESS_FOR_S11="172.16.1.114/24";}
    );

```

## UDP Issues
### Problem
Now that my IP addresses were loading correctly, I was hoping to get a working S11 interface. However, I was still seeing issues in the mme and spgw logs where the udp couldn't bind the sockets.

```
[0m[0;32m000116 00001:026305 7F65771D52C0 DEBUG S11    penair-cn/src/s11/s11_mme_task.c:0329    Initializing S11 interface
[0m[0;32m000117 00001:029522 7F656AFFD700 DEBUG UDP    /src/udp/udp_primitives_server.c:0147    Creating new listen socket on address 192.168.1.120 and port 2123
[0m[0;32m000118 00001:029569 7F656AFFD700 DEBUG UDP    /src/udp/udp_primitives_server.c:0176    Inserting new descriptor for task 7, sd 37
[0m[0;32m000119 00001:029601 7F656AFFD700 DEBUG UDP    /src/udp/udp_primitives_server.c:0192    Received 1 events
[0m[0;32m000120 00001:030867 7F65771D52C0 DEBUG S11    penair-cn/src/s11/s11_mme_task.c:0316    Tx UDP_INIT IP addr 172.16.1.111:2123
[0m[0;32m000121 00001:030896 7F65771D52C0 DEBUG S11    penair-cn/src/s11/s11_mme_task.c:0373    Initializing S11 interface: DONE
[0m[0;32m000122 00001:030930 7F656AFFD700 DEBUG UDP    /src/udp/udp_primitives_server.c:0147    Creating new listen socket on address 172.16.1.111 and port 2123
[0m[0;31m000123 00001:030974 7F656AFFD700 ERROR UDP    /src/udp/udp_primitives_server.c:0153    Socket bind failed (Cannot assign requested address) for address 172.16.1.111 and port 2123
[0m[0;32m000125 00001:031006 7F656AFFD700 DEBUG UDP    /src/udp/udp_primitives_server.c:0192    Received 1 events
```


```
[0m[0;32m000106 00000:373569 7F3088EDC7C0 DEBUG SPGW-A me/openair-cn/src/sgw/sgw_task.c:0288    Initializing SPGW-APP task interface: DONE
[0m[0;32m000107 00000:373706 7F307F6CD700 DEBUG UDP    _sgw/udp/udp_primitives_server.c:0151    Creating new listen socket on address 172.16.1.114 and port 2123
[0m[0;31m000108 00000:373786 7F307F6CD700 ERROR UDP    _sgw/udp/udp_primitives_server.c:0157    Socket bind failed (Address already in use) for address 172.16.1.114 and port 2123
[0m[0;32m000109 00000:373847 7F307F6CD700 DEBUG UDP    _sgw/udp/udp_primitives_server.c:0196    Received 1 events
```

### Solution (kinda)
I don't know why I couldn't bind the socket properly. Instead of fixing this, I went back to the way I used to do it in the old core network.
Currently, the [openair-cn guide](https://github.com/OPENAIRINTERFACE/openair-cn/wiki/Basic-Deployment-of-vEPC) encourages everything to be on its own interface an IP. This helps give better isolation. 
However, I'm really not great at setting up all the virtual interfaces and stuff. 

I just went back to my old config. The MME S11 is on 127.0.0.20/8 and the SPGW is on 127.0.0.30/8. This made it to where they could communicate with no UDP socket binding issues.
