+++
title = "Structs"
date = 2019-03-03T11:00:00-06:00
draft = false
type = "docs"  # Do not modify.

# Show table of contents? true/false
toc = true

# Add menu entry to sidebar.

# Uncomment to customize menu title, e.g. to abbreviate page title.
linktitle = "Structs in OAI"

# Substitute `tutorial` with the name of your tutorials folder.
[menu.oai]
  # Define a parent ID if this page is a parent.
  #name = "YourParentID"
  
  # Reference a parent ID if this page is a child.
  parent = "Other Stuff"
  
  # Order that this page appears in the menu.
  weight = 1
+++

I find myself needing to make some changes in OAI for my own experiments. Its helpful to have a cheatsheet of the various structs.
## RU_t *ru
The ```ru``` struct is [defined here in defs_eNB.h](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair1/PHY/defs_eNB.h#L272)
This is used throughout the project, especially [in lte-ru.c](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-ru.c)

```c
typedef struct RU_t_s{
  /// index of this ru
  uint32_t idx;
 /// Pointer to configuration file
  char *rf_config_file;
  /// southbound interface
  RU_if_south_t if_south;
  /// timing
  node_timing_t if_timing;
  /// function
  node_function_t function;
  /// Ethernet parameters for fronthaul interface
  eth_params_t eth_params;
  /// flag to indicate the RU is in synch with a master reference
  int in_synch;
  /// timing offset
  int rx_offset;        
  /// flag to indicate the RU is a slave to another source
  int is_slave;
  /// Total gain of receive chain
  uint32_t             rx_total_gain_dB;
  /// number of bands that this device can support
  int num_bands;
  /// band list
  int band[MAX_BANDS_PER_RRU];
  /// number of RX paths on device
  int nb_rx;
  /// number of TX paths on device
  int nb_tx;
  /// maximum PDSCH RS EPRE
  int max_pdschReferenceSignalPower;
  /// maximum RX gain
  int max_rxgain;
  /// Attenuation of RX paths on device
  int att_rx;
  /// Attenuation of TX paths on device
  int att_tx;
  /// flag to indicate precoding operation in RU
  int do_precoding;
  /// Frame parameters
  LTE_DL_FRAME_PARMS frame_parms;
  ///timing offset used in TDD
  int              N_TA_offset; 
  /// RF device descriptor
  openair0_device rfdevice;
  /// HW configuration
  openair0_config_t openair0_cfg;
  /// Number of eNBs using this RU
  int num_eNB;
  /// list of eNBs using this RU
  struct PHY_VARS_eNB_s *eNB_list[NUMBER_OF_eNB_MAX];
  /// Mapping of antenna ports to RF chain index
  openair0_rf_map      rf_map;
  /// IF device descriptor
  openair0_device ifdevice;
  /// Pointer for ifdevice buffer struct
  if_buffer_t ifbuffer;
  /// if prach processing is to be performed in RU
  int                  do_prach;
  /// function pointer to synchronous RX fronthaul function (RRU,3GPP_eNB)
  void                 (*fh_south_in)(struct RU_t_s *ru,int *frame, int *subframe);
  /// function pointer to synchronous TX fronthaul function
  void                 (*fh_south_out)(struct RU_t_s *ru);
  /// function pointer to synchronous RX fronthaul function (RRU)
  void                 (*fh_north_in)(struct RU_t_s *ru,int *frame, int *subframe);
  /// function pointer to synchronous RX fronthaul function (RRU)
  void                 (*fh_north_out)(struct RU_t_s *ru);
  /// function pointer to asynchronous fronthaul interface
  void                 (*fh_north_asynch_in)(struct RU_t_s *ru,int *frame, int *subframe);
  /// function pointer to asynchronous fronthaul interface
  void                 (*fh_south_asynch_in)(struct RU_t_s *ru,int *frame, int *subframe);
  /// function pointer to initialization function for radio interface
  int                  (*start_rf)(struct RU_t_s *ru);
  /// function pointer to release function for radio interface
  int                  (*stop_rf)(struct RU_t_s *ru);
  /// function pointer to initialization function for radio interface
  int                  (*start_if)(struct RU_t_s *ru,struct PHY_VARS_eNB_s *eNB);
  /// function pointer to RX front-end processing routine (DFTs/prefix removal or NULL)
  void                 (*feprx)(struct RU_t_s *ru);
  /// function pointer to TX front-end processing routine (IDFTs and prefix removal or NULL)
  void                 (*feptx_ofdm)(struct RU_t_s *ru);
  /// function pointer to TX front-end processing routine (PRECODING)
  void                 (*feptx_prec)(struct RU_t_s *ru);
  /// function pointer to wakeup routine in lte-enb.
  int (*wakeup_rxtx)(struct PHY_VARS_eNB_s *eNB, struct RU_t_s *ru);
  /// function pointer to wakeup routine in lte-enb.
  void (*wakeup_prach_eNB)(struct PHY_VARS_eNB_s *eNB,struct RU_t_s *ru,int frame,int subframe);
#if (LTE_RRC_VERSION >= MAKE_VERSION(14, 0, 0))
  /// function pointer to wakeup routine in lte-enb.
  void (*wakeup_prach_eNB_br)(struct PHY_VARS_eNB_s *eNB,struct RU_t_s *ru,int frame,int subframe);
#endif
  /// function pointer to eNB entry routine
  void (*eNB_top)(struct PHY_VARS_eNB_s *eNB, int frame_rx, int subframe_rx, char *string, struct RU_t_s *ru);
  /// Timing statistics
  time_stats_t ofdm_demod_stats;
  /// Timing statistics (TX)
  time_stats_t ofdm_mod_stats;
  /// Timing wait statistics
  time_stats_t ofdm_demod_wait_stats;
  /// Timing wakeup statistics
  time_stats_t ofdm_demod_wakeup_stats;
  /// Timing wait statistics (TX)
  time_stats_t ofdm_mod_wait_stats;
  /// Timing wakeup statistics (TX)
  time_stats_t ofdm_mod_wakeup_stats;
  /// Timing statistics (RX Fronthaul + Compression)
  time_stats_t rx_fhaul;
  /// Timing statistics (TX Fronthaul + Compression)
  time_stats_t tx_fhaul; 
  /// Timong statistics (Compression)
  time_stats_t compression;
  /// Timing statistics (Fronthaul transport)
  time_stats_t transport;
  /// RX and TX buffers for precoder output
  RU_COMMON            common;
  /// beamforming weight vectors per eNB
  int32_t **beam_weights[NUMBER_OF_eNB_MAX+1][15];

  /// received frequency-domain signal for PRACH (IF4p5 RRU) 
  int16_t              **prach_rxsigF;
  /// received frequency-domain signal for PRACH BR (IF4p5 RRU) 
  int16_t              **prach_rxsigF_br[4];
  /// sequence number for IF5
  uint8_t seqno;
  /// initial timestamp used as an offset make first real timestamp 0
  openair0_timestamp   ts_offset;
  /// process scheduling variables
  RU_proc_t            proc;
  /// stats thread pthread descriptor
  pthread_t            ru_stats_thread;
} RU_t;
```

## LTE_DL_FRAME_PARMS *fp
[This is found on line 583 of openairinterface5g/openair1/PHY/defs_common.h](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair1/PHY/defs_common.h#L583)
```c
typedef struct {
  /// Number of resource blocks (RB) in DL
  uint8_t N_RB_DL;
  /// Number of resource blocks (RB) in UL
  uint8_t N_RB_UL;
  /// EUTRA Band
  uint8_t eutra_band;
  /// DL carrier frequency
  uint32_t dl_CarrierFreq;
  /// UL carrier frequency
  uint32_t ul_CarrierFreq;
  /// TX attenuation
  uint32_t att_tx;
  /// RX attenuation
  uint32_t att_rx;
  ///  total Number of Resource Block Groups: this is ceil(N_PRB/P)
  uint8_t N_RBG;
  /// Total Number of Resource Block Groups SubSets: this is P
  uint8_t N_RBGS;
  /// Cell ID
  uint16_t Nid_cell;
  /// MBSFN Area ID
  uint16_t Nid_cell_mbsfn;
  /// Cyclic Prefix for DL (0=Normal CP, 1=Extended CP)
  lte_prefix_type_t Ncp;
  /// Cyclic Prefix for UL (0=Normal CP, 1=Extended CP)
  lte_prefix_type_t Ncp_UL;
  /// shift of pilot position in one RB
  uint8_t nushift;
  /// Frame type (0 FDD, 1 TDD)
  lte_frame_type_t frame_type;
  /// TDD subframe assignment (0-7) (default = 3) (254=RX only, 255=TX only)
  uint8_t tdd_config;
  /// TDD S-subframe configuration (0-9)
  uint8_t tdd_config_S;
  /// srs extra symbol flag for TDD
  uint8_t srsX;
  /// indicates if node is a UE (NODE=2) or eNB (PRIMARY_CH=0).
  uint8_t node_id;
  /// Indicator that 20 MHz channel uses 3/4 sampling frequency
  uint8_t threequarter_fs;
  /// Size of FFT
  uint16_t ofdm_symbol_size;
  /// Number of prefix samples in all but first symbol of slot
  uint16_t nb_prefix_samples;
  /// Number of prefix samples in first symbol of slot
  uint16_t nb_prefix_samples0;
  /// Carrier offset in FFT buffer for first RE in PRB0
  uint16_t first_carrier_offset;
  /// Number of samples in a subframe
  uint32_t samples_per_tti;
  /// Number of OFDM/SC-FDMA symbols in one subframe (to be modified to account for potential different in UL/DL)
  uint16_t symbols_per_tti;
  /// Number of OFDM symbols in DL portion of S-subframe
  uint16_t dl_symbols_in_S_subframe;
  /// Number of SC-FDMA symbols in UL portion of S-subframe
  uint16_t ul_symbols_in_S_subframe;
  /// Number of Physical transmit antennas in node
  uint8_t nb_antennas_tx;
  /// Number of Receive antennas in node
  uint8_t nb_antennas_rx;
  /// Number of common transmit antenna ports in eNodeB (1 or 2)
  uint8_t nb_antenna_ports_eNB;
  /// PRACH_CONFIG
  PRACH_CONFIG_COMMON prach_config_common;
#if (LTE_RRC_VERSION >= MAKE_VERSION(14, 0, 0))
  /// PRACH_eMTC_CONFIG
  PRACH_eMTC_CONFIG_COMMON prach_emtc_config_common;
#endif
  /// PUCCH Config Common (from 36-331 RRC spec)
  PUCCH_CONFIG_COMMON pucch_config_common;
  /// PDSCH Config Common (from 36-331 RRC spec)
  PDSCH_CONFIG_COMMON pdsch_config_common;
  /// PUSCH Config Common (from 36-331 RRC spec)
  PUSCH_CONFIG_COMMON pusch_config_common;
  /// PHICH Config (from 36-331 RRC spec)
  PHICH_CONFIG_COMMON phich_config_common;
  /// SRS Config (from 36-331 RRC spec)
  SOUNDINGRS_UL_CONFIG_COMMON soundingrs_ul_config_common;
  /// UL Power Control (from 36-331 RRC spec)
  UL_POWER_CONTROL_CONFIG_COMMON ul_power_control_config_common;
  /// Number of MBSFN Configurations
  int num_MBSFN_config;
  /// Array of MBSFN Configurations (max 8 (maxMBSFN-Allocations) elements as per 36.331)
  MBSFN_config_t MBSFN_config[8];
  /// Maximum Number of Retransmissions of RRCConnectionRequest (from 36-331 RRC Spec)
  uint8_t maxHARQ_Msg3Tx;
  /// Size of SI windows used for repetition of one SI message (in frames)
  uint8_t SIwindowsize;
  /// Period of SI windows used for repetition of one SI message (in frames)
  uint16_t SIPeriod;
  /// REGs assigned to PCFICH
  uint16_t pcfich_reg[4];
  /// Index of first REG assigned to PCFICH
  uint8_t pcfich_first_reg_idx;
  /// REGs assigned to PHICH
  uint16_t phich_reg[MAX_NUM_PHICH_GROUPS][3];

  struct MBSFN_SubframeConfig *mbsfn_SubframeConfig[MAX_MBSFN_AREA];
  /// for fair RR scheduler
  uint32_t ue_multiple_max;
} LTE_DL_FRAME_PARMS;
```
## PHY_VARS_eNB *eNB
```c
/// Top-level PHY Data Structure for eNB
typedef struct PHY_VARS_eNB_s {
  /// Module ID indicator for this instance
  module_id_t          Mod_id;
  uint8_t              CC_id;
  uint8_t              configured;
  L1_proc_t            proc;
  int                  single_thread_flag;
  int                  abstraction_flag;
  int                  num_RU;
  RU_t                 *RU_list[MAX_NUM_RU_PER_eNB];
  /// Ethernet parameters for northbound midhaul interface
  eth_params_t         eth_params_n;
  /// Ethernet parameters for fronthaul interface
  eth_params_t         eth_params;
  int                  rx_total_gain_dB;
  int                  (*start_if)(struct RU_t_s *ru,struct PHY_VARS_eNB_s *eNB);
  uint8_t              local_flag;
  LTE_DL_FRAME_PARMS   frame_parms;
  PHY_MEASUREMENTS_eNB measurements;
  IF_Module_t          *if_inst;
  UL_IND_t             UL_INFO;
  pthread_mutex_t      UL_INFO_mutex;
  /// NFAPI RX ULSCH information
  nfapi_rx_indication_pdu_t  rx_pdu_list[NFAPI_RX_IND_MAX_PDU];
  /// NFAPI RX ULSCH CRC information
  nfapi_crc_indication_pdu_t crc_pdu_list[NFAPI_CRC_IND_MAX_PDU];
  /// NFAPI HARQ information
  nfapi_harq_indication_pdu_t harq_pdu_list[NFAPI_HARQ_IND_MAX_PDU];
  /// NFAPI SR information
  nfapi_sr_indication_pdu_t sr_pdu_list[NFAPI_SR_IND_MAX_PDU];
  /// NFAPI CQI information
  nfapi_cqi_indication_pdu_t cqi_pdu_list[NFAPI_CQI_IND_MAX_PDU];
  /// NFAPI CQI information (raw component)
  nfapi_cqi_indication_raw_pdu_t cqi_raw_pdu_list[NFAPI_CQI_IND_MAX_PDU];
  /// NFAPI PRACH information
  nfapi_preamble_pdu_t preamble_list[MAX_NUM_RX_PRACH_PREAMBLES];
#if (LTE_RRC_VERSION >= MAKE_VERSION(14, 0, 0))
  /// NFAPI PRACH information BL/CE UEs
  nfapi_preamble_pdu_t preamble_list_br[MAX_NUM_RX_PRACH_PREAMBLES];
#endif
  Sched_Rsp_t          Sched_INFO;
  LTE_eNB_PDCCH        pdcch_vars[2];
  LTE_eNB_PHICH        phich_vars[2];
#if (LTE_RRC_VERSION >= MAKE_VERSION(14, 0, 0))
  LTE_eNB_EPDCCH       epdcch_vars[2];
  LTE_eNB_MPDCCH       mpdcch_vars[2];
  LTE_eNB_PRACH        prach_vars_br;
#endif
  LTE_eNB_COMMON       common_vars;
  LTE_eNB_UCI          uci_vars[NUMBER_OF_UE_MAX];
  LTE_eNB_SRS          srs_vars[NUMBER_OF_UE_MAX];
  LTE_eNB_PBCH         pbch;
  LTE_eNB_PUSCH       *pusch_vars[NUMBER_OF_UE_MAX];
  LTE_eNB_PRACH        prach_vars;
  LTE_eNB_DLSCH_t     *dlsch[NUMBER_OF_UE_MAX][2];   // Nusers times two spatial streams
  LTE_eNB_ULSCH_t     *ulsch[NUMBER_OF_UE_MAX+1];      // Nusers + number of RA
  LTE_eNB_DLSCH_t     *dlsch_SI,*dlsch_ra,*dlsch_p;
  LTE_eNB_DLSCH_t     *dlsch_MCH;
  LTE_eNB_DLSCH_t     *dlsch_PCH;
  LTE_eNB_UE_stats     UE_stats[NUMBER_OF_UE_MAX];
  LTE_eNB_UE_stats    *UE_stats_ptr[NUMBER_OF_UE_MAX];

  /// cell-specific reference symbols
  uint32_t         lte_gold_table[20][2][14];

  /// UE-specific reference symbols (p=5), TM 7
  uint32_t         lte_gold_uespec_port5_table[NUMBER_OF_UE_MAX][20][38];

  /// UE-specific reference symbols (p=7...14), TM 8/9/10
  uint32_t         lte_gold_uespec_table[2][20][2][21];

  /// mbsfn reference symbols
  uint32_t         lte_gold_mbsfn_table[10][3][42];

  uint32_t X_u[64][839];
#if (LTE_RRC_VERSION >= MAKE_VERSION(14, 0, 0))
  uint32_t X_u_br[4][64][839];
#endif
  uint8_t pbch_configured;
  uint8_t pbch_pdu[4]; //PBCH_PDU_SIZE
  char eNB_generate_rar;

  /// Indicator set to 0 after first SR
  uint8_t first_sr[NUMBER_OF_UE_MAX];

  uint32_t max_peak_val;
  int max_eNB_id, max_sync_pos;

  /// \brief sinr for all subcarriers of the current link (used only for abstraction).
  /// first index: ? [0..N_RB_DL*12[
  double *sinr_dB;

  /// N0 (used for abstraction)
  double N0;

  unsigned char first_run_timing_advance[NUMBER_OF_UE_MAX];
  unsigned char first_run_I0_measurements;

  
  unsigned char    is_secondary_eNB; // primary by default
  unsigned char    is_init_sync;     /// Flag to tell if initial synchronization is performed. This affects how often the secondary eNB will listen to the PSS from the primary system.
  unsigned char    has_valid_precoder; /// Flag to tell if secondary eNB has channel estimates to create NULL-beams from, and this B/F vector is created.
  unsigned char    PeNB_id;          /// id of Primary eNB

  /// hold the precoder for NULL beam to the primary user
  int              **dl_precoder_SeNB[3];
  char             log2_maxp; /// holds the maximum channel/precoder coefficient

  /// if ==0 enables phy only test mode
  int mac_enabled;
  /// counter to average prach energh over first 100 prach opportunities
  int prach_energy_counter;

  // PDSCH Varaibles
  PDSCH_CONFIG_DEDICATED pdsch_config_dedicated[NUMBER_OF_UE_MAX];

  // PUSCH Varaibles
  PUSCH_CONFIG_DEDICATED pusch_config_dedicated[NUMBER_OF_UE_MAX];

  // PUCCH variables
  PUCCH_CONFIG_DEDICATED pucch_config_dedicated[NUMBER_OF_UE_MAX];

  // UL-POWER-Control
  UL_POWER_CONTROL_DEDICATED ul_power_control_dedicated[NUMBER_OF_UE_MAX];

  // TPC
  TPC_PDCCH_CONFIG tpc_pdcch_config_pucch[NUMBER_OF_UE_MAX];
  TPC_PDCCH_CONFIG tpc_pdcch_config_pusch[NUMBER_OF_UE_MAX];

  // CQI reporting
  CQI_REPORT_CONFIG cqi_report_config[NUMBER_OF_UE_MAX];

  // SRS Variables
  SOUNDINGRS_UL_CONFIG_DEDICATED soundingrs_ul_config_dedicated[NUMBER_OF_UE_MAX];
  uint8_t ncs_cell[20][7];

  // Scheduling Request Config
  SCHEDULING_REQUEST_CONFIG scheduling_request_config[NUMBER_OF_UE_MAX];

  // Transmission mode per UE
  uint8_t transmission_mode[NUMBER_OF_UE_MAX];

  /// cba_last successful reception for each group, used for collision detection
  uint8_t cba_last_reception[4];

  // Pointers for active physicalConfigDedicated to be applied in current subframe
  struct LTE_PhysicalConfigDedicated *physicalConfigDedicated[NUMBER_OF_UE_MAX];


  uint32_t rb_mask_ul[4];

  /// Information regarding TM5
  MU_MIMO_mode mu_mimo_mode[NUMBER_OF_UE_MAX];


  /// target_ue_dl_mcs : only for debug purposes
  uint32_t target_ue_dl_mcs;
  /// target_ue_ul_mcs : only for debug purposes
  uint32_t target_ue_ul_mcs;
  /// target_ue_dl_rballoc : only for debug purposes
  uint32_t ue_dl_rb_alloc;
  /// target ul PRBs : only for debug
  uint32_t ue_ul_nb_rb;

  ///check for Total Transmissions
  uint32_t check_for_total_transmissions;

  ///check for MU-MIMO Transmissions
  uint32_t check_for_MUMIMO_transmissions;

  ///check for SU-MIMO Transmissions
  uint32_t check_for_SUMIMO_transmissions;

  ///check for FULL MU-MIMO Transmissions
  uint32_t  FULL_MUMIMO_transmissions;

  /// Counter for total bitrate, bits and throughput in downlink
  uint32_t total_dlsch_bitrate;
  uint32_t total_transmitted_bits;
  uint32_t total_system_throughput;

  int hw_timing_advance;

  time_stats_t phy_proc_tx;
  time_stats_t phy_proc_rx;
  time_stats_t rx_prach;

  time_stats_t ofdm_mod_stats;
  time_stats_t dlsch_common_and_dci;
  time_stats_t dlsch_ue_specific;
  time_stats_t dlsch_encoding_stats;
  time_stats_t dlsch_modulation_stats;
  time_stats_t dlsch_scrambling_stats;
  time_stats_t dlsch_rate_matching_stats;
  time_stats_t dlsch_turbo_encoding_preperation_stats;
  time_stats_t dlsch_turbo_encoding_segmentation_stats;
  time_stats_t dlsch_turbo_encoding_stats;
  time_stats_t dlsch_turbo_encoding_waiting_stats;
  time_stats_t dlsch_turbo_encoding_signal_stats;
  time_stats_t dlsch_turbo_encoding_main_stats;
  time_stats_t dlsch_turbo_encoding_wakeup_stats0;
  time_stats_t dlsch_turbo_encoding_wakeup_stats1;
  time_stats_t dlsch_interleaving_stats;

  time_stats_t rx_dft_stats;
  time_stats_t ulsch_channel_estimation_stats;
  time_stats_t ulsch_freq_offset_estimation_stats;
  time_stats_t ulsch_decoding_stats;
  time_stats_t ulsch_demodulation_stats;
  time_stats_t ulsch_rate_unmatching_stats;
  time_stats_t ulsch_turbo_decoding_stats;
  time_stats_t ulsch_deinterleaving_stats;
  time_stats_t ulsch_demultiplexing_stats;
  time_stats_t ulsch_llr_stats;
  time_stats_t ulsch_tc_init_stats;
  time_stats_t ulsch_tc_alpha_stats;
  time_stats_t ulsch_tc_beta_stats;
  time_stats_t ulsch_tc_gamma_stats;
  time_stats_t ulsch_tc_ext_stats;
  time_stats_t ulsch_tc_intl1_stats;
  time_stats_t ulsch_tc_intl2_stats;

  int32_t pucch1_stats_cnt[NUMBER_OF_UE_MAX][10];
  int32_t pucch1_stats[NUMBER_OF_UE_MAX][10*1024];
  int32_t pucch1_stats_thres[NUMBER_OF_UE_MAX][10*1024];
  int32_t pucch1ab_stats_cnt[NUMBER_OF_UE_MAX][10];
  int32_t pucch1ab_stats[NUMBER_OF_UE_MAX][2*10*1024];
  int32_t pusch_stats_rb[NUMBER_OF_UE_MAX][10240];
  int32_t pusch_stats_round[NUMBER_OF_UE_MAX][10240];
  int32_t pusch_stats_mcs[NUMBER_OF_UE_MAX][10240];
  int32_t pusch_stats_bsr[NUMBER_OF_UE_MAX][10240];
  int32_t pusch_stats_BO[NUMBER_OF_UE_MAX][10240];
} PHY_VARS_eNB;
```
## RAN_CONTEXT_t RC;
This one is a global that is used throughout. It is defined [here](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/common/ran_context.h), but actually called [here in a phy_vars.h](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair1/PHY/phy_vars.h#L47). This phy_vars.h is included in the lte-softmodem.c, and then `extern RAN_CONTEXT_t RC;` is called in many of the files that use this struct such as enb_app.c and the MAC/config.c.

``` c
typedef struct {
  /// RAN context config file name
  char *config_file_name;
  /// Number of RRC instances in this node
  int nb_inst;
  /// Number of Component Carriers per instance in this node
  int *nb_CC;
  /// Number of NB_IoT instances in this node
  int nb_nb_iot_rrc_inst;
  /// Number of MACRLC instances in this node
  int nb_macrlc_inst;
  /// Number of NB_IoT MACRLC instances in this node
  int nb_nb_iot_macrlc_inst;
  /// Number of component carriers per instance in this node
  int *nb_mac_CC;
  /// Number of L1 instances in this node
  int nb_L1_inst;
  /// Number of NB_IoT L1 instances in this node
  int nb_nb_iot_L1_inst;
  /// Number of Component Carriers per instance in this node
  int *nb_L1_CC;
  /// Number of RU instances in this node
  int nb_RU;
  /// FlexRAN context variables
  flexran_agent_info_t **flexran;
  /// eNB context variables
  struct PHY_VARS_eNB_s ***eNB;
  /// NB_IoT L1 context variables
  struct PHY_VARS_eNB_NB_IoT_s **L1_NB_IoT;
  /// RRC context variables
  struct eNB_RRC_INST_s **rrc;
  /// NB_IoT RRC context variables
  //struct eNB_RRC_INST_NB_IoT_s **nb_iot_rrc;
  /// MAC context variables
  struct eNB_MAC_INST_s **mac;
  /// NB_IoT MAC context variables
  struct eNB_MAC_INST_NB_IoT_s **nb_iot_mac;
  /// GTPu descriptor 
  gtpv1u_data_t *gtpv1u_data_g;
  /// RU descriptors. These describe what each radio unit is supposed to do and contain the necessary functions for fronthaul interfaces
  struct RU_t_s **ru;
  /// Mask to indicate fronthaul setup status of RU (hard-limit to 64 RUs)
  uint64_t ru_mask;
  /// Mutex for protecting ru_mask
  pthread_mutex_t ru_mutex;
  /// condition variable for signaling setup completion of an RU
  pthread_cond_t ru_cond;
} RAN_CONTEXT_t;
```

## openair0_device device
This contains the settings for the actual radio unit device such as a USRP. It is defined in /targets/ARCH/COMMON/common_lib.h [here](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/ARCH/COMMON/common_lib.h#L266).
```C
struct openair0_device_t {
  /*!brief Module ID of this device */
  int Mod_id;

  /*!brief Component Carrier ID of this device */
  int CC_id;
  
  /*!brief Type of this device */
  dev_type_t type;

  /*!brief Transport protocol type that the device suppports (in case I/Q samples need to be transported) */
  transport_type_t transp_type;

   /*!brief Type of the device's host (RAU/RRU) */
  host_type_t host_type;

  /* !brief RF frontend parameters set by application */
  openair0_config_t *openair0_cfg;

  /* !brief ETH params set by application */
  eth_params_t *eth_params;

  /*!brief Can be used by driver to hold internal structure*/
  void *priv;

  /* Functions API, which are called by the application*/

  /*! \brief Called to start the transceiver. Return 0 if OK, < 0 if error
      @param device pointer to the device structure specific to the RF hardware target
  */
  int (*trx_start_func)(openair0_device *device);

  /*! \brief Called to send a request message between RAU-RRU on control port
      @param device pointer to the device structure specific to the RF hardware target
      @param msg pointer to the message structure passed between RAU-RRU
      @param msg_len length of the message  
  */  
  int (*trx_ctlsend_func)(openair0_device *device, void *msg, ssize_t msg_len);

  /*! \brief Called to receive a reply  message between RAU-RRU on control port
      @param device pointer to the device structure specific to the RF hardware target
      @param msg pointer to the message structure passed between RAU-RRU
      @param msg_len length of the message  
  */  
  int (*trx_ctlrecv_func)(openair0_device *device, void *msg, ssize_t msg_len);

  /*! \brief Called to send samples to the RF target
      @param device pointer to the device structure specific to the RF hardware target
      @param timestamp The timestamp at whicch the first sample MUST be sent 
      @param buff Buffer which holds the samples
      @param nsamps number of samples to be sent
      @param antenna_id index of the antenna if the device has multiple anteannas
      @param flags flags must be set to TRUE if timestamp parameter needs to be applied
  */   
  int (*trx_write_func)(openair0_device *device, openair0_timestamp timestamp, void **buff, int nsamps,int antenna_id, int flags);

  /*! \brief Receive samples from hardware.
   * Read \ref nsamps samples from each channel to buffers. buff[0] is the array for
   * the first channel. *ptimestamp is the time at which the first sample
   * was received.
   * \param device the hardware to use
   * \param[out] ptimestamp the time at which the first sample was received.
   * \param[out] buff An array of pointers to buffers for received samples. The buffers must be large enough to hold the number of samples \ref nsamps.
   * \param nsamps Number of samples. One sample is 2 byte I + 2 byte Q => 4 byte.
   * \param antenna_id Index of antenna for which to receive samples
   * \returns the number of sample read
   */
  int (*trx_read_func)(openair0_device *device, openair0_timestamp *ptimestamp, void **buff, int nsamps,int antenna_id);

  /*! \brief print the device statistics  
   * \param device the hardware to use
   * \returns  0 on success
   */
  int (*trx_get_stats_func)(openair0_device *device);

  /*! \brief Reset device statistics  
   * \param device the hardware to use
   * \returns 0 in success 
   */
  int (*trx_reset_stats_func)(openair0_device *device);

  /*! \brief Terminate operation of the transceiver -- free all associated resources 
   * \param device the hardware to use
   */
  void (*trx_end_func)(openair0_device *device);

  /*! \brief Stop operation of the transceiver 
   */
  int (*trx_stop_func)(openair0_device *device);

  /* Functions API related to UE*/

  /*! \brief Set RX feaquencies 
   * \param device the hardware to use
   * \param openair0_cfg RF frontend parameters set by application
   * \param exmimo_dump_config  dump EXMIMO configuration 
   * \returns 0 in success 
   */
  int (*trx_set_freq_func)(openair0_device* device, openair0_config_t *openair0_cfg,int exmimo_dump_config);
  
  /*! \brief Set gains
   * \param device the hardware to use
   * \param openair0_cfg RF frontend parameters set by application
   * \returns 0 in success 
   */
  int (*trx_set_gains_func)(openair0_device* device, openair0_config_t *openair0_cfg);

  /*! \brief RRU Configuration callback
   * \param idx RU index
   * \param arg pointer to capabilities or configuration
   */
  void (*configure_rru)(int idx, void* arg);
};
```

## RU_proc_t proc
The `proc` is used in places like the [rx_rf function in lte-ru.c.](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/targets/RT/USER/lte-ru.c#L732)

The struct below is defined in the code [here](https://gitlab.eurecom.fr/oai/openairinterface5g/blob/master/openair1/PHY/defs_eNB.h#L75).

```
typedef struct RU_proc_t_s {
  /// Pointer to associated RU descriptor
  struct RU_t_s *ru;
  /// timestamp received from HW
  openair0_timestamp timestamp_rx;
  /// timestamp to send to "slave rru"
  openair0_timestamp timestamp_tx;
  /// subframe to act upon for reception
  int subframe_rx;
  /// subframe to act upon for transmission
  int subframe_tx;
  /// subframe to act upon for reception of prach
  int subframe_prach;
#if (LTE_RRC_VERSION >= MAKE_VERSION(14, 0, 0))
  /// subframe to act upon for reception of prach BL/CE UEs
  int subframe_prach_br;
#endif
  /// frame to act upon for reception
  int frame_rx;
  /// frame to act upon for transmission
  int frame_tx;
  /// unwrapped frame count
  int frame_tx_unwrap;
  /// frame to act upon for reception of prach
  int frame_prach;
#if (LTE_RRC_VERSION >= MAKE_VERSION(14, 0, 0))
  /// frame to act upon for reception of prach
  int frame_prach_br;
#endif
  /// frame offset for slave RUs (to correct for frame asynchronism at startup)
  int frame_offset;
  /// \brief Instance count for FH processing thread.
  /// \internal This variable is protected by \ref mutex_FH.
  int instance_cnt_FH;
  int instance_cnt_FH1;
  /// \internal This variable is protected by \ref mutex_prach.
  int instance_cnt_prach;
#if (LTE_RRC_VERSION >= MAKE_VERSION(14, 0, 0))
  /// \internal This variable is protected by \ref mutex_prach.
  int instance_cnt_prach_br;
#endif
  /// \internal This variable is protected by \ref mutex_synch.
  int instance_cnt_synch;
  /// \internal This variable is protected by \ref mutex_eNBs.
  int instance_cnt_eNBs;
  /// \brief Instance count for rx processing thread.
  /// \internal This variable is protected by \ref mutex_asynch_rxtx.
  int instance_cnt_asynch_rxtx;
  /// \internal This variable is protected by \ref mutex_fep
  int instance_cnt_fep;
  /// \internal This variable is protected by \ref mutex_feptx
  int instance_cnt_feptx;
  /// This varible is protected by \ref mutex_emulatedRF
  int instance_cnt_emulateRF;
  /// pthread structure for RU FH processing thread
  pthread_t pthread_FH;
  pthread_t pthread_FH1;
  /// pthread structure for RU prach processing thread
  pthread_t pthread_prach;
#if (LTE_RRC_VERSION >= MAKE_VERSION(14, 0, 0))
  /// pthread structure for RU prach processing thread BL/CE UEs
  pthread_t pthread_prach_br;
#endif
  /// pthread struct for RU synch thread
  pthread_t pthread_synch;
  /// pthread struct for RU RX FEP worker thread
  pthread_t pthread_fep;
  /// pthread struct for RU TX FEP worker thread
  pthread_t pthread_feptx;
  /// pthread struct for emulated RF
  pthread_t pthread_emulateRF;
  /// pthread structure for asychronous RX/TX processing thread
  pthread_t pthread_asynch_rxtx;
  /// flag to indicate first RX acquisition
  int first_rx;
  /// flag to indicate first TX transmission
  int first_tx;
  /// pthread attributes for RU FH processing thread
  pthread_attr_t attr_FH;
  pthread_attr_t attr_FH1;
  /// pthread attributes for RU prach
  pthread_attr_t attr_prach;
#if (LTE_RRC_VERSION >= MAKE_VERSION(14, 0, 0))
  /// pthread attributes for RU prach BL/CE UEs
  pthread_attr_t attr_prach_br;
#endif
  /// pthread attributes for RU synch thread
  pthread_attr_t attr_synch;
  /// pthread attributes for asynchronous RX thread
  pthread_attr_t attr_asynch_rxtx;
  /// pthread attributes for worker fep thread
  pthread_attr_t attr_fep;
  /// pthread attributes for worker feptx thread
  pthread_attr_t attr_feptx;
  /// pthread attributes for emulated RF
  pthread_attr_t attr_emulateRF;
  /// scheduling parameters for RU FH thread
  struct sched_param sched_param_FH;
  struct sched_param sched_param_FH1;
  /// scheduling parameters for RU prach thread
  struct sched_param sched_param_prach;
#if (LTE_RRC_VERSION >= MAKE_VERSION(14, 0, 0))
  /// scheduling parameters for RU prach thread BL/CE UEs
  struct sched_param sched_param_prach_br;
#endif
  /// scheduling parameters for RU synch thread
  struct sched_param sched_param_synch;
  /// scheduling parameters for asynch_rxtx thread
  struct sched_param sched_param_asynch_rxtx;
  /// condition variable for RU FH thread
  pthread_cond_t cond_FH;
  pthread_cond_t cond_FH1;
  /// condition variable for RU prach thread
  pthread_cond_t cond_prach;
#if (LTE_RRC_VERSION >= MAKE_VERSION(14, 0, 0))
  /// condition variable for RU prach thread BL/CE UEs
  pthread_cond_t cond_prach_br;
#endif
  /// condition variable for RU synch thread
  pthread_cond_t cond_synch;
  /// condition variable for asynch RX/TX thread
  pthread_cond_t cond_asynch_rxtx;
  /// condition variable for RU RX FEP thread
  pthread_cond_t cond_fep;
  /// condition variable for RU TX FEP thread
  pthread_cond_t cond_feptx;
  /// condition variable for emulated RF
  pthread_cond_t cond_emulateRF;
  /// condition variable for eNB signal
  pthread_cond_t cond_eNBs;
  /// mutex for RU FH
  pthread_mutex_t mutex_FH;
  pthread_mutex_t mutex_FH1;
  /// mutex for RU prach
  pthread_mutex_t mutex_prach;
#if (LTE_RRC_VERSION >= MAKE_VERSION(14, 0, 0))
  /// mutex for RU prach BL/CE UEs
  pthread_mutex_t mutex_prach_br;
#endif
  /// mutex for RU synch
  pthread_mutex_t mutex_synch;
  /// mutex for eNB signal
  pthread_mutex_t mutex_eNBs;
  /// mutex for asynch RX/TX thread
  pthread_mutex_t mutex_asynch_rxtx;
  /// mutex for fep RX worker thread
  pthread_mutex_t mutex_fep;
  /// mutex for fep TX worker thread
  pthread_mutex_t mutex_feptx;
  /// mutex for emulated RF thread
  pthread_mutex_t mutex_emulateRF;
  /// symbol mask for IF4p5 reception per subframe
  uint32_t symbol_mask[10];
  /// number of slave threads
  int                  num_slaves;
  /// array of pointers to slaves
  struct RU_proc_t_s           **slave_proc;
#ifdef PHY_TX_THREAD
  /// pthread structure for PRACH thread
  pthread_t pthread_phy_tx;
  pthread_mutex_t mutex_phy_tx;
  pthread_cond_t cond_phy_tx;
  /// \internal This variable is protected by \ref mutex_phy_tx.
  int instance_cnt_phy_tx;
  /// frame to act upon for transmission
  int frame_phy_tx;
  /// subframe to act upon for transmission
  int subframe_phy_tx;
  /// timestamp to send to "slave rru"
  openair0_timestamp timestamp_phy_tx;
  /// pthread structure for RF TX thread
  pthread_t pthread_rf_tx;
  pthread_mutex_t mutex_rf_tx;
  pthread_cond_t cond_rf_tx;
  /// \internal This variable is protected by \ref mutex_rf_tx.
  int instance_cnt_rf_tx;
#endif
#if defined(PRE_SCD_THREAD)
  pthread_t pthread_pre_scd;
  /// condition variable for time processing thread
  pthread_cond_t cond_pre_scd;
  /// mutex for time thread
  pthread_mutex_t mutex_pre_scd;
  int instance_pre_scd;
#endif
  int emulate_rf_busy;
} RU_proc_t;
```
