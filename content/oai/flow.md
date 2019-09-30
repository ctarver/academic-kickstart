+++
title = "Flow"
date = 2019-03-04T08:41:14-06:00
draft = false
type = "docs"

# Show table of contents? true/false
toc = true

# Add menu entry to sidebar.

# Uncomment to customize menu title, e.g. to abbreviate page title.
linktitle = "Program Flow"

# Substitute `tutorial` with the name of your tutorials folder.
[menu.oai]
  # Define a parent ID if this page is a parent.
  #name = "YourParentID"
  
  # Reference a parent ID if this page is a child.
  parent = "Other Stuff"
  
  # Order that this page appears in the menu.
  weight = 2
+++
To better understand the flow of the program, it is best to run the eNB with the trace options on in the config. To see my log from running the eNB like this, go [here](../enb_pretty.html).

## CFlow
I recently discovered [cflow](https://www.gnu.org/software/cflow/). I ran this on some of the main functions of OAI by using the following command in the root of the openairinterface5g directory.

```bash
cflow targets/RT/USER/lte-softmodem.c targets/RT/USER/lte-enb.c targets/RT/USER/lte-ru.c targets/RT/USER/lte-softmodem-common.c openair1/PHY/INIT/*.c openair2/ENB_APP/*.c openair2/LAYER2/MAC/*.c  openair2/LAYER2/openair2_proc.c openair2/RRC/LTE/*.c -T 
```
The output is [here](../cflow_out.txt).

## The Main Function
The lte-softmodem starts in the [targets/RT/USER/lte-softmodem.c](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-softmodem.c) file in the main() on [line 594](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-softmodem.c#L594).
We then go read in the command line options in the (load_configmodule function)[https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/common/config/config_load_configmodule.c#L184]
Many InTer Task Interface (ITTI) things are setup.
```
[TMR]   Starting itti queue: TASK_UNKNOWN as task 0
[TMR]   Starting itti queue: TASK_TIMER as task 1
[TMR]   Starting itti queue: TASK_L2L1 as task 2
[TMR]   Starting itti queue: TASK_BM as task 3
[TMR]   Starting itti queue: TASK_PHY_ENB as task 4
[TMR]   Starting itti queue: TASK_MAC_ENB as task 5
[TMR]   Starting itti queue: TASK_RLC_ENB as task 6
[TMR]   Starting itti queue: TASK_RRC_ENB_NB_IoT as task 7
[TMR]   Starting itti queue: TASK_PDCP_ENB as task 8
[TMR]   Starting itti queue: TASK_RRC_ENB as task 9
[TMR]   Starting itti queue: TASK_RAL_ENB as task 10
[TMR]   Starting itti queue: TASK_S1AP as task 11
[TMR]   Starting itti queue: TASK_X2AP as task 12
[TMR]   Starting itti queue: TASK_SCTP as task 13
[TMR]   Starting itti queue: TASK_ENB_APP as task 14
[TMR]   Starting itti queue: TASK_FLEXRAN_AGENT as task 15
[TMR]   Starting itti queue: TASK_PHY_UE as task 16
[TMR]   Starting itti queue: TASK_MAC_UE as task 17
[TMR]   Starting itti queue: TASK_RLC_UE as task 18
[TMR]   Starting itti queue: TASK_PDCP_UE as task 19
[TMR]   Starting itti queue: TASK_RRC_UE as task 20
[TMR]   Starting itti queue: TASK_NAS_UE as task 21
[TMR]   Starting itti queue: TASK_RAL_UE as task 22
[TMR]   Starting itti queue: TASK_MSC as task 23
[TMR]   Starting itti queue: TASK_GTPV1_U as task 24
[TMR]   Starting itti queue: TASK_UDP as task 25
```

These tasks are launched with the [create_tasks function in the main function](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-softmodem.c#L709) which sends us [here](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/COMMON/create_tasks.c#L53) which calls the [create task function](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/common/utils/ocp_itti/intertask_interface.cpp). Here, a new pthread launches a function based on the input arguments.

For example the `TASK_RRC_ENB` launches a thread that sends us [here](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair2/RRC/LTE/rrc_eNB.c#L6850). This task waits for messages and will process them as they come in [here](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair2/RRC/LTE/rrc_eNB.c#L6695).

## Configuration message to RRC task
Early in the program, a message is sent to the `TASK_RRC_ENB` task to configure the RRC.
```
[ENB_APP]   Sending configuration message to RRC task
[TMR]   sent messages id=45 to TASK_RRC_ENB
```
Once the task is set up, it recieves the message in the function I showed at the end of the previous section.
```
[RRC]   Received message RRC_CONFIGURATION_REQ
```
This message sends us to the `openair_rrc_eNB_configuration()` [function.](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair2/RRC/LTE/rrc_eNB.c#L5312)
We pass the configuration into the `init_SI` [function](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair2/RRC/LTE/rrc_eNB.c#L5414) which sends us [here](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair2/RRC/LTE/rrc_eNB.c#L143) in the same rrc_eNB.c file.

Many configuations are copied onto a struct of the form ` RC.rrc[ctxt_pP->module_id]->carrier[CC_id].PARAM` before passing this into the `do_MIB()` [function on line 174](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair2/RRC/LTE/rrc_eNB.c#L174). This function is in [openair2/RRC/LTE/MESSAGES/asn1_msg.c](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair2/RRC/LTE/MESSAGES/asn1_msg.c#L175). Here a pointer to a struct is set up  `LTE_BCCH_BCH_Message_t *mib=&carrier->mib ;`  at the address passed into the function. Based on the N_RB, the mib is set up like `mib->message.dl_Bandwidth = LTE_MasterInformationBlock__dl_Bandwidth_n25;` for 25 RB. Once we return to init_SI() the other system information blocks are set up before we get sent to rrc_mac_config_req_eNB().

The function rrc_mac_config_req_eNB() is [here in the config.c of the MAC directory.](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair2/LAYER2/MAC/config.c#L726). We then go into [config_mib()](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair2/LAYER2/MAC/config.c#809) back on [line 238 in the same file](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair2/LAYER2/MAC/config.c#L238). Here, all the parameters are written to a cfg struct, `  nfapi_config_request_t *cfg = &RC.mac[Mod_idP]->config[CC_idP];` which is a pointer to the address of a property of the [global variable `RC` like I describe on my structs page.](../structs). We return to our rrc_mac_config_req_eNB() and at the [end of the function](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair2/LAYER2/MAC/config.c#L1023) we handover to setting up the PHY layer.

From rrc_mac_config_req_eNB, we enter the phy_config_request function on [line 86 of the lte_init.c](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair1/PHY/INIT/lte_init.c#L86) where the fp struct is set up. This is a pointer to a property of the global RC var, `fp = &RC.eNB[Mod_id][CC_id]->frame_parms;` After copying parameters such as the dl_Bandwidth to the fp, we pass this to [init_frame_parms](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair1/PHY/INIT/lte_init.c#L150).

We are then in the lte_params.c file in the [init_fram_params function](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair1/PHY/INIT/lte_parms.c#L42). Here the ofdm_symbol_size and samples_per_tti are set based on an oversampling factor variable, osf, that was passed in as a constant 1. We then return to the rrc_mac_config_req_eNB function. The next thing is the init_lte_top function in  PHY/INIT/init_top.c which sets up tables such as the 16QAM table. We return and finish setting up the prash and pusch channels before finally returning from rrc_mac_config_req_eNB to the init_SI function which then returns to the openair_rrc_eNB_configuration.

Here we go to [rrc_init_global_param](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair2/RRC/LTE/rrc_eNB.c#L5426) which is in [rrc_common.c](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair2/RRC/LTE/rrc_common.c#L51). After returning, we go to [openair_rrc_top_init_eNB](openair_rrc_top_init_eNB) where we will set an [active flag](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair2/RRC/LTE/rrc_eNB.c#L6888). The very last thing done in our openair_rrc_eNB_configuration() is to call [openair_rrc_on](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair2/RRC/LTE/rrc_eNB.c#L5460) which basically sets [more flags](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair2/RRC/LTE/rrc_eNB.c#L135).

### Summary
```
openair_rrc_eNB_configuration()
    init_SI()
        do_MIB(): Works on RC.rrc[ctxt_pP->module_id]->carrier[CC_id]->->mib
        rrc_mac_config_req_eNB()
	          config_mib(): Works on RC.mac[Mod_idP]->config[CC_idP]
	          phy_config_req(): Works on RC.eNB[Mod_id][CC_id]->frame_parms->N_RB_DL, N_RB_UL, dl_CarrierFreq
                init_frame_parms(): Works on RC.eNB[Mod_id][CC_id]->frame_parms->ofdm_symbol_size, samples_per_tti
                init_lte_top()
    rrc_init_global_param()
    openair_rrc_top_init_eNB()
    openair_rrc_on()
```

## init_RU()
While the above configuration of the RRC is happening, the main thread is waiting. This is shown [here](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-softmodem.c#L821). The flag `RC.eNB[i][j]->configured` is watched and we wait for it to be set. It'll be set in the phy_config_request() function [in lte_init.c](RC.eNB[Mod_id][CC_id]->configured).

After we are done waiting, the init_RU function is called in the [main shown here](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-softmodem.c#L831) which sends us to the [lte-ru.c file](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-ru.c#L2735). 

In the init_RU(), we start by setting up the Radio Unit in [RCconfig_RU()](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-ru.c#L2750) which sends us to the [end of the lte_ru.c file](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-ru.c#L2750). Here the settings specific to the radio unit are copied to the global RC variable, like `RC.ru[j]->att_tx = *(RUParamList.paramarray[j][RU_ATT_TX_IDX].uptr);`

For each radio unit, we set up the local ru variable to be equal to the global like shown below.
``` c
ru               = RC.ru[ru_id];
```

Then the entire frame parameters that was set up earlier is [copied to an eNB struct](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-ru.c#L2793).
```c
        LOG_I(PHY,"Copying frame parms from eNB %d to ru %d\n",eNB0->Mod_id,ru->idx);
        memcpy((void*)&ru->frame_parms,(void*)&eNB0->frame_parms,sizeof(LTE_DL_FRAME_PARMS));
```

We then set some more paramters in the [set_function_spec_param](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-ru.c#L2806) before starting the RU threads in the [init_RU_proc() function](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-ru.c#L2809). This launches many threads including (some of these are conditional):
* [ru_thread](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-ru.c#L2214)
* pre_scd_thread
* eNB_thread_phy_tx
* rf_tx
* ru_thread_tx 
* ru_thread_prach
* ru_stats_thread

We'll look at these in more detail later. For now, we continue going through our main thread. The main thread seems to go back and [wait](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-softmodem.c#L847). Many exciting things seem to be going on in the log file such as setting up the USRP while we wait. Once done, we jump to [init_eNB_after_RU()](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-softmodem.c#L855). Which sends us to [lte-enb.c](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-enb.c#L1175).

Here we go to [phy_init_lte_eNB](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-enb.c#L1186) which sends us to the [PHY/INIT/lte_init.c] (https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair1/PHY/INIT/lte_init.c#L721). We were previously in this file in the function phy_config_request(). 

## ru_thread()
The ru_thread is one of the threads launched [here](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-ru.c#L2214) in the init_RU_proc function which was called in the init_RU() of the main thread.

We go [fill the rf config in the ru struct](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-ru.c#L1661) which happens [here](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-ru.c#L1348). Based on the N_RB, the sample rate is actually set. The fp is used to [help fill the cfg](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-ru.c#L1356).  Other things set here are the tx_bw, rx_bw, samples_per_frame, tx_freq, rx_freq, tx_gain, and rx_gain. Through all of this, we work with cfg, which is just a pointer to a config property of the larger ru struct.
```c
openair0_config_t *cfg   = &ru->openair0_cfg
```

After returning back to the ru_thread, we then can [init_fram_params](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-ru.c#L1662). This sends us into the [PHY/INIT/lte_params.c file](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair1/PHY/INIT/lte_parms.c#L42) again. Here the ofdm symbol size is set along with the oversampling rate which is set to 1. I NEED TO CHECK ON THIS BECAUSE WE HAVE ALREAD GONE INTO THIS FUNCTION, AND I DON'T THINK THERE IS ANY REASON TO DO IT AGAIN. MAYBE THIS IS A FUNCTION THAT IS CALLED PERIODICALLY IN THE CODE??? 

After returing back to the ru_thread, we can call the [phy_init_RU function](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-ru.c#L1663). This sends us to the PHY/INIT/lte_init_ru.c [file](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair1/PHY/INIT/lte_init_ru.c#L36)
Here buffers such as the IFFT input buffers and FFT buffers are allocated. 

Finally we can [set up the radio using the openair0_device_load function](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-ru.c#L1663). This is in the [common_lib.c file](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/ARCH/COMMON/common_lib.c#L117) then sets the device.
