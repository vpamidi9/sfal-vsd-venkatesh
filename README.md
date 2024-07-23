 
                              IC Compiler II (TM)

             Version T-2022.03-SP5 for linux64 - Nov 01, 2022 -SLE

                    Copyright (c) 1988 - 2022 Synopsys, Inc.
   This software and the associated documentation are proprietary to Synopsys,
 Inc. This software may only be used in accordance with the terms and conditions
 of a written license agreement with Synopsys, Inc. All other use, reproduction,
   or distribution of this software is strictly prohibited.  Licensed Products
     communicate with Synopsys servers for the purpose of providing software
    updates, detecting software piracy and verifying that customers are using
    Licensed Products in conformity with the applicable License Key for such
  Licensed Products. Synopsys will use information gathered in connection with
    this process to deliver software updates and pursue software pirates and
                                   infringers.

 Inclusivity & Diversity - Visit SolvNetPlus to read the "Synopsys Statement on
            Inclusivity and Diversity" (Refer to article 000036315 at
                        https://solvnetplus.synopsys.com)

icc2_shell> source top.tcl
puts "RM-info : Running script [info script]\n"
RM-info : Running script /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/icc2_common_setup.tcl

##########################################################################################
# Tool: IC Compiler II
# Script: icc2_common_setup.tcl
# Version: P-2019.03-SP4
# Copyright (C) 2014-2019 Synopsys, Inc. All rights reserved.
##########################################################################################
##########################################################################################
## Required variables
## These variables must be correctly filled in for the flow to run properly
##########################################################################################
set DESIGN_NAME                 "vsdbabysoc" ;# Required; name of the design to be worked on; also used as the block name when scripts save or copy a block
set LIBRARY_SUFFIX              "sky130_fd_sc_hd" ;# Suffix for the design library name ; default is unspecified   
set DESIGN_LIBRARY              "${DESIGN_NAME}${LIBRARY_SUFFIX}" ;# Name of the design library; default is ${DESIGN_NAME}${LIBRARY_SUFFIX}
set REFERENCE_LIBRARY           [list /home/venkatesh/VSDBabySoC/synopsys_ICC2flow_130nm/synopsys_skywater_flow_nominal/LEF/sky130_v5_7magic.lef /home/venkatesh/VSDBabySoC/src/lef/avsddac.lef /home/venkatesh/VSDBabySoC/src/lef/avsdpll.lef] ;# Required; a list of reference libraries for the design library.
;#      for library configuration flow (LIBRARY_CONFIGURATION_FLOW set to true below): 
;#              - specify the list of physical source files to be used for library configuration during create_lib
;#      for hierarchical designs using bottom-up flows: include subblock design libraries in the list;
;#      for hierarchical designs using ETMs: include the ETM library in the list.
;#              - If unpack_rm_dirs.pl is used to create dir structures for hierarchical designs, 
;#                in order to transition between hierarchical DP and hierarchical PNR flows properly, 
;#                absolute paths are a requirement.
set COMPRESS_LIBS               "false" ;# Save libs as compressed NDM; only used in DP.
#set VERILOG_NETLIST_FILES      "/home/kunal/workshop/icc2_workshop_collaterals/pnrScripts/spi_slave.synth.v"
set VERILOG_NETLIST_FILES       "/home/venkatesh/VSDBabySoC/output/vsdbabysoc_net.v"    ;# Verilog netlist files;
;#      for DP: required
;#      for PNR: required if INIT_DESIGN_INPUT is ASCII in icc2_pnr_setup.tcl; not required for DC_ASCII or DP_RM_NDM
set UPF_FILE                    ""      ;# A UPF file
;#      for DP: required
;#      for PNR: required if INIT_DESIGN_INPUT is ASCII in icc2_pnr_setup.tcl; not required for DC_ASCII or DP_RM_NDM
;#          for hierarchical designs using ETMs, load the block upf file
;#          for each sub-block linked to ETM, include the following line in the UPF_FILE 
;#              load_upf block.upf -scope block_instance_name
set UPF_SUPPLEMENTAL_FILE       ""      ;# The supplemental UPF file. Only needed if you are running golden UPF flow, in which case, you need both UPF_FILE and this.
;#      for DP: required
;#      for PNR: required if INIT_DESIGN_INPUT is ASCII in icc2_pnr_setup.tcl; not required for DC_ASCII or DP_RM_NDM
;#          If UPF_SUPPLEMENTAL_FILE is specified, scripts assume golden UPF flow. load_upf and save_upf commands will be different.    
set TCL_PARASITIC_SETUP_FILE    "/home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/init_design.read_parasitic_tech_example.tcl"       ;# Specify a Tcl script to read in your TLU+ files by using the read_parasitic_tech command;
;# refer to the example in templates/init_design.read_parasitic_tech_example.tcl 
#set TCL_MCMM_SETUP_FILE         ""
set TCL_MCMM_SETUP_FILE         "/home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/init_design.mcmm_example.auto_expanded.tcl"        ;# Specify a Tcl script to create your corners, modes, scenarios and load respective constraints;
;# two examples are provided in templates/: 
;# init_design.mcmm_example.explicit.tcl: provide mode, corner, and scenario constraints; create modes, corners, 
;# and scenarios; source mode, corner, and scenario constraints, respectively 
;# init_design.mcmm_example.auto_expanded.tcl: provide constraints for the scenarios; create modes, corners, 
;# and scenarios; source scenario constraints which are then expanded to associated modes and corners
;#      for DP: required
;#      for PNR: required if INIT_DESIGN_INPUT is ASCII in icc2_pnr_setup.tcl; not required for DC_ASCII or DP_RM_NDM
set TECH_FILE                   "/home/venkatesh/VSDBabySoC/synopsys_ICC2flow_130nm/synopsys_skywater_flow_nominal/milkywayORtechFiles/sky130_fd_sc_hd.tf"      ;# A technology file; TECH_FILE and TECH_LIB are mutually exclusive ways to specify technology information; 
;# TECH_FILE is recommended, although TECH_LIB is also supported in ICC2 RM. 
set TECH_LIB                    ""      ;# Specify the reference library to be used as a dedicated technology library;
;# as a best practice, please list it as the first library in the REFERENCE_LIBRARY list 
set TECH_LIB_INCLUDES_TECH_SETUP_INFO true 
;# Indicate whether TECH_LIB contains technology setup information such as routing layer direction, offset, 
;# site default, and site symmetry, etc. TECH_LIB may contain this information if loaded during library prep.
;# true|false; this variable is associated with TECH_LIB. 
set TCL_TECH_SETUP_FILE         "/home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/init_design.tech_setup.tcl"
;# Specify a TCL script for setting routing layer direction, offset, site default, and site symmetry list, etc.
;# init_design.tech_setup.tcl is the default. Use it as a template or provide your own script.
;# This script will only get sourced if the following conditions are met: 
;# (1) TECH_FILE is specified (2) TECH_LIB is specified && TECH_LIB_INCLUDES_TECH_SETUP_INFO is false 
set ROUTING_LAYER_DIRECTION_OFFSET_LIST "{met1 horizontal} {met2 vertical} {met3 horizontal} {met4 vertical} {met5 horizontal}" 
;# Specify the routing layers as well as their direction and offset in a list of space delimited pairs;
;# This variable should be defined for all metal routing layers in technology file;
;# Syntax is "{metal_layer_1 direction offset} {metal_layer_2 direction offset} ...";
;# It is required to at least specify metal layers and directions. Offsets are optional. 
;# Example1 is with offsets specified: "{M1 vertical 0.2} {M2 horizontal 0.0} {M3 vertical 0.2}"
;# Example2 is without offsets specified: "{M1 vertical} {M2 horizontal} {M3 vertical}"
##########################################################################################
## Optional variables
## Specify these variables if the corresponding functions are desired 
##########################################################################################
set DESIGN_LIBRARY_SCALE_FACTOR ""      ;# Specify the length precision for the library. Length precision for the design
;# library and its ref libraries must be identical. Tool default is 10000, which
;# implies one unit is one Angstrom or 0.1nm.
set UPF_UPDATE_SUPPLY_SET_FILE  ""      ;# A UPF file to resolve UPF supply sets
#set DEF_FLOORPLAN_FILES                "/home/kunal/design/scripts/pnr/ICC2-RM_P-2019.03-SP4/write_data_dir/picorv32/picorv32.icc.floorplan/floorplan.def.gz"  ;# DEF files which contain the floorplan information;
set DEF_FLOORPLAN_FILES                ""  ;# DEF files which contain the floorplan information;
;#      for DP: not required
;#      for PNR: required if INIT_DESIGN_INPUT = ASCII in icc2_pnr_setup.tcl and neither TCL_FLOORPLAN_FILE or 
;#               initialize_floorplan is used; DEF_FLOORPLAN_FILES and TCL_FLOORPLAN_FILE are mutually exclusive;
;#               not required if INIT_DESIGN_INPUT = DC_ASCII or DP_RM_NDM
set DEF_SCAN_FILE               ""      ;# A scan DEF file for scan chain information;
;#      for PNR: not required if INIT_DESIGN_INPUT = DC_ASCII or DP_RM_NDM, as SCANDEF is expected to be loaded already
set DEF_SITE_NAME_PAIRS         {}      ;# A list of site name pairs for read_def -convert; 
;# specify site name pairs with from_site first followed by to_site;
;# Example: set DEF_SITE_NAME_PAIRS {{from_site_1 to_site_1} {from_site_2 to_site_2}}   
set SITE_DEFAULT                ""      ;# Specify the default site name if there are multiple site defs in the technology file;
;# this is to be used by initialize_floorplan command; example: set SITE_DEFAULT "unit";
;# this is applied in the init_design.tech_setup.tcl script 
set SITE_SYMMETRY_LIST  ""              ;# Specify a list of site def and its symmetry value;
;# this is to be used by read_def or initialize_floorplan command to control the site symmetry;
;# example: set SITE_SYMMETRY_LIST "{unit Y} {unit1 Y}"; this is applied in the init_design.tech_setup.tcl script 
set MIN_ROUTING_LAYER           "met1"  ;# Min routing layer name; normally should be specified; otherwise tool can use all metal layers
set MAX_ROUTING_LAYER           "met5"  ;# Max routing layer name; normally should be specified; otherwise tool can use all metal layers
set LIBRARY_CONFIGURATION_FLOW  false   ;# Set it to true enables library configuration flow which calls the library manager under the hood to generate .nlibs, 
;# save them to disk, and automatically link them to the design.
;# Requires LINK_LIBRARY to be specified with .db files and REFERENCE_LIBRARY to be specified with physical
;# source files for the library configuration flow. Also search_path (in icc2_pnr_setup.tcl) should include paths 
;# to these .db and physical source files.
set LINK_LIBRARY                [list  /home/venkatesh/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.db /home/venkatesh/VSDBabySoC/src/lib/avsddac.db /home/venkatesh/VSDBabySoC/src/lib/avsdpll.db] ;# Specify .db files;
;#      for running VC-LP (vc_lp.tcl) and Formality (fm.tcl): required
;#      for ICC-II without LIBRARY_CONFIGURATION_FLOW enabled: not required
;#      for ICC-II with LIBRARY_CONFIGURATION_FLOW enabled: required; 
;#              - the .db files specified will be used for the library configuration under the hood during create_lib 
##########################################################################################
## Variables related to flow controls of flat PNR, hierarchical PNR and transition with DP
##########################################################################################
set DESIGN_STYLE                "hier"  ;# Specify the design style; flat|hier; default is flat; 
;# specify flat for a totally flat flow (flat PNR for short) and 
;# specify hier for a hierarchical flow (hier PNR for short);
;#      for hier PNR: required and auto set if unpack_rm_dirs.pl is used; (see README.unpack_rm_dirs.txt for details)
;#      for flat PNR: this should set to flat (default)
;#      for DP: not used 
set PHYSICAL_HIERARCHY_LEVEL    ""      ;# Specify the current level of hierarchy for the hierarchical PNR flow; top|intermediate|bottom;
;#      for hier PNR: required and auto set if unpack_rm_dirs.pl is used; (see README.unpack_rm_dirs.txt for details)
;#      for flat PNR and for DP: not used.
set RELEASE_DIR_DP              "write_data_dir_hier"   ;# Specify the release directory of DP RM; 
;# this is where init_design.tcl of PNR flow gets DP RM released libraries;
;#      for hier PNR: required and auto set if unpack_rm_dirs.pl is used; (see README.unpack_rm_dirs.txt for details)
;#      for flat PNR: required if INIT_DESIGN_INPUT = DP_RM_NDM, as init_design.tcl needs to know where DP RM libraries are
;#      for DP: not used 
set RELEASE_LABEL_NAME_DP       "vsdbabysoc_label"      
;# Specify the label name of the block in the DP RM released library;
;# this is the label name which init_design.tcl of PNR flow will open. 
set RELEASE_DIR_PNR             ""      ;# Specify the release directory of PNR RM; 
;# this is where the init_design.tcl of hierarchical PNR flow gets the sub-block libraries;     
;#      for hier PNR: required and auto set if unpack_rm_dirs.pl is used; (see README.unpack_rm_dirs.txt for details)
;#      for flat PNR and for DP: not used.
##########################################################################################
## Variables related to REDHAWK ANALYSIS FUSION
##########################################################################################
set REDHAWK_SEARCH_PATH         ""      ;# Required. Search path to the NDM, reference libraries, and etc.
puts "RM-info : Completed script [info script]\n"
RM-info : Completed script /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/icc2_common_setup.tcl

puts "RM-info : Running script [info script]\n"
RM-info : Running script /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/icc2_dp_setup.tcl

##########################################################################################
# Tool: IC Compiler II 
# Script: icc2_dp_setup.tcl 
# Version: P-2019.03-SP4
# Copyright (C) 2014-2019 Synopsys, Inc. All rights reserved.
##########################################################################################
##########################################################################################
#                               Flow Setup
##########################################################################################
set DP_FLOW         "flat"    ;# hier or flat
set FLOORPLAN_STYLE "channel" ;# Supported design styles channel, abutted
set CHECK_DESIGN    "true"    ;# run atomic check_design pre checks prior to DP commands
set DISTRIBUTED 0  ;# Use distributed runs
### It is required to include the set_host_options command to enable distributed mode tasks. For example,
set_host_options -max_cores 8
#set_host_options -name block_script -submit_command [list qsub -P bnormal -l mem_free=6G,qsc=o -cwd]
set BLOCK_DIST_JOB_FILE             ""     ;# File to set block specific resource requests for distributed jobs
# For example:
#   set_host_options -name block_script  -submit_command "bsub -q normal"
#   set_host_options -name large_block   -submit_command "bsub -q huge"
#   set_host_options -name special_block -submit_command "rsh" local_machine
#   set_app_options -block [get_block block4] -list {plan.distributed_run.block_host large_block}
#   set_app_options -block [get_block block5] -list {plan.distributed_run.block_host large_block}
#   set_app_options -block [get_block block2] -list {plan.distributed_run.block_host special_block}
#  
#   All the jobs associated with blocks that do not have the plan.distributed_run.block_host app option specified
#   will run using the block_script host option. The jobs for blocks block4 and block5 will use the large_block 
#   host option. The job form  block2  will  use  the  special_block host option.
##########################################################################################
# If the design is run with MIBs then change the block list appropriately
##########################################################################################
set DP_BLOCK_REFS                     [list] ;# design names for each physical block (including black boxes) in the design;
;# this includes bottom and mid level blocks in a Multiple Physical Hierarchy (MPH) design
set DP_INTERMEDIATE_LEVEL_BLOCK_REFS  [list data_memory] ;# design reference names for mid level blocks only
set DP_BB_BLOCK_REFS                  [list] ;# Black Box reference names 
set BOTTOM_BLOCK_VIEW             "abstract" ;# Support abstract or design view for bottom blocks
;# in the hier flow
set INTERMEDIATE_BLOCK_VIEW       "abstract" ;# Support abstract or design view for intermediate blocks
if { [info exists INTERMEDIATE_BLOCK_VIEW] && $INTERMEDIATE_BLOCK_VIEW == "abstract" } {
   set_app_options -name abstract.allow_all_level_abstract -value true  ;# Dafult value is false
}
Warning: Application option 'abstract.allow_all_level_abstract' will be made block-scoped in the upcoming 2019.12 release. (ABS-549)
# Provide blackbox instanace: target area, BB UPF file, BB Timing file, boundary
#set DP_BB_BLOCK_REFS "leon3s_bb"
#set DP_BB_BLOCKS(leon3s_bb,area)        [list 1346051] ;
#set DP_BB_BLOCKS(leon3s_bb,upf)         [list ${des_dir}/leon3s_bb.upf] ;
#set DP_BB_BLOCKS(leon3s_bb,timing)      [list ${des_dir}/leon3s_bbt.tcl] ;
#set DP_BB_BLOCKS(leon3s_bb,boundary)    { {x1 y1} {x2 y1} {x2 y2} {x1 y2} {x1 y1} } ;
#set DP_BB_SPLIT    "true"
##########################################################################################
#                               CONSTRAINTS / UPF INTENT
##########################################################################################
set TCL_TIMING_RULER_SETUP_FILE       "" ;# file sourced to define parasitic constraints for use with timing ruler 
# before full extraction environment is defined
# Example setup file:
#       set_parasitic_parameters \
                                          #         -early_spec para_WORST \
                                          #         -late_spec para_WORST
set CONSTRAINT_MAPPING_FILE           "" ;# Constraint Mapping File. Default is "split/mapfile"
set TCL_UPF_FILE                      "" ;# Optional power intent TCL script
##########################################################################################
#                               TOP LEVEL FLOORPLAN CREATION (die, pad, RDL) / PLACE IO
##########################################################################################
set TCL_PHYSICAL_CONSTRAINTS_FILE     "" ;# TCL script for primary die area creation. If specified, DEF_FLOORPLAN_FILES will be loaded after TCL_PHYSICAL_CONSTRAINTS_FILE
set TCL_PRE_COMMIT_FILE               "" ;# file sourced to set attributes, lib cell purposes, .. etc on specific cells, prior to running commit_block
set TCL_USER_INIT_DP_POST_SCRIPT      "" ;# An optional Tcl file to be sourced at the very end of init_dp.tcl before save_block.
##########################################################################################
#                               PRE_SHAPING
##########################################################################################
set TCL_USER_PRE_SHAPING_PRE_SCRIPT   "" ;# An optional Tcl file to be sourced at the very beginning of the task
set TCL_USER_PRE_SHAPING_POST_SCRIPT  "" ;# An optional Tcl file to be sourced at the very end of the task
##########################################################################################
#                               PLACE_IO
##########################################################################################
set TCL_PAD_CONSTRAINTS_FILE          "" ;# file sourced to create everything needed by place_io to complete IO placement
;# including flip chip bumps, and io constraints
set TCL_RDL_FILE                      "" ;# file sourced to create RDL routes
set TCL_USER_PLACE_IO_PRE_SCRIPT      "" ;# An optional Tcl file to be sourced at the very beginning of the task
set TCL_USER_PLACE_IO_POST_SCRIPT     "" ;# An optional Tcl file to be sourced at the very end of the task
##########################################################################################
#                               SHAPING
##########################################################################################
switch $FLOORPLAN_STYLE {
   channel {set SHAPING_CMD_OPTIONS           "-channels true"} 
   abutted {set SHAPING_CMD_OPTIONS           "-channels false"} 
}
set TCL_SHAPING_CONSTRAINTS_FILE      "" ;# Specify any constraints prior to shaping i.e. set_shaping_options
# or specify some block shapes manually, for example:
#    set_block_boundary -cell block1 -boundary {{2.10 2.16} {2.10 273.60} \
                                          #    {262.02 273.60} {262.02 2.16}} -orientation R0
#    set_fixed_objects [get_cells block1]
# Support TCL based shaping constraints
# An example is in rm_icc2_dp_scripts/tcl_shaping_constraints_example.tcl
set SHAPING_CONSTRAINTS_FILE          "" ;# Will be included as the -constraint_file option for shape_blocks
set TCL_SHAPING_PNS_STRATEGY_FILE     "" ;# file sourced to create PG strategies for block grid creation
set TCL_MANUAL_SHAPING_FILE           "" ;# File sourced to re-create all block shapes.
# If this file exists, automatic shaping will be by-passed.
# Existing auto or manual block shapes can be written out using the following:
#    write_floorplan -objects <BLOCK_INSTS>
set TCL_USER_SHAPING_PRE_SCRIPT       "" ;# An optional Tcl file to be sourced at the very beginning of the task
set TCL_USER_SHAPING_POST_SCRIPT      "" ;# An optional Tcl file to be sourced at the very end of the task
##########################################################################################
#                               PLACEMENT
##########################################################################################
set TCL_PLACEMENT_CONSTRAINTS_FILE    "" ;# Placeholder for any macro or standard cell placement constraints & options.
# File is sourced prior to DP placement
set PLACEMENT_PIN_CONSTRAINT_AWARE    "false" ;# tells create_placement to consider pin constraints during placement
set USE_INCREMENTAL_DATA              "0" ;# Use floorplan constraints that were written out on a previous run
set CONGESTION_DRIVEN_PLACEMENT       "" ;# Set to one of the following: std_cell, macro, or both to enable congestion driven placement
set TIMING_DRIVEN_PLACEMENT           "" ;# Set to one of the following: std_cell, macro, or both to enable timing driven placement
set TCL_USER_PLACEMENT_PRE_SCRIPT     "" ;# An optional Tcl file to be sourced at the very beginning of the task
set TCL_USER_PLACEMENT_POST_SCRIPT    "" ;# An optional Tcl file to be sourced at the very end of the task
##########################################################################################
#                               GLOBAL PLANNING
##########################################################################################
set TCL_GLOBAL_PLANNING_FILE          "" ;#Global planning for bus/critical nets
##########################################################################################
#                               PNS
##########################################################################################
set TCL_PNS_FILE                      "/home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/pns_example.tcl" ;# File sourced to define all power structures. 
# This file will include the following types of PG commands:
#   PG Regions
#   PG Patterns
#   PG Strategies
# Note: The file should not contain compile_pg statements
# An example is in rm_icc2_dp_scripts/pns_example.tcl
set PNS_CHARACTERIZE_FLOW             "true"  ;# Perform PG characterization and implementation
set TCL_COMPILE_PG_FILE               "/home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/compile_pg_example.tcl" ;# File should contain all the compile_pg_* commands to create the power networks 
# specified in the strategies in the TCL_PNS_FILE. 
# An example is in rm_icc2_dp_scripts/compile_pg_example.tcl
set TCL_PG_PUSHDOWN_FILE              "" ;# Create this file to facilitate manual pushdown and bypass auto pushdown in the flow.
set TCL_POST_PNS_FILE                 "" ;# If it exists, this file will be sourced after PG creation.
set TCL_USER_CREATE_POWER_PRE_SCRIPT  "" ;# An optional Tcl file to be sourced at the very beginning of the task
set TCL_USER_CREATE_POWER_POST_SCRIPT "" ;# An optional Tcl file to be sourced at the very end of the task
##########################################################################################
#                               PLACE PINS
##########################################################################################
##Note:Feedthroughs are disabled by default. Enable feedthroughs either through set_*_pin_constraints  Tcl commands or through Pin constraints file
set TCL_PIN_CONSTRAINT_FILE           "" ;# file sourced to apply set_*_pin_constraints to the design
set CUSTOM_PIN_CONSTRAINT_FILE        "" ;# will be loaded via read_pin_constraints -file
;# used for more complex pin constraints, 
;# or in constraint replay
set PLACE_PINS_SELF                   "true" ;# Set to true if the block's top level pins are not all connected to IO drivers.
set TIMING_PIN_PLACEMENT              "true" ;# Set to true for timing driven pin placement
set TCL_USER_PLACE_PINS_PRE_SCRIPT    "" ;# An optional Tcl file to be sourced at the very beginning of the task
set TCL_USER_PLACE_PINS_POST_SCRIPT   "" ;# An optional Tcl file to be sourced at the very end of the task
##########################################################################################
#                               PRE-TIMING
##########################################################################################
set TCL_USER_PRE_TIMING_PRE_SCRIPT    "" ;# An optional Tcl file to be sourced at the very beginning of the task
set TCL_USER_PRE_TIMING_POST_SCRIPT   "" ;# An optional Tcl file to be sourced at the very end of the task
##########################################################################################
#                               TIMING ESTIMATION
##########################################################################################
set TCL_TIMING_ESTIMATION_SETUP_FILE       "" ;# Specify any constraints prior to timing estimation
set TCL_USER_TIMING_ESTIMATION_PRE_SCRIPT  "" ;# An optional Tcl file to be sourced at the very beginning of the task
set TCL_USER_TIMING_ESTIMATION_POST_SCRIPT "" ;# An optional Tcl file to be sourced at the very end of the task
set TCL_LIB_CELL_DONT_USE_FILE             "" ;# A Tcl file for customized don't use ("set_lib_cell_purpose -exclude <purpose>" commands);
;#  to prevent estimate_timing picking a lib_cell not used by pnr flow.
;#  please specify non-optimization purpose lib cells in the file. 
##########################################################################################
#                               BUDGETING
##########################################################################################
set TCL_BUDGETING_SETUP_FILE                "" ;# Specify any constraints prior to budgeting
set TCL_BOUNDARY_BUDGETING_CONSTRAINTS_FILE "" ;# An optional user constraints file to override compute_budget_constraints
set TCL_USER_BUDGETING_PRE_SCRIPT           "" ;# An optional Tcl file to be sourced at the very beginning of the task
set TCL_USER_BUDGETING_POST_SCRIPT          "" ;# An optional Tcl file to be sourced at the very end of the task
##########################################################################################
## System Variables (there's no need to change the following)
##########################################################################################
set WORK_DIR ./work
set WORK_DIR_WRITE_DATA ./write_data_dir
if !{[file exists $WORK_DIR]} {file mkdir $WORK_DIR}
set SPLIT_CONSTRAINTS_LABEL_NAME split_constraints
set INIT_DP_LABEL_NAME init_dp
set PRE_SHAPING_LABEL_NAME pre_shaping
set PLACE_IO_LABEL_NAME place_io
set SHAPING_LABEL_NAME shaping
set PLACEMENT_LABEL_NAME placement
set CREATE_POWER_LABEL_NAME create_power
set CLOCK_TRUNK_PLANNING_LABEL_NAME clock_trunk_planning
set PLACE_PINS_LABEL_NAME place_pins
set PRE_TIMING_LABEL_NAME pre_timing
set TIMING_ESTIMATION_LABEL_NAME timing_estimation
set BUDGETING_LABEL_NAME budgeting
# Block label to be release by write_data
if {$DP_FLOW == "flat"} {
   set WRITE_DATA_LABEL_NAME timing_estimation
} else {
   set WRITE_DATA_LABEL_NAME budgeting
}
## Directories
set OUTPUTS_DIR "/home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/outputs_icc2"      ;# Directory to write output data files; mainly used by write_data.tcl
set REPORTS_DIR "/home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/rpts_icc2"         ;# Directory to write reports; mainly used by report_qor.tcl
set REPORTS_DIR_SPLIT_CONSTRAINTS $REPORTS_DIR/$SPLIT_CONSTRAINTS_LABEL_NAME
set REPORTS_DIR_INIT_DP $REPORTS_DIR/$INIT_DP_LABEL_NAME
set REPORTS_DIR_PRE_SHAPING $REPORTS_DIR/$PRE_SHAPING_LABEL_NAME
set REPORTS_DIR_PLACE_IO $REPORTS_DIR/$PLACE_IO_LABEL_NAME
set REPORTS_DIR_SHAPING $REPORTS_DIR/$SHAPING_LABEL_NAME
set REPORTS_DIR_PLACEMENT $REPORTS_DIR/$PLACEMENT_LABEL_NAME
set REPORTS_DIR_CREATE_POWER $REPORTS_DIR/$CREATE_POWER_LABEL_NAME
set REPORTS_DIR_CLOCK_TRUNK_PLANNING $REPORTS_DIR/$CLOCK_TRUNK_PLANNING_LABEL_NAME
set REPORTS_DIR_PLACE_PINS $REPORTS_DIR/$PLACE_PINS_LABEL_NAME
set REPORTS_DIR_PRE_TIMING $REPORTS_DIR/$PRE_TIMING_LABEL_NAME
set REPORTS_DIR_TIMING_ESTIMATION $REPORTS_DIR/$TIMING_ESTIMATION_LABEL_NAME
set REPORTS_DIR_BUDGETING $REPORTS_DIR/$BUDGETING_LABEL_NAME
if !{[file exists $REPORTS_DIR]} {file mkdir $REPORTS_DIR}
if !{[file exists $OUTPUTS_DIR]} {file mkdir $OUTPUTS_DIR}
if !{[file exists $REPORTS_DIR_SPLIT_CONSTRAINTS]} {file mkdir $REPORTS_DIR_SPLIT_CONSTRAINTS}
if !{[file exists $REPORTS_DIR_INIT_DP]} {file mkdir $REPORTS_DIR_INIT_DP}
if !{[file exists $REPORTS_DIR_PRE_SHAPING]} {file mkdir $REPORTS_DIR_PRE_SHAPING}
if !{[file exists $REPORTS_DIR_PLACE_IO]} {file mkdir $REPORTS_DIR_PLACE_IO}
if !{[file exists $REPORTS_DIR_SHAPING]} {file mkdir $REPORTS_DIR_SHAPING}
if !{[file exists $REPORTS_DIR_PLACEMENT]} {file mkdir $REPORTS_DIR_PLACEMENT}
if !{[file exists $REPORTS_DIR_CREATE_POWER]} {file mkdir $REPORTS_DIR_CREATE_POWER}
if !{[file exists $REPORTS_DIR_CLOCK_TRUNK_PLANNING]} {file mkdir $REPORTS_DIR_CLOCK_TRUNK_PLANNING}
if !{[file exists $REPORTS_DIR_PLACE_PINS]} {file mkdir $REPORTS_DIR_PLACE_PINS}
if !{[file exists $REPORTS_DIR_PRE_TIMING]} {file mkdir $REPORTS_DIR_PRE_TIMING}
if !{[file exists $REPORTS_DIR_TIMING_ESTIMATION]} {file mkdir $REPORTS_DIR_TIMING_ESTIMATION}
if !{[file exists $REPORTS_DIR_BUDGETING]} {file mkdir $REPORTS_DIR_BUDGETING}
if {[info exists env(LOGS_DIR)]} {
   set log_dir $env(LOGS_DIR)
} else {
   set log_dir /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/logs_icc2 
}
set search_path [list /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/rm_icc2_dp_scripts /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/rm_icc2_pnr_scripts $WORK_DIR ] 
lappend search_path .
##########################################################################################
#                               Optional Settings
##########################################################################################
set_message_info -id PVT-012 -limit 1
set_message_info -id PVT-013 -limit 1
set search_path "/home/venkatesh/VSDBabySoC/src/lib"
set_app_var link_library "/home/venkatesh/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.db /home/venkatesh/VSDBabySoC/src/lib/avsdpll.db /home/venkatesh/VSDBabySoC/src/lib/avsddac.db"
puts "RM-info : Completed script [info script]\n"
RM-info : Completed script /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/icc2_dp_setup.tcl

RM-info : create_lib ./work/vsdbabysocsky130_fd_sc_hd -tech /home/venkatesh/VSDBabySoC/synopsys_ICC2flow_130nm/synopsys_skywater_flow_nominal/milkywayORtechFiles/sky130_fd_sc_hd.tf -ref_libs {/home/venkatesh/VSDBabySoC/synopsys_ICC2flow_130nm/synopsys_skywater_flow_nominal/LEF/sky130_v5_7magic.lef /home/venkatesh/VSDBabySoC/src/lef/avsddac.lef /home/venkatesh/VSDBabySoC/src/lef/avsdpll.lef}
Warning: sky130_fd_sc_hd.tf line 26, unsupported syntax 'fatWireViaKeepoutMode' in 'Technology' section. It will be ignored. (TECH-002)
Warning: Section DesignRule '' is missing the attribute 'stackable'. (sky130_fd_sc_hd.tf line 846) (TECH-026)
Warning: Layer 'via4/met4' is missing the attribute 'maxStackLevel'. (sky130_fd_sc_hd.tf line 846) (TECH-026)
Warning: Section DesignRule '' is missing the attribute 'stackable'. (sky130_fd_sc_hd.tf line 854) (TECH-026)
Warning: Layer 'via4/met5' is missing the attribute 'maxStackLevel'. (sky130_fd_sc_hd.tf line 854) (TECH-026)
Warning: Section DesignRule '' is missing the attribute 'stackable'. (sky130_fd_sc_hd.tf line 862) (TECH-026)
Warning: Layer 'via3/met3' is missing the attribute 'maxStackLevel'. (sky130_fd_sc_hd.tf line 862) (TECH-026)
Warning: Section DesignRule '' is missing the attribute 'stackable'. (sky130_fd_sc_hd.tf line 870) (TECH-026)
Warning: Layer 'via3/met4' is missing the attribute 'maxStackLevel'. (sky130_fd_sc_hd.tf line 870) (TECH-026)
Warning: Section DesignRule '' is missing the attribute 'stackable'. (sky130_fd_sc_hd.tf line 878) (TECH-026)
Warning: Layer 'via2/met2' is missing the attribute 'maxStackLevel'. (sky130_fd_sc_hd.tf line 878) (TECH-026)
Warning: Section DesignRule '' is missing the attribute 'stackable'. (sky130_fd_sc_hd.tf line 886) (TECH-026)
Warning: Layer 'via2/met3' is missing the attribute 'maxStackLevel'. (sky130_fd_sc_hd.tf line 886) (TECH-026)
Warning: Section DesignRule '' is missing the attribute 'stackable'. (sky130_fd_sc_hd.tf line 894) (TECH-026)
Warning: Layer 'via/met1' is missing the attribute 'maxStackLevel'. (sky130_fd_sc_hd.tf line 894) (TECH-026)
Warning: Section DesignRule '' is missing the attribute 'stackable'. (sky130_fd_sc_hd.tf line 902) (TECH-026)
Warning: Layer 'via/met2' is missing the attribute 'maxStackLevel'. (sky130_fd_sc_hd.tf line 902) (TECH-026)
Warning: Section DesignRule '' is missing the attribute 'stackable'. (sky130_fd_sc_hd.tf line 910) (TECH-026)
Warning: Layer 'mcon/li1' is missing the attribute 'maxStackLevel'. (sky130_fd_sc_hd.tf line 910) (TECH-026)
Warning: Section DesignRule '' is missing the attribute 'stackable'. (sky130_fd_sc_hd.tf line 918) (TECH-026)
Warning: Layer 'mcon/met1' is missing the attribute 'maxStackLevel'. (sky130_fd_sc_hd.tf line 918) (TECH-026)
Warning: DesignRule is defined with non-consecutive layers 'via4' and 'met4'. (sky130_fd_sc_hd.tf line 851) (TECH-034)
Warning: DesignRule is defined with non-consecutive layers 'via4' and 'met5'. (sky130_fd_sc_hd.tf line 859) (TECH-034)
Warning: DesignRule is defined with non-consecutive layers 'via3' and 'met3'. (sky130_fd_sc_hd.tf line 867) (TECH-034)
Warning: DesignRule is defined with non-consecutive layers 'via3' and 'met4'. (sky130_fd_sc_hd.tf line 875) (TECH-034)
Warning: DesignRule is defined with non-consecutive layers 'via2' and 'met2'. (sky130_fd_sc_hd.tf line 883) (TECH-034)
Warning: DesignRule is defined with non-consecutive layers 'via2' and 'met3'. (sky130_fd_sc_hd.tf line 891) (TECH-034)
Warning: DesignRule is defined with non-consecutive layers 'via' and 'met1'. (sky130_fd_sc_hd.tf line 899) (TECH-034)
Warning: DesignRule is defined with non-consecutive layers 'via' and 'met2'. (sky130_fd_sc_hd.tf line 907) (TECH-034)
Warning: DesignRule is defined with non-consecutive layers 'mcon' and 'li1'. (sky130_fd_sc_hd.tf line 915) (TECH-034)
Warning: DesignRule is defined with non-consecutive layers 'mcon' and 'met1'. (sky130_fd_sc_hd.tf line 923) (TECH-034)
Information: Loading technology file '/home/venkatesh/VSDBabySoC/synopsys_ICC2flow_130nm/synopsys_skywater_flow_nominal/milkywayORtechFiles/sky130_fd_sc_hd.tf' (FILE-007)
...Created 4 lib groups

... 4 cell libraries and 1 aggregate library to build.


... checking whether need to build aggregate library: EXPLORE_macros.ndm (1/1)
... checking whether can reuse library under output directory
reuse existing cell library under output directory: CLIBs/EXPLORE_macros.ndm

... checking whether need to build cell library: sky130_fd_sc_hd__tt_025C_1v80.ndm (1/4)
... checking whether can reuse library under output directory
reuse existing cell library under output directory: CLIBs/sky130_fd_sc_hd__tt_025C_1v80.ndm

... checking whether need to build cell library: sky130_fd_sc_hd__tt_025C_1v80_physical_only.ndm (4/4)
... checking whether can reuse library under output directory
reuse existing cell library under output directory: CLIBs/sky130_fd_sc_hd__tt_025C_1v80_physical_only.ndm

... 4 cell libraries and 1 aggregate library can be reused.



Information: Successfully built 3 reference libraries: sky130_fd_sc_hd__tt_025C_1v80.ndm EXPLORE_macros.ndm sky130_fd_sc_hd__tt_025C_1v80_physical_only.ndm. (LIB-093)
RM-info : Reading full chip verilog (/home/venkatesh/VSDBabySoC/output/vsdbabysoc_net.v)
Information: Reading Verilog into new design 'vsdbabysoc/init_dp' in library 'vsdbabysocsky130_fd_sc_hd'. (VR-012)
Loading verilog file '/home/venkatesh/VSDBabySoC/output/vsdbabysoc_net.v'
Number of modules read: 2
Top level ports: 7
Total ports in all modules: 19
Total nets in all modules: 3004
Total instances in all modules: 2744
Elapsed = 00:00:00.04, CPU = 00:00:00.04
RM-info : Sourcing /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/init_design.tech_setup.tcl
puts "RM-info : Running script [info script]\n"
RM-info : Running script /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/init_design.tech_setup.tcl

##########################################################################################
# Tool: IC Compiler II 
# Script: tech_setup.tcl
# Version: P-2019.03-SP4
# Copyright (C) 2014-2019 Synopsys, Inc. All rights reserved.
##########################################################################################
## Set routing_direction and track_offset
if {$ROUTING_LAYER_DIRECTION_OFFSET_LIST != ""} {
        foreach direction_offset_pair $ROUTING_LAYER_DIRECTION_OFFSET_LIST {
                set layer [lindex $direction_offset_pair 0]
                set direction [lindex $direction_offset_pair 1]
                set offset [lindex $direction_offset_pair 2]
                set_attribute [get_layers $layer] routing_direction $direction
                if {$offset != ""} {
                        set_attribute [get_layers $layer] track_offset $offset
                }
        }
} else {
        puts "RM-error : ROUTING_LAYER_DIRECTION_OFFSET_LIST is not specified. You must manually set routing layer directions and offsets!"
}
Information: The design specific attribute override for layer 'met1' is set in the current block 'vsdbabysoc', because the actual library setting may not be overwritten. (ATTR-12)
Information: The design specific attribute override for layer 'met2' is set in the current block 'vsdbabysoc', because the actual library setting may not be overwritten. (ATTR-12)
Information: The design specific attribute override for layer 'met3' is set in the current block 'vsdbabysoc', because the actual library setting may not be overwritten. (ATTR-12)
Information: The design specific attribute override for layer 'met4' is set in the current block 'vsdbabysoc', because the actual library setting may not be overwritten. (ATTR-12)
Information: The design specific attribute override for layer 'met5' is set in the current block 'vsdbabysoc', because the actual library setting may not be overwritten. (ATTR-12)
## Set site default
if {$SITE_DEFAULT != ""} {
        set_attribute [get_site_defs] is_default false
        set_attribute [get_site_defs $SITE_DEFAULT] is_default true
}
## Set site symmetry
if {$SITE_SYMMETRY_LIST != ""} {
        foreach sym_pair $SITE_SYMMETRY_LIST {
                set site_name [lindex $sym_pair 0]
                set site_sym [lindex $sym_pair 1]
                set_attribute [get_site_defs $site_name] symmetry $site_sym
        }       
}
puts "RM-info : Completed script [info script]\n"
RM-info : Completed script /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/init_design.tech_setup.tcl

RM-info : Sourcing /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/init_design.read_parasitic_tech_example.tcl
puts "RM-info: Running script [info script]\n"
RM-info: Running script /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/init_design.read_parasitic_tech_example.tcl

##########################################################################################
# Tool: IC Compiler II
# Script: init_design.read_parasitic_tech_example.tcl (template)
# Version: P-2019.03-SP4
# Copyright (C) 2014-2019 Synopsys, Inc. All rights reserved.
##########################################################################################
##############################################################################################
# The following is a sample script to read two TLU+ files, 
# which you can expand to accomodate your design.
##############################################################################################
########################################
## Variables
########################################
## Parasitic tech files for read_parasitic_tech command; expand the section as needed
set parasitic1                          "temp1" ;# name of parasitic tech model 1
set tluplus_file($parasitic1)           "/home/venkatesh/VSDBabySoC/synopsys_ICC2flow_130nm/synopsys_skywater_flow_nominal/itf_files/skywater130.nominal.tluplus" ;# TLU+ files to read for parasitic 1
set layer_map_file($parasitic1)         "/home/venkatesh/VSDBabySoC/synopsys_ICC2flow_130nm/synopsys_skywater_flow_nominal/milkywayORtechFiles/skywater130_fd_sc_hd.map" ;# layer mapping file between ITF and tech for parasitic 1
#set parasitic2                         "temp2" ;# name of parasitic tech model 2
#set tluplus_file($parasitic2)           "/home/kunal/design/picosoc/pdk/sample_180nm.tluplus" ;# TLU+ files to read for parasitic 2
#set layer_map_file($parasitic2)         "" ;# layer mapping file between ITF and tech for parasitic 2
########################################
## Read parasitic files
########################################
## Read in the TLUPlus files first.
#  Later on in the corner constraints, you can then refer to these parasitic models.
foreach p [array name tluplus_file] {  
        puts "RM-info: read_parasitic_tech -tlup $tluplus_file($p) -layermap $layer_map_file($p) -name $p"
        #read_parasitic_tech -tlup $tluplus_file($p) -layermap $layer_map_file($p) -name $p
        read_parasitic_tech -tlup $tluplus_file($p)  -name $p

}
RM-info: read_parasitic_tech -tlup /home/venkatesh/VSDBabySoC/synopsys_ICC2flow_130nm/synopsys_skywater_flow_nominal/itf_files/skywater130.nominal.tluplus -layermap /home/venkatesh/VSDBabySoC/synopsys_ICC2flow_130nm/synopsys_skywater_flow_nominal/milkywayORtechFiles/skywater130_fd_sc_hd.map -name temp1
Information: The command 'read_parasitic_tech' cleared the undo history. (UNDO-016)
puts "RM-info: Completed script [info script]\n"
RM-info: Completed script /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/init_design.read_parasitic_tech_example.tcl

Using libraries: vsdbabysocsky130_fd_sc_hd sky130_fd_sc_hd__tt_025C_1v80 EXPLORE_macros sky130_fd_sc_hd__tt_025C_1v80_physical_only
Linking block vsdbabysocsky130_fd_sc_hd:vsdbabysoc/init_dp.design
Information: User units loaded from library 'sky130_fd_sc_hd__tt_025C_1v80' (LNK-040)
Design 'vsdbabysoc' was successfully linked.
[icc2-lic Sun Jul 21 02:29:44 2024] Command 'initialize_floorplan' requires licenses
[icc2-lic Sun Jul 21 02:29:44 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:29:44 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:44 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:44 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:29:44 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:44 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:44 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:29:44 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:44 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:44 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:44 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:44 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:29:44 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:44 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:44 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:29:44 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:44 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:44 2024] Check-out of alternate set of keys directly with queueing was successful
Removing existing floorplan objects
Creating core...
Core utilization ratio = 30.02%
Unplacing all cells...
Creating site array...
Creating routing tracks...
Initializing floorplan completed.
Information: The command 'save_lib' cleared the undo history. (UNDO-016)
Saving all libraries...
Information: Saving 'vsdbabysocsky130_fd_sc_hd:vsdbabysoc/init_dp.design' to 'vsdbabysocsky130_fd_sc_hd:floorplan_in_progress.design'. (DES-028)
[icc2-lic Sun Jul 21 02:29:44 2024] Command 'place_pins' requires licenses
[icc2-lic Sun Jul 21 02:29:44 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:29:44 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:44 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:44 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:29:44 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:44 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:44 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:29:44 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:44 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:44 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:44 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:44 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:29:44 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:44 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:44 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:29:44 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:44 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:44 2024] Check-out of alternate set of keys directly with queueing was successful
Information: Starting 'place_pins' (FLW-8000)
Information: Time: 2024-07-21 02:29:44 / Session: 0.02 hr / Command: 0.00 hr / Memory: 387 MB (FLW-8100)
Load DB...
CPU Time for load db: 00:00:00.00u 00:00:00.00s 00:00:00.00e: 

Min routing layer: met1
Max routing layer: met5


CPU Time for Top Level Pre-Route Processing: 00:00:00.00u 00:00:00.00s 00:00:00.00e: 
Number of block ports: 7
Number of block pin locations assigned from router: 0
CPU Time for Pin Preparation: 00:00:00.00u 00:00:00.00s 00:00:00.00e: 
Number of PG ports on blocks: 0
Number of pins created: 7
CPU Time for Pin Creation: 00:00:00.01u 00:00:00.00s 00:00:00.01e: 
Total Pin Placement CPU Time: 00:00:00.02u 00:00:00.00s 00:00:00.02e: 
Information: Ending 'place_pins' (FLW-8001)
Information: Time: 2024-07-21 02:29:44 / Session: 0.02 hr / Command: 0.00 hr / Memory: 387 MB (FLW-8100)
Writing out physical pin constraints based on existing pins ... done
0 total number of pre-routes loaded
0 total number of vias loaded
4 total number of edges loaded
------------------------ Start Of Pin Layer Check -------------------------
------------------------- End Of Pin Layer Check --------------------------

----------------------- Start Of Missing Pin Check ------------------------
------------------------ End Of Missing Pin Check -------------------------

----------------------- Start Of Pin Spacing Check ------------------------
------------------------ End Of Pin Spacing Check -------------------------

-------------------- Start Of Technology Spacing Check --------------------
--------------------- End Of Technology Spacing Check ---------------------

------------------------- Start Of Pin Side Check -------------------------
-------------------------- End Of Pin Side Check --------------------------

----------------------- Start Of Pin Stacking Check -----------------------
------------------------ End Of Pin Stacking Check ------------------------

------------------------ Start Of Pin Short Check -------------------------
------------------------- End Of Pin Short Check --------------------------

------------------- Start Of Pin Pre-Route Short Check --------------------
-------------------- End Of Pin Pre-Route Short Check ---------------------

No violation has been found
***Summary***
---------------------------------------------------------------------------
Type of Violation        |                                            Count
-------------------------+-------------------------------------------------
Layer Violation          |                                               0
Missing Pins             |                                               0
Pin PreRoute             |                                               0
Pin Short                |                                               0
Pin Side                 |                                               0
Pin Spacing              |                                               0
Stacking Violation       |                                               0
Technology Spacing       |                                               0
-------------------------+-------------------------------------------------
Total Violations         |                                               0
---------------------------------------------------------------------------
Saving all libraries...
RM-info : Running connect_pg_net -automatic on all blocks
Information: Total 2 netlist change(s) and disconnections have been made to resolve conflicts between power intent and PG netlist. (UPF-073)
****************************************
Report : Power/Ground Connection Summary
Design : vsdbabysoc
Version: T-2022.03-SP5
Date   : Sun Jul 21 02:29:44 2024
****************************************
P/G net name                  P/G pin count(previous/current)
--------------------------------------------------------------------------------
Power net VDD                 0/2745
Unconnected nwell pins        2741
Ground net VSS                0/2744
Unconnected pwell pins        2741
--------------------------------------------------------------------------------
Information: connections of 5489 power/ground pin(s) are created or changed.
Information: The command 'save_block' cleared the undo history. (UNDO-016)
Information: Saving 'vsdbabysocsky130_fd_sc_hd:vsdbabysoc/init_dp.design' to 'vsdbabysocsky130_fd_sc_hd:vsdbabysoc/pre_shaping.design'. (DES-028)
Saving all libraries...
RM-info : Sourcing /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/init_design.read_parasitic_tech_example.tcl
puts "RM-info: Running script [info script]\n"
RM-info: Running script /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/init_design.read_parasitic_tech_example.tcl

##########################################################################################
# Tool: IC Compiler II
# Script: init_design.read_parasitic_tech_example.tcl (template)
# Version: P-2019.03-SP4
# Copyright (C) 2014-2019 Synopsys, Inc. All rights reserved.
##########################################################################################
##############################################################################################
# The following is a sample script to read two TLU+ files, 
# which you can expand to accomodate your design.
##############################################################################################
########################################
## Variables
########################################
## Parasitic tech files for read_parasitic_tech command; expand the section as needed
set parasitic1                          "temp1" ;# name of parasitic tech model 1
set tluplus_file($parasitic1)           "/home/venkatesh/VSDBabySoC/synopsys_ICC2flow_130nm/synopsys_skywater_flow_nominal/itf_files/skywater130.nominal.tluplus" ;# TLU+ files to read for parasitic 1
set layer_map_file($parasitic1)         "/home/venkatesh/VSDBabySoC/synopsys_ICC2flow_130nm/synopsys_skywater_flow_nominal/milkywayORtechFiles/skywater130_fd_sc_hd.map" ;# layer mapping file between ITF and tech for parasitic 1
#set parasitic2                         "temp2" ;# name of parasitic tech model 2
#set tluplus_file($parasitic2)           "/home/kunal/design/picosoc/pdk/sample_180nm.tluplus" ;# TLU+ files to read for parasitic 2
#set layer_map_file($parasitic2)         "" ;# layer mapping file between ITF and tech for parasitic 2
########################################
## Read parasitic files
########################################
## Read in the TLUPlus files first.
#  Later on in the corner constraints, you can then refer to these parasitic models.
foreach p [array name tluplus_file] {  
        puts "RM-info: read_parasitic_tech -tlup $tluplus_file($p) -layermap $layer_map_file($p) -name $p"
        #read_parasitic_tech -tlup $tluplus_file($p) -layermap $layer_map_file($p) -name $p
        read_parasitic_tech -tlup $tluplus_file($p)  -name $p

}
RM-info: read_parasitic_tech -tlup /home/venkatesh/VSDBabySoC/synopsys_ICC2flow_130nm/synopsys_skywater_flow_nominal/itf_files/skywater130.nominal.tluplus -layermap /home/venkatesh/VSDBabySoC/synopsys_ICC2flow_130nm/synopsys_skywater_flow_nominal/milkywayORtechFiles/skywater130_fd_sc_hd.map -name temp1
puts "RM-info: Completed script [info script]\n"
RM-info: Completed script /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/init_design.read_parasitic_tech_example.tcl

RM-info : Loading TCL_MCMM_SETUP_FILE (/home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/init_design.mcmm_example.auto_expanded.tcl)
puts "RM-info: Running script [info script]\n"
RM-info: Running script /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/init_design.mcmm_example.auto_expanded.tcl

##########################################################################################
# Tool: IC Compiler II
# Script: init_design.mcmm_example.auto_expanded.tcl (template)
# Version: P-2019.03-SP4
# Copyright (C) 2014-2019 Synopsys, Inc. All rights reserved.
##########################################################################################
## Note :
#  1. To see the full list of mode / corner / scenario specific commands, 
#      refer to SolvNet 1777585 : "Multicorner-multimode constraint classification" 
#
#  2. Corner operating conditions are recommended to be specified directly through 
#     set_process_number, set_voltage and set_temperature
#
#       The PVT resolution function always finds the closest PVT match between the operating conditions and 
#       the library pane.
#       A Corner operating condition may be specified directly with the set_process_number, set_voltage and 
#       set_temperature commands or indirectly with the set_operating_conditions command.
#       The set_process_label command may be used to distinguish between library panes with the same PVT 
#       values but different process labels.
##############################################################################################
# The following is a sample script to create two scenarios with scenario constraints provided,
# and let the constraints auto expanded to associated modes and scenarios. At the end of script,
# remove_duplicate_timing_contexts is used to improve runtime and capacity without loss of constraints.
# Reading of the TLUPlus files should be done beforehand,
# so the parasitic models can be referred to in the constraints.
# Specify TCL_PARASITIC_SETUP_FILE in icc2_common_setup.tcl for your read_parasitic_tech commands.
# read_parasitic_tech_example.tcl is provided as an example.
##############################################################################################
########################################
## Variables
########################################
## Scenario constraints; expand the section as needed
set scenario1                           "func1" ;# name of scenario1
set scenario_constraints($scenario1)    "/home/venkatesh/VSDBabySoC/src/sdc/vsdbabysoc_synthesis.sdc" ;# for all scenario1 specific constraints
#set scenario2                          "func2" ;# name of scenario2
#set scenario_constraints($scenario2)    "/home/kunal/design/picosoc/rtl/picorv32.sdc" ;# for all scenario2 specific constraints
########################################
## Create modes, corners, and scenarios
########################################
remove_modes -all; remove_corners -all; remove_scenarios -all
foreach s [array name scenario_constraints] {
        create_mode $s
        create_corner $s
        create_scenario -name $s -mode $s -corner $s
        set_parasitic_parameters -late_spec temp1 -early_spec temp1 -library ${DESIGN_NAME}${LIBRARY_SUFFIX} 
        set_voltage 1.10 -corner [current_corner] -object_list [get_supply_nets VDD] 
}
Created scenario func1 for mode func1 and corner func1
All analysis types are activated.
########################################
## Populate constraints 
########################################
## Populate scenario constraints which will then be automatically expanded to its associated modes and corners
foreach s [array name scenario_constraints] {
        current_scenario $s
        puts "RM-info: current_scenario $s"
        puts "RM-info: source $scenario_constraints($s)"
        source $scenario_constraints($s)

        # pls ensure $scenario_constraints($s) includes set_parasitic_parameters command for the corresponding corner,
        # for example, set_parasitic_parameters -late_spec $parasitics1 -early_spec $parasitics2,
        # where the command points to the parasitics read by the read_parasitic_tech commands.
        # Specify TCL_PARASITIC_SETUP_FILE in icc2_common_setup.tcl for your read_parasitic_tech commands.
        # read_parasitic_tech_example.tcl is provided as an example.    
}
RM-info: current_scenario func1
RM-info: source /home/venkatesh/VSDBabySoC/src/sdc/vsdbabysoc_synthesis.sdc
Warning: Creating a clock on internal pin 'pll/CLK'. (UIC-019)
Warning: The 'set_max_fanout' command is not supported in this program.  The command will be ignored. (CSTR-011)
Information: Timer using 8 threads
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: set_max_delay is forcing pin 'dac/OUT' to be a timing startpoint. (UIC-011)
########################################
## Configure analysis settings for scenarios
########################################
# Below are just examples to show usage of set_scenario_status (actual usage shold depend on your objective)
# scenario1 is a setup scenario and scenario2 is a hold scenario
set_scenario_status $scenario1 -none -setup true -hold true -leakage_power true -dynamic_power true -max_transition true -max_capacitance true -min_capacitance false -active true
Scenario func1 (mode func1 corner func1) is active for setup/hold/leakage_power/dynamic_power/max_transition/max_capacitance analysis.
#set_scenario_status $scenario2 -none -setup false -hold true -leakage_power true -dynamic_power false -max_transition true -max_capacitance false -min_capacitance true -active true
#redirect -file ${REPORTS_DIR}/${INIT_DESIGN_BLOCK_NAME}.report_scenarios.rpt {report_scenarios} 
redirect -file ${DESIGN_NAME}.report_scenarios.rpt {report_scenarios}
## To remove duplicate modes, corners, scenarios, and to improve runtime and capacity without loss of constraints :
remove_duplicate_timing_contexts
Information: No Scenario reduction opportunities were found
Information: No Mode reduction opportunities were found
Information: No Corner reduction opportunities were found
Information: No Mode, Corner or Scenario reduction was possible
puts "RM-info: Completed script [info script]\n"
RM-info: Completed script /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/init_design.mcmm_example.auto_expanded.tcl

[icc2-lic Sun Jul 21 02:29:45 2024] Command 'create_placement' requires licenses
[icc2-lic Sun Jul 21 02:29:45 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:29:45 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:45 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:45 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:29:45 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:45 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:45 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:29:45 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:45 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:45 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:45 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:45 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:29:45 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:45 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:45 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:29:45 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:45 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:45 2024] Check-out of alternate set of keys directly with queueing was successful
Information: Starting 'create_placement' (FLW-8000)
Information: Time: 2024-07-21 02:29:45 / Session: 0.02 hr / Command: 0.00 hr / Memory: 387 MB (FLW-8100)
Warning: The -congestion_effort option only effects the standard cell congestion reduction, which is not enabled in this run. (DPP-221)
Creating appropriate block views (if needed)...
Multi-Processing Summary
  Max number of cores for parent process: 8; hostname: sfalvsd
  No distributed processing
Command Option Settings Summary
  -floorplan -effort medium -congestion -congestion_effort medium
Design summary for global macro placement
  Number of std cells                 : 2741
  Number of floating/fixed hard macros: 2 / 0
  Number of blocks/MIBs/VAs           : 0 / 0 / 0
  Number of (unique) edit groups      : 0
Information: Trace mode is set to dfa. (DPP-053)
Info: Running global macro placer.
coarse place 0% done.
coarse place 50% done.
coarse place 100% done.
Running macro grouping.
Planning locations for 1 groups of macros.
Running packing.
Doing congestion reduction iteration 1 of 1
  Placing standard cells for congestion reduction
  Running router for congestion reduction
Warning: Layer li1 does not have a preferred direction, assigning to vertical. (ZRT-025)
Warning: Ignore pin (cell avsdpll, pin VDD) on layer nwell. (ZRT-030)
Warning: Ignore pin (cell avsdpll, pin VDD#2_1) on layer nwell. (ZRT-030)
Warning: Ignore pin (cell avsdpll, pin VDD#2) on layer nwell. (ZRT-030)
Warning: Ignore pin (cell avsdpll, pin VDD#3) on layer nwell. (ZRT-030)
Warning: Ignore 2 top cell ports with no pins. (ZRT-027)
Warning: Layer met4 pitch 0.920 may be too small: wire/via-down 0.615, wire/via-up 1.040. (ZRT-026)
Warning: Master cell avsddac has duplicated redundant library pin shapes at {0.000 0.000} {0.170 0.170} on layer li1. (ZRT-625)
Warning: Master cell avsddac has duplicated redundant library pin shapes at {0.000 0.000} {0.170 0.170} on layer li1. (ZRT-625)
Warning: Corner func1:  0 process number, 0 process label, 3 voltage, and 0 temperature mismatches. (PVT-030)
Warning: 2743 cells affected for early, 2743 for late. (PVT-031)
Warning: 0 port driving_cells affected for early, 0 for late. (PVT-034)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Updating keepouts for congestion reduction for top
AOR: Running advanced overlap remover
AOR: Running solution-finding phases on space Core
AOR: Solution-finding phase 1 of at most 9
AOR: Running solution-finding phase with 50 macros per window
AOR: Running global instance.
AOR: Legal solution found!
AOR: Total cost after solution-finding phases: 13600
AOR: Running post-optimization phase with 20 macros per window
AOR: Running global instance.
AOR: Total cost after post-optimization phase: 13600
AOR: Running post-optimization phase with 10 macros per window
AOR: Running global instance.
AOR: Total cost after post-optimization phase: 13600
AOR: CPU time: 0.01
Remove overlaps
Generating automatic soft blockages for vsdbabysoc/init_dp, hor/vert channel sizes are 16.32/16.32
Macro placement done.
Placing top level std cells.
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
coarse place 0% done.
coarse place 20% done.
coarse place 40% done.
coarse place 60% done.
coarse place 80% done.
coarse place 100% done.
Running block placement.
Running timing driven standard cell placement
No editable block found.
Running virtual optimization on full netlist ...
Created estimated_corner
Information: The stitching and editing of coupling caps is turned OFF for design 'vsdbabysocsky130_fd_sc_hd:vsdbabysoc/init_dp.design'. (TIM-125)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Information: Design Average RC for design vsdbabysoc  (NEX-011)
Information: r = 0.497486 ohm/um, via_r = 3.489162 ohm/cut, c = 0.143348 ff/um, cc = 0.000000 ff/um (X dir) (NEX-017)
Information: r = 0.733512 ohm/um, via_r = 3.511051 ohm/cut, c = 0.125840 ff/um, cc = 0.000000 ff/um (Y dir) (NEX-017)
Number of Site types in the design = 1
Setting up Chip Core
Chip Core shape: (50000 50000) (15285200 15282000)
Number of VARs = 1
Number of unique PDs = 1
Number of Power Domains = 1
Number of Voltage Areas = 1
Number of supply Nets = 2
Number of used supplies = 0
Blocked VAs: 
Information: The RC mode used is VR for design 'vsdbabysoc'. (NEX-022)
Information: Update timing completed net estimation for all the timing graph nets (TIM-111)
Information: Net estimation statistics: timing graph nets = 2763, routed nets = 0, across physical hierarchy nets = 0, parasitics cached nets = 2763, delay annotated nets = 0, parasitics annotated nets = 0, multi-voltage nets = 19. (TIM-112)
Information: Initializing classic cellmap without advanced rules enabled and without PDC enabled
Information: The following app options are used in cellmap
        place.legalize.enable_color_aware_placement : false
        place.legalize.use_nll_query_cm : false
        place.legalize.enable_advanced_legalizer : false
        place.legalize.enable_prerouted_net_check : true
        place.legalize.enable_advanced_prerouted_net_check : false
        place.legalize.always_continue : true
        place.legalize.limit_legality_checks : false
        place.common.pnet_aware_density : 1.0000
        place.common.pnet_aware_min_width : 0.0000
        place.common.pnet_aware_layers : {}
        place.common.use_placement_model : false
        place.common.enable_advanced_placement_model : true
        cts.placement.cell_spacing_rule_style : maximum
Total 0.0500 seconds to build cellmap data
INFO: creating 50(r) x 50(c) GridCells YDim 30.664 XDim 30.6704
INFO: creating 50(r) x 50(c) GridCells YDim 30.664 XDim 30.6704
Total 0.0900 seconds to load 2741 cell instances into cellmap, 2741 cells are off site row
Moveable cells: 2741; Application fixed cells: 0; Macro cells: 0; User fixed cells: 0
Average cell width 3.3452, cell height 2.7200, cell area 9.0989 for total 2741 placed and application fixed cells
Information: Current block utilization is '0.01520', effective utilization is '0.01522'. (OPT-055)
APSINFO: No multi-Vth libcells found, turning off percentage LVT optimization flow
Drc Mode Option: auto
Warning: Cannot find buffers. Net core/n1164 cannot be buffered. (OPT-019)
APS-VIPO:Using met3 layer for chain buffer model creation
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
APS-VIPO: Total cell number is 2750
estimate timing (5%) done
estimate timing (10%) done
estimate timing (15%) done
estimate timing (20%) done
estimate timing (25%) done
estimate timing (30%) done
estimate timing (35%) done
estimate timing (40%) done
estimate timing (45%) done
estimate timing (50%) done
estimate timing (55%) done
estimate timing (60%) done
estimate timing (65%) done
estimate timing (70%) done
estimate timing (75%) done
estimate timing (80%) done
estimate timing (85%) done
estimate timing (90%) done
estimate timing (95%) done
estimate timing (100%) done
In the step 1, 1620 cells are processed
In the step 2, 2750 cells are processed
No estimate_timing rule detected on nets.
Virtually optimized 902 out of 2750 cells
Virtually buffered 902 cell outputs
Back-annotation statistics for mode 'func1'
  5242 back-annotations generated.
  968 have been transition time back annotations.
  0 have been skipped.
  Back-annotation succeeded on all arcs
net weight 3 is set for 0 number of nets by estimate_timing out of 3007 nets 
Information: The net parasitics of block vsdbabysoc are cleared. (TIM-123)
Creating appropriate block views (if needed)...
Design summary for global macro placement
  Number of std cells                 : 2741
  Number of floating/fixed hard macros: 2 / 0
  Number of blocks/MIBs/VAs           : 0 / 0 / 0
  Number of (unique) edit groups      : 0
Updating keepouts for congestion reduction for top
AOR: Running advanced overlap remover
AOR: Running solution-finding phases on space Core
AOR: Solution-finding phase 1 of at most 9
AOR: Running solution-finding phase with 50 macros per window
AOR: Running global instance.
AOR: Legal solution found!
AOR: Total cost after solution-finding phases: 9900
AOR: Running post-optimization phase with 20 macros per window
AOR: Running global instance.
AOR: Total cost after post-optimization phase: 9900
AOR: Running post-optimization phase with 10 macros per window
AOR: Running global instance.
AOR: Total cost after post-optimization phase: 9900
AOR: CPU time: 0
Remove overlaps
Generating automatic soft blockages for vsdbabysoc/init_dp, hor/vert channel sizes are 16.32/16.32
Macro Displacement Report
  Maximum move distance macro: dac distance: 0.68
  Maximum move distance macro: pll distance: 0.31
  Average move distance (only considering moved macros): 0.50
  Average move distance (over all macros): 0.50
  Total move distance: 0.99
  Number of moved macros / total number of macros: 2/2
Placing top level std cells.
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
coarse place 40% done.
coarse place 60% done.
coarse place 80% done.
coarse place 100% done.
Running block placement.
Floorplan placement done.
[icc2-lic Sun Jul 21 02:29:53 2024] Command 'report_placement' requires licenses
[icc2-lic Sun Jul 21 02:29:53 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:53 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:53 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out of alternate set of keys directly with queueing was successful
****************************************
Report : report_placement
Design : vsdbabysoc
Version: T-2022.03-SP5
Date   : Sun Jul 21 02:29:53 2024
****************************************
  ==================
  Note: Ignoring violations of fixed cells or between fixed pairs of cells. 
        To include violations of / between fixed cells, disable -ignore_fixed. 
  ==================

  Wire length report (all)
  ==================
  wire length in design vsdbabysoc/init_dp: 86972.151 microns.
  wire length in design vsdbabysoc/init_dp (see through blk pins): 86972.151 microns.
  ------------------
  Total wire length: 86972.151 microns.

  Physical hierarchy violations report
  ====================================
  Violations in design vsdbabysoc/init_dp:
     0 cells have placement violation.
  ------------------------------------
  Total 0 cells have placement violation.

  Voltage area violations report
  ====================================
  Voltage area placement violations in design vsdbabysoc/init_dp:
     0 cells placed outside the voltage area which they belong to.
  ------------------------------------
  Total 0 macro cells placed outside the voltage area which they belong to.

  Hard macro to hard macro overlap report
  =======================================
  HM to HM overlaps in design vsdbabysoc/init_dp: 0
  ---------------------------------------
  Total hard macro to hard macro overlaps: 0

Information: Default error view vsdbabysoc_dpplace.err is created in GUI error browser. (DPP-054)
Information: Elapsed time for create_placement excluding pending time: 00:00:08.18. (DPUI-902)
Information: CPU time for create_placement : 00:00:13.99. (DPUI-903)
Information: Peak memory usage for create_placement : 637 MB. (DPUI-904)
Information: Ending 'create_placement' (FLW-8001)
Information: Time: 2024-07-21 02:29:53 / Session: 0.02 hr / Command: 0.00 hr / Memory: 637 MB (FLW-8100)
Information: The command 'save_block' cleared the undo history. (UNDO-016)
Saving all libraries...
RM-info : Sourcing TCL_PNS_FILE (/home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/pns_example.tcl)
### Example script.  For details please refer to the IC Compiler II Design Planning User Guide and command man pages
################################################################################
#-------------------------------------------------------------------------------
#P G   R I N G   C R E A T I O N
#-------------------------------------------------------------------------------
################################################################################
create_pg_ring_pattern ring_pattern -vertical_layer met4 \
    -horizontal_width {5} -vertical_spacing {4} \
    -horizontal_layer met5 -horizontal_width {5} \
    -horizontal_spacing {4} -corner_bridge false
Warning: duplicate option '-horizontal_width' overrides previous value. (CMD-018)
Successfully create PG ring pattern ring_pattern.
set_pg_strategy core_ring -core -pattern \
    {{pattern: ring_pattern}{nets: {VDD VSS}}{offset: {3 3}}} \
    -extension {{stop: innermost_ring}}
Successfully set PG strategy core_ring.
################################################################################
#-------------------------------------------------------------------------------
# P A D   T O   R I N G   P G   C O N N E C T I O N S
#-------------------------------------------------------------------------------
################################################################################
#create_pg_macro_conn_pattern hm_pattern -pin_conn_type scattered_pin -layer {metal7 metal8}
#create_pg_macro_conn_pattern pad_pattern -pin_conn_type scattered_pin -layer {M7 M6}
#set all_pg_pads [get_cells * -hier -filter "ref_name==VDD_NS || ref_name==VSS_NS"]
#set_pg_strategy s_pad -macros $all_pg_pads  -pattern {{name: pad_pattern} {nets: {VDD VDD_LOW VSS}}}
################################################################################
#-------------------------------------------------------------------------------
# P G   M E S H   C R E A T I O N
#-------------------------------------------------------------------------------
################################################################################
create_pg_mesh_pattern pg_mesh1 \
   -parameters {w1 p1 w2 p2 f t} \
   -layers {{{horizontal_layer: met5} {width: @w1} {spacing: interleaving} \
        {pitch: @p1} {offset: @f} {trim: @t}} \
             {{vertical_layer: met4 } {width: @w2} {spacing: interleaving} \
        {pitch: @p2} {offset: @f} {trim: @t}}}
Successfully create mesh pattern pg_mesh1.
set_pg_strategy s_mesh1 \
   -pattern {{pattern: pg_mesh1} {nets: {VDD VSS VSS VDD}} \
{offset_start: 10 20} {parameters: 4 80 6 120 3.344 false}} \
   -core -extension {{stop: outermost_ring}}
Successfully set PG strategy s_mesh1.
################################################################################
#-------------------------------------------------------------------------------
# M A C R O   P G   C O N N E C T I O N S
#-------------------------------------------------------------------------------
################################################################################
#set toplevel_hms [filter_collection [get_cells * -physical_context] "is_hard_macro == true"]
#set_pg_strategy macro_con -macros $toplevel_hms -pattern {{name: hm_pattern} {nets: {VDD VSS}}}
################################################################################
#-------------------------------------------------------------------------------
# S T A N D A R D    C E L L    R A I L    I N S E R T I O N
#-------------------------------------------------------------------------------
################################################################################
create_pg_std_cell_conn_pattern \
    std_cell_rail  \
    -layers {met1} \
    -rail_width 0.06
Successfully create standard cell rail pattern std_cell_rail.
set_pg_strategy rail_strat -core \
    -pattern {{name: std_cell_rail} {nets: VDD VSS} }
Successfully set PG strategy rail_strat.
RM-info : RUNNING PNS CHARACTERIZATION FLOW because $PNS_CHARACTERIZE_FLOW == true
[icc2-lic Sun Jul 21 02:29:53 2024] Command 'characterize_block_pg' requires licenses
[icc2-lic Sun Jul 21 02:29:53 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:53 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:53 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out of alternate set of keys directly with queueing was successful
Automatic PG net connection through connect_pg_net is disabled.
Characterize block PG constraints.
Load technology.
Updating PG strategies.
Updating strategy core_ring.
Updating strategy s_mesh1.
Updating strategy rail_strat.
Characterize top-level PG constraints.
Characterize PG constraints for top-level design top in script ./block_pg/top_pg.tcl.
Characterize PG constraints for top-level design vsdbabysoc in script ./block_pg/vsdbabysoc_pg.tcl.
Create PG mapping file ./block_pg/pg_mapfile.
[icc2-lic Sun Jul 21 02:29:53 2024] Command 'set_constraint_mapping_file' requires licenses
[icc2-lic Sun Jul 21 02:29:53 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:53 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:53 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out of alternate set of keys directly with queueing was successful
RM-info : Running run_block_compile_pg 
[icc2-lic Sun Jul 21 02:29:53 2024] Command 'run_block_compile_pg' requires licenses
[icc2-lic Sun Jul 21 02:29:53 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:53 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:53 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out of alternate set of keys directly with queueing was successful
Automatic PG net connection through connect_pg_net is disabled.
Create PG for the top-portion design.
[icc2-lic Sun Jul 21 02:29:53 2024] Command 'compile_pg' requires licenses
[icc2-lic Sun Jul 21 02:29:53 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:53 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:53 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out of alternate set of keys directly with queueing was successful
Sanity check for inputs.
No strategy-level via rule is specified, the default rule will be applied.
Automatic PG net connection through connect_pg_net is disabled.
Updating PG strategies.
Updating strategy core_ring.
Loading library and design information.
Number of Standard Cells: 2741
Number of Hard Macros: 2
Number of Pads: 0
Creating rings.
Creating via connection between strategies and existing shapes.
Via DRC checking runtime 0.00 seconds.
via connection runtime: 0 seconds.
Commiting wires and vias.
Committed 8 wires.
Committing wires takes 0.00 seconds.
Committed 8 vias.
Committed 0 wires for via creation.
Committing vias takes 0.00 seconds.
Overall PG creation runtime: 0 seconds.
Successfully compiled PG.
Overall runtime: 0 seconds.
[icc2-lic Sun Jul 21 02:29:53 2024] Command 'compile_pg' requires licenses
[icc2-lic Sun Jul 21 02:29:53 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:53 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:53 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:29:53 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:53 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:53 2024] Check-out of alternate set of keys directly with queueing was successful
Sanity check for inputs.
No strategy-level via rule is specified, the default rule will be applied.
Automatic PG net connection through connect_pg_net is disabled.
Updating PG strategies.
Updating strategy s_mesh1.
Loading library and design information.
Number of Standard Cells: 2741
Number of Hard Macros: 2
Number of Pads: 0
Creating straps and vias in power plan.
Creating wire shapes for strategies s_mesh1 .
Creating wire shapes runtime: 0 seconds
Blockage cutting and DRC fixing for wire shapes for strategies s_mesh1 .
Check and fix DRC for 167 wires for strategy s_mesh1.
Number of threads: 8
Number of partitions: 5
Direction of partitions: vertical
Number of wires: 91
Checking DRC for 91 wires:50% 100%
Number of threads: 8
Number of partitions: 7
Direction of partitions: horizontal
Number of wires: 76
Checking DRC for 76 wires:0% 5% 20% 80% 80% 85% 90% 100%
Creating 167 wires after DRC fixing.
Wire DRC checking runtime 0.00 seconds.
Creating via shapes for strategies s_mesh1 .
Number of threads: 8
Working on strategy s_mesh1.
Number of detected intersections: 1378
Total runtime of via shapes creation: 0 seconds
Check and fix DRC for 1378 stacked vias for strategy s_mesh1.
Number of threads: 8
Number of partitions: 7
Direction of partitions: horizontal
Number of vias: 1378
Checking DRC for 1378 stacked vias:5% 20% 30% 35% 45% 50% 60% 70% 75% 90% 95% 100%
Runtime of via DRC checking for strategy s_mesh1: 1.00 seconds.
Creating via connection between strategies and existing shapes.
Check and fix DRCs for 259 stacked vias.
Number of vias: 154
Checking DRC for 154 stacked vias:0% 5% 10% 15% 20% 25% 30% 35% 40% 45% 50% 55% 60% 65% 70% 75% 80% 85% 90% 95% 100%
2 large vertical vias are not fixed
Number of vias: 105
Checking DRC for 105 stacked vias:0% 5% 10% 15% 20% 25% 30% 35% 40% 45% 50% 55% 60% 65% 70% 75% 80% 85% 90% 95% 100%
3 regular vias are not fixed
Via DRC checking runtime 0.00 seconds.
via connection runtime: 0 seconds.
Commiting wires and vias.
Committed 167 wires.
Committing wires takes 0.00 seconds.
Committed 1632 vias.
Committed 0 wires for via creation.
Committing vias takes 0.00 seconds.
Overall PG creation runtime: 1 seconds.
Successfully compiled PG.
Overall runtime: 1 seconds.
[icc2-lic Sun Jul 21 02:29:54 2024] Command 'compile_pg' requires licenses
[icc2-lic Sun Jul 21 02:29:54 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:29:54 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:54 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:54 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:29:54 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:54 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:54 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:29:54 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:54 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:54 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:54 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:54 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:29:54 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:54 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:54 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:29:54 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:54 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:54 2024] Check-out of alternate set of keys directly with queueing was successful
Sanity check for inputs.
No strategy-level via rule is specified, the default rule will be applied.
Automatic PG net connection through connect_pg_net is disabled.
Updating PG strategies.
Updating strategy rail_strat.
Loading library and design information.
Number of Standard Cells: 2741
Number of Hard Macros: 2
Number of Pads: 0
Creating standard cell rails.
Creating standard cell rails for strategy rail_strat.
User specified width 0.0600 um for net VDD at layer met1 is smaller than the layer min width 0.1400, use min width instead for rail creation.
User specified width 0.0600 um for net VSS at layer met1 is smaller than the layer min width 0.1400, use min width instead for rail creation.
DRC checking and fixing for standard cell rail strategy rail_strat.
Number of threads: 8
Number of partitions: 51
Direction of partitions: horizontal
Number of wires: 774
Checking DRC for 774 wires:100%
Creating 774 wires after DRC fixing.
Wire DRC checking runtime 0.00 seconds.
Creating via connection between strategies and existing shapes.
Check and fix DRCs for 10162 stacked vias.
Number of threads: 8
Number of partitions: 51
Direction of partitions: horizontal
Number of vias: 10162
Checking DRC for 10162 stacked vias:15% 100%
Via DRC checking runtime 1.00 seconds.
via connection runtime: 1 seconds.
Commiting wires and vias.
Committed 774 wires.
Committing wires takes 0.00 seconds.
Committed 30486 vias.
Committed 0 wires for via creation.
Committing vias takes 0.00 seconds.
Overall PG creation runtime: 1 seconds.
Successfully compiled PG.
Overall runtime: 1 seconds.
PG creation runtime for top-portion design: 5 seconds.
Successfully run distributed PG creation.
Checking secondary net through power switch is enabled. 
Secondary net will be checked together from primary net. They will be treated as the same net
Primary Net : VDD    Secondary Net:
Primary Net : VSS    Secondary Net:
Loading cell instances...
Number of Standard Cells: 0
Number of Macro Cells: 2
Number of IO Pad Cells: 0
Number of Blocks: 0
Loading P/G wires and vias...
Number of VDD Wires: 474
Number of VDD Vias: 15695
Number of VDD Terminals: 0
**************Verify net VDD connectivity*****************
  Number of floating wires: 106
  Number of floating vias: 0
  Number of floating std cells: 0
  Number of floating hard macros: 2
  Number of floating I/O pads: 0
  Number of floating terminals: 0
  Number of floating hierarchical blocks: 0
************************************************************
Loading cell instances...
Loading P/G wires and vias...
Number of VSS Wires: 475
Number of VSS Vias: 16431
Number of VSS Terminals: 0
**************Verify net VSS connectivity*****************
  Number of floating wires: 107
  Number of floating vias: 0
  Number of floating std cells: 0
  Number of floating hard macros: 2
  Number of floating I/O pads: 0
  Number of floating terminals: 0
  Number of floating hierarchical blocks: 0
************************************************************
Overall runtime: 0 seconds.
Command check_pg_drc started  at Sun Jul 21 02:29:55 2024
Command check_pg_drc finished at Sun Jul 21 02:29:55 2024
CPU usage for check_pg_drc: 1.18 seconds ( 0.00 hours)
Elapsed time for check_pg_drc: 0.23 seconds ( 0.00 hours)
Total number of errors found: 298
   263 insufficient spacings on mcon
   35 insufficient spacings on met4
------------
Description of the errors can be seen in gui error set "DRC_report_by_check_pg_drc"
------------
Saving all libraries...
[icc2-lic Sun Jul 21 02:29:56 2024] Command 'estimate_timing' requires licenses
[icc2-lic Sun Jul 21 02:29:56 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:29:56 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:56 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:56 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:29:56 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:56 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:56 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:29:56 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:56 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:56 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:56 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:56 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:29:56 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:56 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:56 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:29:56 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:56 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:56 2024] Check-out of alternate set of keys directly with queueing was successful
Information: Starting 'estimate_timing' (FLW-8000)
Information: Time: 2024-07-21 02:29:56 / Session: 0.02 hr / Command: 0.00 hr / Memory: 637 MB (FLW-8100)
Created estimated_corner
Information: The stitching and editing of coupling caps is turned OFF for design 'vsdbabysocsky130_fd_sc_hd:vsdbabysoc/init_dp.design'. (TIM-125)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Information: Design Average RC for design vsdbabysoc  (NEX-011)
Information: r = 0.494087 ohm/um, via_r = 3.543520 ohm/cut, c = 0.155222 ff/um, cc = 0.000000 ff/um (X dir) (NEX-017)
Information: r = 0.761295 ohm/um, via_r = 3.588455 ohm/cut, c = 0.129413 ff/um, cc = 0.000000 ff/um (Y dir) (NEX-017)
Information: The RC mode used is VR for design 'vsdbabysoc'. (NEX-022)
Information: Update timing completed net estimation for all the timing graph nets (TIM-111)
Information: Net estimation statistics: timing graph nets = 2763, routed nets = 0, across physical hierarchy nets = 0, parasitics cached nets = 2763, delay annotated nets = 0, parasitics annotated nets = 0, multi-voltage nets = 19. (TIM-112)
Number of Site types in the design = 1
Setting up Chip Core
Chip Core shape: (50000 50000) (15285200 15282000)
Number of VARs = 1
Number of unique PDs = 1
Number of Power Domains = 1
Number of Voltage Areas = 1
Number of supply Nets = 2
Number of used supplies = 0
Blocked VAs: 
Information: Initializing classic cellmap without advanced rules enabled and without PDC enabled
Information: The following app options are used in cellmap
        place.legalize.enable_color_aware_placement : false
        place.legalize.use_nll_query_cm : false
        place.legalize.enable_advanced_legalizer : false
        place.legalize.enable_prerouted_net_check : true
        place.legalize.enable_advanced_prerouted_net_check : false
        place.legalize.always_continue : true
        place.legalize.limit_legality_checks : false
        place.common.pnet_aware_density : 1.0000
        place.common.pnet_aware_min_width : 0.0000
        place.common.pnet_aware_layers : {}
        place.common.use_placement_model : false
        place.common.enable_advanced_placement_model : true
        cts.placement.cell_spacing_rule_style : maximum
Total 0.0300 seconds to build cellmap data
INFO: creating 50(r) x 50(c) GridCells YDim 30.664 XDim 30.6704
INFO: creating 50(r) x 50(c) GridCells YDim 30.664 XDim 30.6704
Total 0.0600 seconds to load 2741 cell instances into cellmap, 2741 cells are off site row
Moveable cells: 2741; Application fixed cells: 0; Macro cells: 0; User fixed cells: 0
Average cell width 3.3452, cell height 2.7200, cell area 9.0989 for total 2741 placed and application fixed cells
Information: Current block utilization is '0.01520', effective utilization is '0.01522'. (OPT-055)
APSINFO: No multi-Vth libcells found, turning off percentage LVT optimization flow
Drc Mode Option: auto
Warning: Cannot find buffers. Net core/n1164 cannot be buffered. (OPT-019)
APS-VIPO:Using met3 layer for chain buffer model creation
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
APS-VIPO: Total cell number is 2750
estimate timing (5%) done
estimate timing (10%) done
estimate timing (15%) done
estimate timing (20%) done
estimate timing (25%) done
estimate timing (30%) done
estimate timing (35%) done
estimate timing (40%) done
estimate timing (45%) done
estimate timing (50%) done
estimate timing (55%) done
estimate timing (60%) done
estimate timing (65%) done
estimate timing (70%) done
estimate timing (75%) done
estimate timing (80%) done
estimate timing (85%) done
estimate timing (90%) done
estimate timing (95%) done
estimate timing (100%) done
In the step 1, 1540 cells are processed
In the step 2, 2750 cells are processed
No estimate_timing rule detected on nets.
Virtually optimized 827 out of 2750 cells
Virtually buffered 827 cell outputs
Back-annotation statistics for mode 'func1'
  4948 back-annotations generated.
  973 have been transition time back annotations.
  0 have been skipped.
  Back-annotation succeeded on all arcs
Information: Elapsed time for estimate_timing excluding pending time: 00:00:02.27. (DPUI-902)
Information: CPU time for estimate_timing : 00:00:04.37. (DPUI-903)
Information: Peak memory usage for estimate_timing : 642 MB. (DPUI-904)
Information: Ending 'estimate_timing' (FLW-8001)
Information: Time: 2024-07-21 02:29:58 / Session: 0.02 hr / Command: 0.00 hr / Memory: 643 MB (FLW-8100)
Saving all libraries...
Information: The net parasitics of block vsdbabysoc are cleared. (TIM-123)
[icc2-lic Sun Jul 21 02:29:58 2024] Command 'set_constraint_mapping_file' requires licenses
[icc2-lic Sun Jul 21 02:29:58 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:29:58 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:58 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:58 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:29:58 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:58 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:58 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:29:58 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:58 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:58 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:58 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:58 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:29:58 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:58 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:58 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:29:58 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:58 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:58 2024] Check-out of alternate set of keys directly with queueing was successful
Constraint mapping file data for design vsdbabysoc/init_dp is reset.
Information: Explicit supply net connections to isolation and retention cells will be written out. Use mv.upf.save_upf_include_supply_exceptions_for_iso_retn to control this. (UPF-450)
Information: Explicit supply net connections to isolation and retention cells will be written out. Use mv.upf.save_upf_include_supply_exceptions_for_iso_retn to control this. (UPF-450)
****************************************
Report : Reporting Mismatches
Version: T-2022.03-SP5
Date   : Sun Jul 21 02:29:58 2024
****************************************

==============================
Library : EXPLORE_macros
==============================
Mismatch Type                        Total Count   Repaired  Unrepaired
--------------------------------------------------------------------------------
lib_missing_physical_port            3             3         0
****************************************
Report : Reporting Mismatches
Version: T-2022.03-SP5
Date   : Sun Jul 21 02:29:58 2024
****************************************

==============================
Library : EXPLORE_macros
==============================
Mismatch Type                        Total Count   Repaired  Unrepaired
--------------------------------------------------------------------------------
lib_missing_physical_port            3             3         0
****************************************
Report : Reporting Mismatches
Version: T-2022.03-SP5
Date   : Sun Jul 21 02:29:58 2024
****************************************

==============================
Library : EXPLORE_macros
==============================
Mismatch Type                        Total Count   Repaired  Unrepaired
--------------------------------------------------------------------------------
lib_missing_physical_port            3             3         0
****************************************
Report : Reporting Mismatches
Version: T-2022.03-SP5
Date   : Sun Jul 21 02:29:59 2024
****************************************

==============================
Library : EXPLORE_macros
==============================
Mismatch Type                        Total Count   Repaired  Unrepaired
--------------------------------------------------------------------------------
lib_missing_physical_port            3             3         0
Information: The command 'write_lef' cleared the undo history. (UNDO-016)
Warning: Block 'vsdbabysoc' has no frame view with empty label, will ignore it. (LEFW-005)
Warning: Block 'floorplan_in_progress' has no frame view with empty label, will ignore it. (LEFW-005)
Warning: Use defaultWidth 0.17 as the WRONGDIRECTION width for layer 'li1'. (LEFW-007)
Warning: Use defaultWidth 0.14 as the WRONGDIRECTION width for layer 'met1'. (LEFW-007)
Warning: Use defaultWidth 0.14 as the WRONGDIRECTION width for layer 'met2'. (LEFW-007)
Warning: Use defaultWidth 0.3 as the WRONGDIRECTION width for layer 'met3'. (LEFW-007)
Warning: Use defaultWidth 0.3 as the WRONGDIRECTION width for layer 'met4'. (LEFW-007)
Warning: Use defaultWidth 1.6 as the WRONGDIRECTION width for layer 'met5'. (LEFW-007)
[icc2-lic Sun Jul 21 02:29:59 2024] Command 'write_floorplan' requires licenses
[icc2-lic Sun Jul 21 02:29:59 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:29:59 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:59 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:59 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:29:59 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:59 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:59 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:29:59 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:59 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:59 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:59 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:59 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:29:59 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:59 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:59 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:29:59 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:59 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:59 2024] Check-out of alternate set of keys directly with queueing was successful
Writing floorplan for vsdbabysoc.design

Warning: Multiply cell derates are skipped!

Warning: Multiply net derates are skipped!
[icc2-lic Sun Jul 21 02:29:59 2024] Command 'write_floorplan' requires licenses
[icc2-lic Sun Jul 21 02:29:59 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:29:59 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:59 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:59 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:29:59 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:59 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:59 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:29:59 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:59 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:59 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:59 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:59 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:29:59 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:59 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:59 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:29:59 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:59 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:59 2024] Check-out of alternate set of keys directly with queueing was successful
Writing floorplan for vsdbabysoc.design
Warning: Found placement blockage auto_generate_blockage0 with purpose information. This information will not be transfered to IC Compiler. (DPWF-201)
Warning: Found placement blockage auto_generate_blockage1 with purpose information. This information will not be transfered to IC Compiler. (DPWF-201)
Warning: Found placement blockage auto_generate_blockage2 with purpose information. This information will not be transfered to IC Compiler. (DPWF-201)
Saving all libraries...
Warning: No corner objects matched 'estimated_corner' (SEL-004)
Error: Value for list 'corner_list' must have more than zero elements. (CMD-036)
[icc2-lic Sun Jul 21 02:29:59 2024] Command 'place_opt' requires licenses
[icc2-lic Sun Jul 21 02:29:59 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:29:59 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:59 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:59 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:29:59 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:59 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:59 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:29:59 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:29:59 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:29:59 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:59 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:59 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:29:59 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:29:59 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:59 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:29:59 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:29:59 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:29:59 2024] Check-out of alternate set of keys directly with queueing was successful
Information: Starting 'place_opt' (FLW-8000)
Information: Time: 2024-07-21 02:29:59 / Session: 0.02 hr / Command: 0.00 hr / Memory: 651 MB (FLW-8100)
INFO: place_opt is running in balanced flow mode
Warning: No default buffer/inverter available for voltage areas DEFAULT_VA. (OPT-014)
Warning: Can not find available buffer/inverter lib cells for the site unit. (OPT-016)
INFO: Total Power Aware Optimization Enabled (Dynamic + Leakage)
INFO: disable CRPR-based timing. 
Information: The stitching and editing of coupling caps is turned OFF for design 'vsdbabysocsky130_fd_sc_hd:vsdbabysoc/init_dp.design'. (TIM-125)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Information: Design Average RC for design vsdbabysoc  (NEX-011)
Information: r = 0.494087 ohm/um, via_r = 3.543520 ohm/cut, c = 0.155222 ff/um, cc = 0.000000 ff/um (X dir) (NEX-017)
Information: r = 0.761295 ohm/um, via_r = 3.588455 ohm/cut, c = 0.129413 ff/um, cc = 0.000000 ff/um (Y dir) (NEX-017)
Number of Site types in the design = 1
Setting up Chip Core
Chip Core shape: (50000 50000) (15285200 15282000)
Number of VARs = 1
Number of unique PDs = 1
Number of Power Domains = 1
Number of Voltage Areas = 1
Number of supply Nets = 2
Number of used supplies = 0
Blocked VAs: 
Warning: Cannot find default buffer/inverter for VA DEFAULT_VA with Block Hierarchy . (OPT-043)
Error: Cannot find usable buffers or inverters. (OPT-045)
INFO: Dynamic Scenario ASR Mode:  0
INFO: Running power improvement flow (1)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Warning: No clock routing rules were specified. Use set_clock_routing_rules to specify routing rules for accurate clock latency estimation. (OPT-902)
Information: Estimating clock gate latencies for scenario 'func1'. (OPT-909)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)

Information: Starting place_opt / initial_place (FLW-8000)
Information: Time: 2024-07-21 02:30:00 / Session: 0.02 hr / Command: 0.00 hr / Memory: 651 MB (FLW-8100)


Information: Starting place_opt / initial_place / Initial Placement (FLW-8000)
Information: Time: 2024-07-21 02:30:00 / Session: 0.02 hr / Command: 0.00 hr / Memory: 651 MB (FLW-8100)
Information: The RC mode used is VR for design 'vsdbabysoc'. (NEX-022)
Information: Update timing completed net estimation for all the timing graph nets (TIM-111)
Information: Net estimation statistics: timing graph nets = 2763, routed nets = 0, across physical hierarchy nets = 0, parasitics cached nets = 2763, delay annotated nets = 0, parasitics annotated nets = 0, multi-voltage nets = 19. (TIM-112)
Information: Ending place_opt / initial_place / Initial Placement (FLW-8001)
Information: Time: 2024-07-21 02:30:00 / Session: 0.02 hr / Command: 0.00 hr / Memory: 651 MB (FLW-8100)
Information: Running auto PG connection. (NDM-099)
Warning: Incomplete place_opt / initial_place (FLW-8004)
Information: Ending 'place_opt' (FLW-8001)
Information: Time: 2024-07-21 02:30:00 / Session: 0.02 hr / Command: 0.00 hr / Memory: 651 MB (FLW-8100)
Error: 0
        Use error_info for more info. (CMD-013)
Saving all libraries...
[icc2-lic Sun Jul 21 02:30:01 2024] Command 'clock_opt' requires licenses
[icc2-lic Sun Jul 21 02:30:01 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:30:01 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:30:01 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:30:01 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:30:01 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:30:01 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:30:01 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:30:01 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:30:01 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:30:01 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:30:01 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:30:01 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:30:01 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:30:01 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:30:01 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:30:01 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:30:01 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:30:01 2024] Check-out of alternate set of keys directly with queueing was successful
Information: Starting 'clock_opt' (FLW-8000)
Information: Time: 2024-07-21 02:30:01 / Session: 0.02 hr / Command: 0.00 hr / Memory: 651 MB (FLW-8100)
INFO: clock_opt is running in balanced flow mode.
Warning: No default buffer/inverter available for voltage areas DEFAULT_VA. (OPT-014)
Warning: Can not find available buffer/inverter lib cells for the site unit. (OPT-016)
INFO: Total Power Aware Optimization Enabled (Dynamic + Leakage)
Number of Site types in the design = 1
Setting up Chip Core
Chip Core shape: (50000 50000) (15285200 15282000)
Number of VARs = 1
Number of unique PDs = 1
Number of Power Domains = 1
Number of Voltage Areas = 1
Number of supply Nets = 2
Number of used supplies = 0
Blocked VAs: 
Warning: Cannot find default buffer/inverter for VA DEFAULT_VA with Block Hierarchy . (OPT-043)
Error: Cannot find usable buffers or inverters. (OPT-045)

Information: Starting clock_opt / build_clock (FLW-8000)
Information: Time: 2024-07-21 02:30:01 / Session: 0.02 hr / Command: 0.00 hr / Memory: 651 MB (FLW-8100)

Information: Starting clock_opt / build_clock / Trial CTS (FLW-8000)
Information: Time: 2024-07-21 02:30:01 / Session: 0.02 hr / Command: 0.00 hr / Memory: 651 MB (FLW-8100)
Information: Ending clock_opt / build_clock / Trial CTS (FLW-8001)
Information: Time: 2024-07-21 02:30:01 / Session: 0.02 hr / Command: 0.00 hr / Memory: 651 MB (FLW-8100)
Information: Running auto PG connection. (NDM-099)
Warning: Incomplete clock_opt / build_clock (FLW-8004)
Information: Ending 'clock_opt' (FLW-8001)
Information: Time: 2024-07-21 02:30:02 / Session: 0.02 hr / Command: 0.00 hr / Memory: 651 MB (FLW-8100)
Error: 0
        Use error_info for more info. (CMD-013)
Saving all libraries...
[icc2-lic Sun Jul 21 02:30:02 2024] Command 'route_auto' requires licenses
[icc2-lic Sun Jul 21 02:30:02 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:30:02 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:30:02 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:30:02 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:30:02 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:30:02 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:30:02 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:30:02 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:30:02 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:30:02 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:30:02 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:30:02 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:30:02 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:30:02 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:30:02 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:30:02 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:30:02 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:30:02 2024] Check-out of alternate set of keys directly with queueing was successful
Information: Starting 'route_auto' (FLW-8000)
Information: Time: 2024-07-21 02:30:02 / Session: 0.02 hr / Command: 0.00 hr / Memory: 651 MB (FLW-8100)
Generating Timing information  
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Design  Scenario func1 (Mode func1 Corner func1)
Generating Timing information  ... Done
Information: The net parasitics of block vsdbabysoc are cleared. (TIM-123)
[End of Generating Timing Information] Elapsed real time: 0:00:01 
[End of Generating Timing Information] Elapsed cpu  time: sys=0:00:00 usr=0:00:01 total=0:00:02
Warning: Layer li1 does not have a preferred direction, assigning to vertical. (ZRT-025)
Cell Min-Routing-Layer = met1
Cell Max-Routing-Layer = met5
Turn off antenna since no rule is specified
Information: Option route.detail.force_end_on_preferred_grid will be ignored since none of the layers have preferred grid. (ZRT-703)
Warning: Ignore pin (cell avsdpll, pin VDD) on layer nwell. (ZRT-030)
Warning: Ignore pin (cell avsdpll, pin VDD#2_1) on layer nwell. (ZRT-030)
Warning: Ignore pin (cell avsdpll, pin VDD#2) on layer nwell. (ZRT-030)
Warning: Ignore pin (cell avsdpll, pin VDD#3) on layer nwell. (ZRT-030)
Warning: Ignore 2 top cell ports with no pins. (ZRT-027)
Info: number of net_type_blockage: 0 
Information: Via ladder engine would be activated for pattern must join connection in certain commands. Please refer to man-page for the command list. (ZRT-619)
Via on layer (via4) needs more than one tracks
Warning: Layer met4 pitch 0.920 may be too small: wire/via-down 0.615, wire/via-up 1.040. (ZRT-026)
Transition layer name: met4(4)
Information: When applicable Min-max layer allow_pin_connection mode will allow paths of length 5.75 outside the layer range. (ZRT-707)
Information: When applicable Min-max layer allow_pin_connection mode will allow paths of length 5.75 outside the layer range on clock nets. (ZRT-718)
Information: When applicable layer based tapering will taper up to 0.00 in distance from the pin. (ZRT-706)
Warning: Master cell avsddac has duplicated redundant library pin shapes at {0.000 0.000} {0.170 0.170} on layer li1. (ZRT-625)
Warning: Master cell avsddac has duplicated redundant library pin shapes at {0.000 0.000} {0.170 0.170} on layer li1. (ZRT-625)
Start Global Route ... 
[Init] Elapsed real time: 0:00:00 
[Init] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[Init] Stage (MB): Used    0  Alloctr    0  Proc    0 
[Init] Total (MB): Used   22  Alloctr   23  Proc 6260 
Info: route.global.delay_based_route_rejection is 16.
Printing options for 'route.common.*'

Printing options for 'route.global.*'
global.timing_driven                                    :        true                

Begin global routing.
Loading timing information to the router from design
Timing information loaded to the router.
[End of Loading Timing] Elapsed real time: 0:00:00 
[End of Loading Timing] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
Constructing data structure ...
Design statistics:
Design Bounding Box (0.00um,0.00um,1533.52um,1533.20um)
Number of routing layers = 6
layer li1, dir Ver, min width = 0.17um, min space = 0.17um pitch = 0.46um
layer met1, dir Hor, min width = 0.14um, min space = 0.14um pitch = 0.34um
layer met2, dir Ver, min width = 0.14um, min space = 0.14um pitch = 0.46um
layer met3, dir Hor, min width = 0.3um, min space = 0.3um pitch = 0.68um
layer met4, dir Ver, min width = 0.3um, min space = 0.3um pitch = 0.92um
layer met5, dir Hor, min width = 1.6um, min space = 1.6um pitch = 3.4um
Current Stage stats:
[End of Build Tech Data] Elapsed real time: 0:00:00 
[End of Build Tech Data] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[End of Build Tech Data] Stage (MB): Used    4  Alloctr    4  Proc    0 
[End of Build Tech Data] Total (MB): Used   30  Alloctr   31  Proc 6260 
Net statistics:
Total number of nets     = 2765
Number of nets to route  = 2740
25 nets are fully connected,
 of which 25 are detail routed and 0 are global routed.
Net length statistics: 
Net Count(Ignore Fully Rted) 2740, Total Half Perimeter Wire Length (HPWL) 92619 microns
HPWL   0 ~   50 microns: Net Count     2472     Total HPWL        42912 microns
HPWL  50 ~  100 microns: Net Count      135     Total HPWL         9279 microns
HPWL 100 ~  200 microns: Net Count       54     Total HPWL         7738 microns
HPWL 200 ~  300 microns: Net Count       55     Total HPWL        14103 microns
HPWL 300 ~  400 microns: Net Count       12     Total HPWL         3878 microns
HPWL 400 ~  500 microns: Net Count        0     Total HPWL            0 microns
HPWL 500 ~  600 microns: Net Count        1     Total HPWL          525 microns
HPWL 600 ~  700 microns: Net Count        0     Total HPWL            0 microns
HPWL 700 ~  800 microns: Net Count        0     Total HPWL            0 microns
HPWL 800 ~  900 microns: Net Count        3     Total HPWL         2585 microns
HPWL 900 ~ 1000 microns: Net Count        2     Total HPWL         1879 microns
HPWL     > 1000 microns: Net Count        6     Total HPWL         9720 microns
Current Stage stats:
[End of Build All Nets] Elapsed real time: 0:00:00 
[End of Build All Nets] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[End of Build All Nets] Stage (MB): Used    2  Alloctr    2  Proc    0 
[End of Build All Nets] Total (MB): Used   32  Alloctr   34  Proc 6260 
Number of partitions: 1 (1 x 1)
Size of partitions: 564 gCells x 564 gCells
Average gCell capacity  4.14     on layer (1)    li1
Average gCell capacity  4.93     on layer (2)    met1
Average gCell capacity  4.07     on layer (3)    met2
Average gCell capacity  2.75     on layer (4)    met3
Average gCell capacity  1.60     on layer (5)    met4
Average gCell capacity  0.32     on layer (6)    met5
Average number of tracks per gCell 5.91  on layer (1)    li1
Average number of tracks per gCell 8.00  on layer (2)    met1
Average number of tracks per gCell 5.91  on layer (3)    met2
Average number of tracks per gCell 4.00  on layer (4)    met3
Average number of tracks per gCell 2.96  on layer (5)    met4
Average number of tracks per gCell 0.80  on layer (6)    met5
Number of gCells = 1908576
Current Stage stats:
[End of Build Congestion Map] Elapsed real time: 0:00:01 
[End of Build Congestion Map] Elapsed cpu  time: sys=0:00:00 usr=0:00:01 total=0:00:01
[End of Build Congestion Map] Stage (MB): Used   27  Alloctr   27  Proc    0 
[End of Build Congestion Map] Total (MB): Used   60  Alloctr   62  Proc 6260 
Number of partitions: 1 (1 x 1)
Size of partitions: 564 gCells x 564 gCells
Current Stage stats:
[End of Add Nets Demand] Elapsed real time: 0:00:00 
[End of Add Nets Demand] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[End of Add Nets Demand] Stage (MB): Used    0  Alloctr    0  Proc    0 
[End of Add Nets Demand] Total (MB): Used   60  Alloctr   62  Proc 6260 
Number of user frozen nets = 0
Timing criticality report: total 274 (9.91)% critical nets.
   Number of criticality 1 nets = 29 (1.05)%
   Number of criticality 2 nets = 10 (0.36)%
   Number of criticality 3 nets = 29 (1.05)%
   Number of criticality 4 nets = 31 (1.12)%
   Number of criticality 5 nets = 25 (0.90)%
   Number of criticality 6 nets = 18 (0.65)%
   Number of criticality 7 nets = 132 (4.77)%
Information: RC layer preference is turned on for this design. (ZRT-613)
Total stats:
[End of Build Data] Elapsed real time: 0:00:02 
[End of Build Data] Elapsed cpu  time: sys=0:00:00 usr=0:00:02 total=0:00:02
[End of Build Data] Stage (MB): Used   34  Alloctr   35  Proc    0 
[End of Build Data] Total (MB): Used   61  Alloctr   62  Proc 6260 
Number of partitions: 1 (1 x 1)
Size of partitions: 564 gCells x 564 gCells
Current Stage stats:
[End of Blocked Pin Detection] Elapsed real time: 0:00:00 
[End of Blocked Pin Detection] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[End of Blocked Pin Detection] Stage (MB): Used  176  Alloctr  176  Proc  160 
[End of Blocked Pin Detection] Total (MB): Used  237  Alloctr  239  Proc 6421 
Information: Using 8 threads for routing. (ZRT-444)
Information: Multiple gcell levels ON
Information: Buffer distance is estimated to be ~4656.0000um (1711 gCells)

Start GR phase 0
Number of partitions: 1 (1 x 1)
Size of partitions: 564 gCells x 564 gCells
[rtAllBotParts] Elapsed real time: 0:00:02 
[rtAllBotParts] Elapsed cpu  time: sys=0:00:00 usr=0:00:02 total=0:00:02
[rtTop] Elapsed real time: 0:00:00 
[rtTop] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
Current Stage stats:
[End of Initial Routing] Elapsed real time: 0:00:02 
[End of Initial Routing] Elapsed cpu  time: sys=0:00:00 usr=0:00:02 total=0:00:02
[End of Initial Routing] Stage (MB): Used    2  Alloctr    2  Proc    0 
[End of Initial Routing] Total (MB): Used  239  Alloctr  241  Proc 6421 
Initial. Routing result:
Initial. Both Dirs: Dmd-Cap  =  2572 Max = 8 GRCs =  1279 (0.20%)
Initial. H routing: Dmd-Cap  =  2190 Max = 8 (GRCs =  1) GRCs =  1073 (0.34%)
Initial. V routing: Dmd-Cap  =   382 Max = 7 (GRCs =  1) GRCs =   206 (0.06%)
Initial. Both Dirs: Overflow =  3192 Max = 7 GRCs =  2961 (0.47%)
Initial. H routing: Overflow =  2295 Max = 6 (GRCs =  6) GRCs =  2078 (0.65%)
Initial. V routing: Overflow =   897 Max = 7 (GRCs =  3) GRCs =   883 (0.28%)
Initial. li1        Overflow =     0 Max = 0 (GRCs =  0) GRCs =     0 (0.00%)
Initial. met1       Overflow =  1446 Max = 6 (GRCs =  6) GRCs =  1170 (0.37%)
Initial. met2       Overflow =   807 Max = 7 (GRCs =  3) GRCs =   821 (0.26%)
Initial. met3       Overflow =   287 Max = 4 (GRCs =  1) GRCs =   460 (0.14%)
Initial. met4       Overflow =    90 Max = 4 (GRCs =  2) GRCs =    62 (0.02%)
Initial. met5       Overflow =   561 Max = 3 (GRCs =  4) GRCs =   448 (0.14%)


Overflow over macro areas

Initial. Both Dirs: Overflow =     1 Max =  1 GRCs =     7 (0.00%)
Initial. H routing: Overflow =     1 Max =  1 (GRCs =  7) GRCs =     7 (0.01%)
Initial. V routing: Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
Initial. li1        Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
Initial. met1       Overflow =     1 Max =  1 (GRCs =  7) GRCs =     7 (0.01%)
Initial. met2       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
Initial. met3       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
Initial. met4       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
Initial. met5       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)


Density distribution:
Layer    0.0  0.1  0.2  0.3  0.4  0.5  0.6  0.7  0.8  0.9  1.0  1.1  1.2  > 1.2 
li1      99.7 0.18 0.08 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00
met1     97.8 0.37 0.11 0.21 0.00 0.44 0.19 0.08 0.05 0.00 0.47 0.00 0.06 0.19
met2     96.7 1.15 0.09 0.52 0.00 0.33 0.35 0.02 0.36 0.00 0.27 0.09 0.03 0.07
met3     96.6 0.00 1.38 0.22 0.00 0.51 0.12 0.54 0.00 0.00 0.44 0.00 0.06 0.04
met4     98.6 0.00 0.00 0.87 0.00 0.10 0.24 0.00 0.00 0.00 0.10 0.00 0.00 0.02
met5     99.2 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.58 0.00 0.00 0.17
Total    98.1 0.28 0.26 0.30 0.00 0.22 0.15 0.10 0.07 0.00 0.31 0.02 0.02 0.08


Initial. Total Wire Length = 107132.06
Initial. Layer li1 wire length = 16.68
Initial. Layer met1 wire length = 12990.36
Initial. Layer met2 wire length = 44293.90
Initial. Layer met3 wire length = 33532.47
Initial. Layer met4 wire length = 9682.39
Initial. Layer met5 wire length = 6616.25
Initial. Total Number of Contacts = 26348
Initial. Via L1M1_PR count = 10011
Initial. Via M1M2_PR count = 9294
Initial. Via M2M3_PR count = 5759
Initial. Via M3M4_PR count = 927
Initial. Via M4M5_PR count = 357
Initial. completed.

Start GR phase 1
Sun Jul 21 02:30:09 2024
Number of partitions: 1 (1 x 1)
Size of partitions: 564 gCells x 564 gCells
[rtAllParts] Elapsed real time: 0:00:02 
[rtAllParts] Elapsed cpu  time: sys=0:00:00 usr=0:00:02 total=0:00:02
Current Stage stats:
[End of Phase1 Routing] Elapsed real time: 0:00:02 
[End of Phase1 Routing] Elapsed cpu  time: sys=0:00:00 usr=0:00:02 total=0:00:02
[End of Phase1 Routing] Stage (MB): Used    0  Alloctr    1  Proc    0 
[End of Phase1 Routing] Total (MB): Used  239  Alloctr  242  Proc 6421 
phase1. Routing result:
phase1. Both Dirs: Dmd-Cap  =  2281 Max = 8 GRCs =  1251 (0.20%)
phase1. H routing: Dmd-Cap  =  2075 Max = 8 (GRCs =  1) GRCs =  1109 (0.35%)
phase1. V routing: Dmd-Cap  =   206 Max = 4 (GRCs =  2) GRCs =   142 (0.04%)
phase1. Both Dirs: Overflow =  2179 Max = 8 GRCs =  2511 (0.39%)
phase1. H routing: Overflow =  1840 Max = 8 (GRCs =  1) GRCs =  1898 (0.60%)
phase1. V routing: Overflow =   338 Max = 4 (GRCs =  4) GRCs =   613 (0.19%)
phase1. li1        Overflow =     0 Max = 0 (GRCs =  0) GRCs =     0 (0.00%)
phase1. met1       Overflow =  1295 Max = 8 (GRCs =  1) GRCs =  1100 (0.35%)
phase1. met2       Overflow =   252 Max = 4 (GRCs =  4) GRCs =   546 (0.17%)
phase1. met3       Overflow =   183 Max = 4 (GRCs =  1) GRCs =   461 (0.14%)
phase1. met4       Overflow =    86 Max = 3 (GRCs =  2) GRCs =    67 (0.02%)
phase1. met5       Overflow =   362 Max = 2 (GRCs = 25) GRCs =   337 (0.11%)


Overflow over macro areas

phase1. Both Dirs: Overflow =     1 Max =  1 GRCs =     7 (0.00%)
phase1. H routing: Overflow =     1 Max =  1 (GRCs =  7) GRCs =     7 (0.01%)
phase1. V routing: Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase1. li1        Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase1. met1       Overflow =     1 Max =  1 (GRCs =  7) GRCs =     7 (0.01%)
phase1. met2       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase1. met3       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase1. met4       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase1. met5       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)


Density distribution:
Layer    0.0  0.1  0.2  0.3  0.4  0.5  0.6  0.7  0.8  0.9  1.0  1.1  1.2  > 1.2 
li1      99.7 0.18 0.08 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00
met1     97.6 0.37 0.11 0.20 0.00 0.29 0.20 0.09 0.06 0.00 0.78 0.00 0.05 0.17
met2     96.6 1.14 0.09 0.50 0.00 0.43 0.38 0.02 0.46 0.00 0.31 0.04 0.01 0.01
met3     96.6 0.00 1.33 0.21 0.00 0.46 0.11 0.55 0.00 0.00 0.64 0.00 0.05 0.02
met4     98.5 0.00 0.00 0.76 0.00 0.13 0.34 0.00 0.00 0.00 0.24 0.00 0.00 0.03
met5     99.2 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.65 0.00 0.00 0.13
Total    98.1 0.28 0.25 0.28 0.00 0.21 0.17 0.10 0.09 0.00 0.43 0.01 0.02 0.06


phase1. Total Wire Length = 109534.09
phase1. Layer li1 wire length = 15.08
phase1. Layer met1 wire length = 12726.31
phase1. Layer met2 wire length = 43256.04
phase1. Layer met3 wire length = 35025.99
phase1. Layer met4 wire length = 12210.55
phase1. Layer met5 wire length = 6300.11
phase1. Total Number of Contacts = 27511
phase1. Via L1M1_PR count = 9972
phase1. Via M1M2_PR count = 9429
phase1. Via M2M3_PR count = 6218
phase1. Via M3M4_PR count = 1546
phase1. Via M4M5_PR count = 346
phase1. completed.

Start GR phase 2
Sun Jul 21 02:30:12 2024
Number of partitions: 1 (1 x 1)
Size of partitions: 564 gCells x 564 gCells
[rtAllParts] Elapsed real time: 0:00:02 
[rtAllParts] Elapsed cpu  time: sys=0:00:00 usr=0:00:03 total=0:00:03
Number of partitions: 1 (1 x 1)
Size of partitions: 564 gCells x 564 gCells
[updNetsDmd] Elapsed real time: 0:00:00 
[updNetsDmd] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
Current Stage stats:
[End of Phase2 Routing] Elapsed real time: 0:00:02 
[End of Phase2 Routing] Elapsed cpu  time: sys=0:00:00 usr=0:00:03 total=0:00:03
[End of Phase2 Routing] Stage (MB): Used    0  Alloctr    0  Proc    0 
[End of Phase2 Routing] Total (MB): Used  239  Alloctr  243  Proc 6421 
phase2. Routing result:
phase2. Both Dirs: Dmd-Cap  =   921 Max = 5 GRCs =   684 (0.11%)
phase2. H routing: Dmd-Cap  =   842 Max = 5 (GRCs =  1) GRCs =   619 (0.19%)
phase2. V routing: Dmd-Cap  =    79 Max = 3 (GRCs =  1) GRCs =    65 (0.02%)
phase2. Both Dirs: Overflow =  1140 Max = 5 GRCs =  1200 (0.19%)
phase2. H routing: Overflow =   904 Max = 5 (GRCs =  1) GRCs =   989 (0.31%)
phase2. V routing: Overflow =   236 Max = 2 (GRCs = 25) GRCs =   211 (0.07%)
phase2. li1        Overflow =     0 Max = 0 (GRCs =  0) GRCs =     0 (0.00%)
phase2. met1       Overflow =   664 Max = 5 (GRCs =  1) GRCs =   757 (0.24%)
phase2. met2       Overflow =   206 Max = 2 (GRCs = 23) GRCs =   183 (0.06%)
phase2. met3       Overflow =   233 Max = 2 (GRCs =  8) GRCs =   225 (0.07%)
phase2. met4       Overflow =    30 Max = 2 (GRCs =  2) GRCs =    28 (0.01%)
phase2. met5       Overflow =     7 Max = 1 (GRCs =  7) GRCs =     7 (0.00%)


Overflow over macro areas

phase2. Both Dirs: Overflow =     0 Max =  0 GRCs =     0 (0.00%)
phase2. H routing: Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase2. V routing: Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase2. li1        Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase2. met1       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase2. met2       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase2. met3       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase2. met4       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase2. met5       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)


Density distribution:
Layer    0.0  0.1  0.2  0.3  0.4  0.5  0.6  0.7  0.8  0.9  1.0  1.1  1.2  > 1.2 
li1      100. 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00
met1     97.9 0.20 0.10 0.20 0.00 0.41 0.22 0.11 0.07 0.00 0.58 0.00 0.02 0.12
met2     96.8 0.96 0.01 0.49 0.00 0.42 0.39 0.02 0.44 0.00 0.35 0.05 0.02 0.01
met3     96.7 0.00 1.18 0.21 0.00 0.40 0.11 0.51 0.00 0.00 0.74 0.00 0.08 0.02
met4     98.4 0.00 0.00 0.71 0.00 0.14 0.40 0.00 0.00 0.00 0.30 0.00 0.00 0.01
met5     99.1 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.81 0.00 0.00 0.00
Total    98.2 0.19 0.20 0.26 0.00 0.22 0.19 0.10 0.08 0.00 0.47 0.01 0.02 0.03


phase2. Total Wire Length = 111464.65
phase2. Layer li1 wire length = 9.24
phase2. Layer met1 wire length = 12323.59
phase2. Layer met2 wire length = 43843.80
phase2. Layer met3 wire length = 36192.53
phase2. Layer met4 wire length = 13408.16
phase2. Layer met5 wire length = 5687.34
phase2. Total Number of Contacts = 28121
phase2. Via L1M1_PR count = 9965
phase2. Via M1M2_PR count = 9499
phase2. Via M2M3_PR count = 6608
phase2. Via M3M4_PR count = 1749
phase2. Via M4M5_PR count = 300
phase2. completed.

Start GR phase 3
Sun Jul 21 02:30:15 2024
Number of partitions: 1 (1 x 1)
Size of partitions: 564 gCells x 564 gCells
[rtAllParts] Elapsed real time: 0:00:02 
[rtAllParts] Elapsed cpu  time: sys=0:00:00 usr=0:00:02 total=0:00:02
Number of partitions: 1 (1 x 1)
Size of partitions: 564 gCells x 564 gCells
[updNetsDmd] Elapsed real time: 0:00:00 
[updNetsDmd] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
Current Stage stats:
[End of Phase3 Routing] Elapsed real time: 0:00:02 
[End of Phase3 Routing] Elapsed cpu  time: sys=0:00:00 usr=0:00:02 total=0:00:02
[End of Phase3 Routing] Stage (MB): Used    0  Alloctr    0  Proc    0 
[End of Phase3 Routing] Total (MB): Used  239  Alloctr  243  Proc 6421 
phase3. Routing result:
phase3. Both Dirs: Dmd-Cap  =   800 Max = 4 GRCs =   626 (0.10%)
phase3. H routing: Dmd-Cap  =   751 Max = 4 (GRCs =  3) GRCs =   580 (0.18%)
phase3. V routing: Dmd-Cap  =    49 Max = 2 (GRCs =  3) GRCs =    46 (0.01%)
phase3. Both Dirs: Overflow =   893 Max = 5 GRCs =  1005 (0.16%)
phase3. H routing: Overflow =   726 Max = 5 (GRCs =  1) GRCs =   843 (0.27%)
phase3. V routing: Overflow =   167 Max = 2 (GRCs =  5) GRCs =   162 (0.05%)
phase3. li1        Overflow =     0 Max = 0 (GRCs =  0) GRCs =     0 (0.00%)
phase3. met1       Overflow =   589 Max = 5 (GRCs =  1) GRCs =   707 (0.22%)
phase3. met2       Overflow =   160 Max = 2 (GRCs =  5) GRCs =   155 (0.05%)
phase3. met3       Overflow =   135 Max = 2 (GRCs =  1) GRCs =   134 (0.04%)
phase3. met4       Overflow =     7 Max = 1 (GRCs =  7) GRCs =     7 (0.00%)
phase3. met5       Overflow =     2 Max = 1 (GRCs =  2) GRCs =     2 (0.00%)


Overflow over macro areas

phase3. Both Dirs: Overflow =     0 Max =  0 GRCs =     0 (0.00%)
phase3. H routing: Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase3. V routing: Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase3. li1        Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase3. met1       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase3. met2       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase3. met3       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase3. met4       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase3. met5       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)


Density distribution:
Layer    0.0  0.1  0.2  0.3  0.4  0.5  0.6  0.7  0.8  0.9  1.0  1.1  1.2  > 1.2 
li1      99.9 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00
met1     97.9 0.20 0.10 0.20 0.00 0.42 0.22 0.11 0.07 0.00 0.62 0.00 0.02 0.10
met2     96.8 0.97 0.01 0.48 0.00 0.40 0.37 0.02 0.41 0.00 0.44 0.05 0.01 0.00
met3     96.7 0.00 1.17 0.20 0.00 0.39 0.10 0.47 0.00 0.00 0.88 0.00 0.05 0.01
met4     98.3 0.00 0.00 0.75 0.00 0.14 0.36 0.00 0.00 0.00 0.38 0.00 0.00 0.00
met5     99.1 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.82 0.00 0.00 0.00
Total    98.2 0.19 0.20 0.27 0.00 0.22 0.17 0.10 0.08 0.00 0.52 0.01 0.01 0.02


phase3. Total Wire Length = 113725.29
phase3. Layer li1 wire length = 21.24
phase3. Layer met1 wire length = 12182.82
phase3. Layer met2 wire length = 44812.10
phase3. Layer met3 wire length = 36729.23
phase3. Layer met4 wire length = 14272.02
phase3. Layer met5 wire length = 5707.88
phase3. Total Number of Contacts = 28483
phase3. Via L1M1_PR count = 9940
phase3. Via M1M2_PR count = 9546
phase3. Via M2M3_PR count = 6762
phase3. Via M3M4_PR count = 1931
phase3. Via M4M5_PR count = 304
phase3. completed.
[End of Whole Chip Routing] Elapsed real time: 0:00:12 
[End of Whole Chip Routing] Elapsed cpu  time: sys=0:00:00 usr=0:00:13 total=0:00:13
[End of Whole Chip Routing] Stage (MB): Used  213  Alloctr  215  Proc  160 
[End of Whole Chip Routing] Total (MB): Used  239  Alloctr  243  Proc 6421 

Congestion utilization per direction:
Average vertical track utilization   =  0.96 %
Peak    vertical track utilization   = 128.57 %
Average horizontal track utilization =  1.25 %
Peak    horizontal track utilization = 157.14 %

[End of Global Routing] Elapsed real time: 0:00:13 
[End of Global Routing] Elapsed cpu  time: sys=0:00:00 usr=0:00:13 total=0:00:13
[End of Global Routing] Stage (MB): Used  191  Alloctr  193  Proc  160 
[End of Global Routing] Total (MB): Used  217  Alloctr  220  Proc 6421 
Updating congestion ...
Updating congestion ...
[End of dbOut] Elapsed real time: 0:00:00 
[End of dbOut] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[End of dbOut] Stage (MB): Used   -8  Alloctr   -8  Proc    0 
[End of dbOut] Total (MB): Used   56  Alloctr   57  Proc 6421 

Start track assignment

Printing options for 'route.common.*'

Printing options for 'route.track.*'

Information: Using 8 threads for routing. (ZRT-444)

[Track Assign: TA init] Elapsed real time: 0:00:00 
[Track Assign: TA init] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[Track Assign: TA init] Stage (MB): Used    4  Alloctr    4  Proc    0 
[Track Assign: TA init] Total (MB): Used   25  Alloctr   26  Proc 6421 

Start initial assignment

Assign Horizontal partitions, iteration 0
Routed partition 1/30       
Routed partition 2/30       
Routed partition 3/30       
Routed partition 4/30       
Routed partition 5/30       
Routed partition 6/30       
Routed partition 7/30       
Routed partition 8/30       
Routed partition 9/30       
Routed partition 10/30      
Routed partition 11/30      
Routed partition 12/30      
Routed partition 13/30      
Routed partition 14/30      
Routed partition 15/30      
Routed partition 16/30      
Routed partition 17/30      
Routed partition 18/30      
Routed partition 19/30      
Routed partition 20/30      
Routed partition 21/30      
Routed partition 22/30      
Routed partition 23/30      
Routed partition 24/30      
Routed partition 25/30      
Routed partition 26/30      
Routed partition 27/30      
Routed partition 28/30      
Routed partition 29/30      
Routed partition 30/30      

Assign Vertical partitions, iteration 0
Routed partition 1/30       
Routed partition 2/30       
Routed partition 3/30       
Routed partition 4/30       
Routed partition 5/30       
Routed partition 6/30       
Routed partition 7/30       
Routed partition 8/30       
Routed partition 9/30       
Routed partition 10/30      
Routed partition 11/30      
Routed partition 12/30      
Routed partition 13/30      
Routed partition 14/30      
Routed partition 15/30      
Routed partition 16/30      
Routed partition 17/30      
Routed partition 18/30      
Routed partition 19/30      
Routed partition 20/30      
Routed partition 21/30      
Routed partition 22/30      
Routed partition 23/30      
Routed partition 24/30      
Routed partition 25/30      
Routed partition 26/30      
Routed partition 27/30      
Routed partition 28/30      
Routed partition 29/30      
Routed partition 30/30      

Number of wires with overlap after iteration 0 = 19649 of 36041


[Track Assign: Iteration 0] Elapsed real time: 0:00:01 
[Track Assign: Iteration 0] Elapsed cpu  time: sys=0:00:01 usr=0:00:04 total=0:00:05
[Track Assign: Iteration 0] Stage (MB): Used   11  Alloctr   11  Proc   46 
[Track Assign: Iteration 0] Total (MB): Used   31  Alloctr   33  Proc 6467 

Reroute to fix overlaps (iter = 1)

Assign Horizontal partitions, iteration 1
Routed partition 1/30       
Routed partition 2/30       
Routed partition 3/30       
Routed partition 4/30       
Routed partition 5/30       
Routed partition 6/30       
Routed partition 7/30       
Routed partition 8/30       
Routed partition 9/30       
Routed partition 10/30      
Routed partition 11/30      
Routed partition 12/30      
Routed partition 13/30      
Routed partition 14/30      
Routed partition 15/30      
Routed partition 16/30      
Routed partition 17/30      
Routed partition 18/30      
Routed partition 19/30      
Routed partition 20/30      
Routed partition 21/30      
Routed partition 22/30      
Routed partition 23/30      
Routed partition 24/30      
Routed partition 25/30      
Routed partition 26/30      
Routed partition 27/30      
Routed partition 28/30      
Routed partition 29/30      
Routed partition 30/30      

Assign Vertical partitions, iteration 1
Routed partition 1/30       
Routed partition 2/30       
Routed partition 3/30       
Routed partition 4/30       
Routed partition 5/30       
Routed partition 6/30       
Routed partition 7/30       
Routed partition 8/30       
Routed partition 9/30       
Routed partition 10/30      
Routed partition 11/30      
Routed partition 12/30      
Routed partition 13/30      
Routed partition 14/30      
Routed partition 15/30      
Routed partition 16/30      
Routed partition 17/30      
Routed partition 18/30      
Routed partition 19/30      
Routed partition 20/30      
Routed partition 21/30      
Routed partition 22/30      
Routed partition 23/30      
Routed partition 24/30      
Routed partition 25/30      
Routed partition 26/30      
Routed partition 27/30      
Routed partition 28/30      
Routed partition 29/30      
Routed partition 30/30      

[Track Assign: Iteration 1] Elapsed real time: 0:00:03 
[Track Assign: Iteration 1] Elapsed cpu  time: sys=0:00:05 usr=0:00:11 total=0:00:16
[Track Assign: Iteration 1] Stage (MB): Used   11  Alloctr   12  Proc   62 
[Track Assign: Iteration 1] Total (MB): Used   31  Alloctr   34  Proc 6483 

Number of wires with overlap after iteration 1 = 12982 of 27884


Wire length and via report:
---------------------------
Number of li1 wires: 1985                 : 0
Number of met1 wires: 10563              L1M1_PR: 10239
Number of met2 wires: 9782               M1M2_PR: 10738
Number of met3 wires: 4454               M2M3_PR: 6353
Number of met4 wires: 926                M3M4_PR: 1625
Number of met5 wires: 174                M4M5_PR: 301
Total number of wires: 27884             vias: 29256

Total li1 wire length: 1014.8
Total met1 wire length: 16296.9
Total met2 wire length: 46707.1
Total met3 wire length: 35158.0
Total met4 wire length: 13214.3
Total met5 wire length: 5595.6
Total wire length: 117986.8

Longest li1 wire length: 4.7
Longest met1 wire length: 219.9
Longest met2 wire length: 1115.2
Longest met3 wire length: 1202.4
Longest met4 wire length: 818.0
Longest met5 wire length: 529.9

Info: numNewViaInsted = 0 
Warning: Shape {7694350 6855950 7696050 6875941} on layer li1 is not on min manufacturing grid. Snap it to {7694350 6855950 7696050 6875950}. The shape belongs to Net: core/n1560. (ZRT-543)
Warning: Shape {8554550 7722120 8556250 7728050} on layer li1 is not on min manufacturing grid. Snap it to {8554550 7722100 8556250 7728050}. The shape belongs to Net: core/CPU_Xreg_value_a4[3][28]. (ZRT-543)
Warning: Shape {8572950 6798150 8574650 6820849} on layer li1 is not on min manufacturing grid. Snap it to {8572950 6798150 8574650 6820850}. The shape belongs to Net: core/n1089. (ZRT-543)
Warning: Shape {7961150 7177127 7962850 7197650} on layer li1 is not on min manufacturing grid. Snap it to {7961150 7177100 7962850 7197650}. The shape belongs to Net: CLK. (ZRT-543)
Warning: Shape {8400494 6315350 8404450 6317050} on layer li1 is not on min manufacturing grid. Snap it to {8400450 6315350 8404450 6317050}. The shape belongs to Net: CLK. (ZRT-543)
Warning: Shape {8264750 6730150 8266450 6736007} on layer li1 is not on min manufacturing grid. Snap it to {8264750 6730150 8266450 6736050}. The shape belongs to Net: CLK. (ZRT-543)
Warning: Shape {8261650 8059550 8263350 8072992} on layer li1 is not on min manufacturing grid. Snap it to {8261650 8059550 8263350 8073000}. The shape belongs to Net: core/CPU_imem_rd_addr_a1[2]. (ZRT-543)
Warning: Shape {9437750 7308150 9439450 7312661} on layer li1 is not on min manufacturing grid. Snap it to {9437750 7308150 9439450 7312700}. The shape belongs to Net: core/CPU_src2_value_a3[9]. (ZRT-543)
Warning: Shape {7828731 6553350 7834050 6555050} on layer li1 is not on min manufacturing grid. Snap it to {7828700 6553350 7834050 6555050}. The shape belongs to Net: core/CPU_Xreg_value_a4[8][4]. (ZRT-543)
Warning: Shape {8124250 8015350 8125950 8019013} on layer li1 is not on min manufacturing grid. Snap it to {8124250 8015350 8125950 8019050}. The shape belongs to Net: core/CPU_imem_rd_addr_a1[2]. (ZRT-543)
Warning: Shape {8260150 6471164 8261850 6493850} on layer li1 is not on min manufacturing grid. Snap it to {8260150 6471150 8261850 6493850}. The shape belongs to Net: CLK. (ZRT-543)
Warning: Shape {8871950 7149052 8873650 7153450} on layer li1 is not on min manufacturing grid. Snap it to {8871950 7149050 8873650 7153450}. The shape belongs to Net: core/CPU_Xreg_value_a4[3][23]. (ZRT-543)
Warning: Shape {7749550 7448734 7751250 7469650} on layer li1 is not on min manufacturing grid. Snap it to {7749550 7448700 7751250 7469650}. The shape belongs to Net: CLK. (ZRT-543)
Warning: Shape {7988750 6954550 7990450 6959172} on layer li1 is not on min manufacturing grid. Snap it to {7988750 6954550 7990450 6959200}. The shape belongs to Net: core/n158. (ZRT-543)
Warning: Shape {8361350 7223150 8364816 7224850} on layer li1 is not on min manufacturing grid. Snap it to {8361350 7223150 8364850 7224850}. The shape belongs to Net: CLK. (ZRT-543)
Warning: Shape {7965750 7977036 7967450 7983050} on layer li1 is not on min manufacturing grid. Snap it to {7965750 7977000 7967450 7983050}. The shape belongs to Net: core/CPU_imem_rd_addr_a1[2]. (ZRT-543)
Warning: Shape {7657450 6768323 7659150 6772650} on layer li1 is not on min manufacturing grid. Snap it to {7657450 6768300 7659150 6772650}. The shape belongs to Net: core/CPU_Xreg_value_a4[10][3]. (ZRT-543)
Warning: Shape {8697900 8178550 8699600 8182223} on layer li1 is not on min manufacturing grid. Snap it to {8697900 8178550 8699600 8182250}. The shape belongs to Net: CLK. (ZRT-543)
Warning: Shape {7813950 7539350 7815650 7546249} on layer li1 is not on min manufacturing grid. Snap it to {7813950 7539350 7815650 7546250}. The shape belongs to Net: core/CPU_src1_value_a3[0]. (ZRT-543)
Warning: Shape {8197050 8144991 8198750 8149650} on layer li1 is not on min manufacturing grid. Snap it to {8197050 8144950 8198750 8149650}. The shape belongs to Net: core/n1456. (ZRT-543)

[Track Assign: Done] Elapsed real time: 0:00:03 
[Track Assign: Done] Elapsed cpu  time: sys=0:00:05 usr=0:00:12 total=0:00:17
[Track Assign: Done] Stage (MB): Used    1  Alloctr    2  Proc   62 
[Track Assign: Done] Total (MB): Used   22  Alloctr   24  Proc 6483 
Printing options for 'route.common.*'

Printing options for 'route.detail.*'

Printing options for 'route.auto_via_ladder.*'

[Dr init] Elapsed real time: 0:00:00 
[Dr init] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[Dr init] Stage (MB): Used   38  Alloctr   38  Proc    0 
[Dr init] Total (MB): Used   61  Alloctr   62  Proc 6483 
Total number of nets = 2765, of which 0 are not extracted
Total number of open nets = 0, of which 0 are frozen
Information: Using 8 threads for routing. (ZRT-444)
Region based early termination has 3249 candidate regions.
Start DR iteration 0: uniform partition
Routed  2676/3249 Partitions, Violations =      0
Routed  2677/3249 Partitions, Violations =      0
Routed  2678/3249 Partitions, Violations =      12
Routed  2679/3249 Partitions, Violations =      12
Routed  2680/3249 Partitions, Violations =      12
Routed  2681/3249 Partitions, Violations =      12
Routed  2682/3249 Partitions, Violations =      12
Routed  2683/3249 Partitions, Violations =      12
Routed  2684/3249 Partitions, Violations =      14
Routed  2685/3249 Partitions, Violations =      14
Routed  2686/3249 Partitions, Violations =      14
Routed  2687/3249 Partitions, Violations =      14
Routed  2688/3249 Partitions, Violations =      14
Routed  2689/3249 Partitions, Violations =      14
Routed  2690/3249 Partitions, Violations =      14
Routed  2691/3249 Partitions, Violations =      14
Routed  2692/3249 Partitions, Violations =      14
Routed  2693/3249 Partitions, Violations =      14
Routed  2694/3249 Partitions, Violations =      14
Routed  2695/3249 Partitions, Violations =      14
Routed  2696/3249 Partitions, Violations =      14
Routed  2697/3249 Partitions, Violations =      14
Routed  2698/3249 Partitions, Violations =      14
Routed  2699/3249 Partitions, Violations =      14
Routed  2700/3249 Partitions, Violations =      14
Routed  2701/3249 Partitions, Violations =      14
Routed  2702/3249 Partitions, Violations =      14
Routed  2703/3249 Partitions, Violations =      14
Routed  2704/3249 Partitions, Violations =      14
Routed  2705/3249 Partitions, Violations =      14
Routed  2706/3249 Partitions, Violations =      14
Routed  2707/3249 Partitions, Violations =      14
Routed  2708/3249 Partitions, Violations =      14
Routed  2709/3249 Partitions, Violations =      14
Routed  2710/3249 Partitions, Violations =      14
Routed  2711/3249 Partitions, Violations =      14
Routed  2712/3249 Partitions, Violations =      14
Routed  2713/3249 Partitions, Violations =      14
Routed  2714/3249 Partitions, Violations =      18
Routed  2715/3249 Partitions, Violations =      18
Routed  2716/3249 Partitions, Violations =      18
Routed  2717/3249 Partitions, Violations =      18
Routed  2718/3249 Partitions, Violations =      18
Routed  2719/3249 Partitions, Violations =      18
Routed  2720/3249 Partitions, Violations =      18
Routed  2721/3249 Partitions, Violations =      18
Routed  2722/3249 Partitions, Violations =      18
Routed  2723/3249 Partitions, Violations =      18
Routed  2724/3249 Partitions, Violations =      22
Routed  2725/3249 Partitions, Violations =      22
Routed  2726/3249 Partitions, Violations =      22
Routed  2727/3249 Partitions, Violations =      22
Routed  2728/3249 Partitions, Violations =      22
Routed  2729/3249 Partitions, Violations =      18
Routed  2730/3249 Partitions, Violations =      18
Routed  2731/3249 Partitions, Violations =      18
Routed  2733/3249 Partitions, Violations =      18
Routed  2733/3249 Partitions, Violations =      18
Routed  2734/3249 Partitions, Violations =      18
Routed  2735/3249 Partitions, Violations =      20
Routed  2736/3249 Partitions, Violations =      20
Routed  2737/3249 Partitions, Violations =      20
Routed  2738/3249 Partitions, Violations =      20
Routed  2739/3249 Partitions, Violations =      22
Routed  2740/3249 Partitions, Violations =      22
Routed  2741/3249 Partitions, Violations =      22
Routed  2742/3249 Partitions, Violations =      22
Routed  2743/3249 Partitions, Violations =      22
Routed  2744/3249 Partitions, Violations =      22
Routed  2745/3249 Partitions, Violations =      22
Routed  2746/3249 Partitions, Violations =      22
Routed  2748/3249 Partitions, Violations =      22
Routed  2749/3249 Partitions, Violations =      22
Routed  2749/3249 Partitions, Violations =      22
Routed  2750/3249 Partitions, Violations =      22
Routed  2751/3249 Partitions, Violations =      22
Routed  2752/3249 Partitions, Violations =      22
Routed  2753/3249 Partitions, Violations =      22
Routed  2754/3249 Partitions, Violations =      22
Routed  2755/3249 Partitions, Violations =      24
Routed  2756/3249 Partitions, Violations =      24
Routed  2757/3249 Partitions, Violations =      24
Routed  2758/3249 Partitions, Violations =      150
Routed  2759/3249 Partitions, Violations =      152
Routed  2761/3249 Partitions, Violations =      152
Routed  2762/3249 Partitions, Violations =      152
Routed  2762/3249 Partitions, Violations =      152
Routed  2763/3249 Partitions, Violations =      152
Routed  2764/3249 Partitions, Violations =      154
Routed  2765/3249 Partitions, Violations =      154
Routed  2766/3249 Partitions, Violations =      154
Routed  2767/3249 Partitions, Violations =      154
Routed  2768/3249 Partitions, Violations =      154
Routed  2769/3249 Partitions, Violations =      156
Routed  2770/3249 Partitions, Violations =      156
Routed  2771/3249 Partitions, Violations =      156
Routed  2772/3249 Partitions, Violations =      152
Routed  2773/3249 Partitions, Violations =      152
Routed  2774/3249 Partitions, Violations =      152
Routed  2775/3249 Partitions, Violations =      154
Routed  2776/3249 Partitions, Violations =      154
Routed  2777/3249 Partitions, Violations =      154
Routed  2779/3249 Partitions, Violations =      154
Routed  2780/3249 Partitions, Violations =      154
Routed  2780/3249 Partitions, Violations =      154
Routed  2781/3249 Partitions, Violations =      154
Routed  2782/3249 Partitions, Violations =      156
Routed  2783/3249 Partitions, Violations =      156
Routed  2784/3249 Partitions, Violations =      121
Routed  2785/3249 Partitions, Violations =      121
Routed  2786/3249 Partitions, Violations =      121
Routed  2787/3249 Partitions, Violations =      121
Routed  2788/3249 Partitions, Violations =      121
Routed  2789/3249 Partitions, Violations =      121
Routed  2790/3249 Partitions, Violations =      121
Routed  2791/3249 Partitions, Violations =      121
Routed  2792/3249 Partitions, Violations =      121
Routed  2793/3249 Partitions, Violations =      121
Routed  2794/3249 Partitions, Violations =      121
Routed  2795/3249 Partitions, Violations =      121
Routed  2796/3249 Partitions, Violations =      121
Routed  2797/3249 Partitions, Violations =      121
Routed  2798/3249 Partitions, Violations =      121
Routed  2799/3249 Partitions, Violations =      121
Routed  2800/3249 Partitions, Violations =      121
Routed  2801/3249 Partitions, Violations =      121
Routed  2802/3249 Partitions, Violations =      121
Routed  2803/3249 Partitions, Violations =      121
Routed  2804/3249 Partitions, Violations =      121
Routed  2805/3249 Partitions, Violations =      121
Routed  2806/3249 Partitions, Violations =      121
Routed  2807/3249 Partitions, Violations =      121
Routed  2808/3249 Partitions, Violations =      121
Routed  2809/3249 Partitions, Violations =      121
Routed  2810/3249 Partitions, Violations =      121
Routed  2811/3249 Partitions, Violations =      121
Routed  2812/3249 Partitions, Violations =      121
Routed  2813/3249 Partitions, Violations =      121
Routed  2814/3249 Partitions, Violations =      121
Routed  2815/3249 Partitions, Violations =      121
Routed  2816/3249 Partitions, Violations =      121
Routed  2817/3249 Partitions, Violations =      121
Routed  2818/3249 Partitions, Violations =      121
Routed  2819/3249 Partitions, Violations =      121
Routed  2820/3249 Partitions, Violations =      121
Routed  2821/3249 Partitions, Violations =      121
Routed  2822/3249 Partitions, Violations =      121
Routed  2823/3249 Partitions, Violations =      121
Routed  2824/3249 Partitions, Violations =      121
Routed  2825/3249 Partitions, Violations =      121
Routed  2826/3249 Partitions, Violations =      121
Routed  2827/3249 Partitions, Violations =      121
Routed  2828/3249 Partitions, Violations =      121
Routed  2829/3249 Partitions, Violations =      123
Routed  2831/3249 Partitions, Violations =      123
Routed  2832/3249 Partitions, Violations =      123
Routed  2832/3249 Partitions, Violations =      123
Routed  2833/3249 Partitions, Violations =      123
Routed  2834/3249 Partitions, Violations =      123
Routed  2835/3249 Partitions, Violations =      123
Routed  2836/3249 Partitions, Violations =      119
Routed  2837/3249 Partitions, Violations =      119
Routed  2838/3249 Partitions, Violations =      119
Routed  2839/3249 Partitions, Violations =      119
Routed  2840/3249 Partitions, Violations =      119
Routed  2841/3249 Partitions, Violations =      119
Routed  2842/3249 Partitions, Violations =      119
Routed  2843/3249 Partitions, Violations =      119
Routed  2844/3249 Partitions, Violations =      119
Routed  2845/3249 Partitions, Violations =      119
Routed  2846/3249 Partitions, Violations =      119
Routed  2847/3249 Partitions, Violations =      121
Routed  2848/3249 Partitions, Violations =      121
Routed  2849/3249 Partitions, Violations =      121
Routed  2850/3249 Partitions, Violations =      123
Routed  2851/3249 Partitions, Violations =      123
Routed  2852/3249 Partitions, Violations =      123
Routed  2853/3249 Partitions, Violations =      123
Routed  2854/3249 Partitions, Violations =      123
Routed  2864/3249 Partitions, Violations =      121
Routed  2880/3249 Partitions, Violations =      123
Routed  2896/3249 Partitions, Violations =      115
Routed  2912/3249 Partitions, Violations =      356
Routed  2928/3249 Partitions, Violations =      358
Routed  2944/3249 Partitions, Violations =      359
Routed  2960/3249 Partitions, Violations =      295
Routed  2976/3249 Partitions, Violations =      470
Routed  2992/3249 Partitions, Violations =      470
Routed  3008/3249 Partitions, Violations =      470
Routed  3024/3249 Partitions, Violations =      570
Routed  3040/3249 Partitions, Violations =      1012
Routed  3056/3249 Partitions, Violations =      1012
Routed  3072/3249 Partitions, Violations =      1036
Routed  3088/3249 Partitions, Violations =      1036
Routed  3104/3249 Partitions, Violations =      1053
Routed  3120/3249 Partitions, Violations =      991
Routed  3136/3249 Partitions, Violations =      1808
Routed  3152/3249 Partitions, Violations =      1808
Routed  3168/3249 Partitions, Violations =      2532
Routed  3184/3249 Partitions, Violations =      3242
Routed  3200/3249 Partitions, Violations =      4472
Routed  3216/3249 Partitions, Violations =      5593
Routed  3232/3249 Partitions, Violations =      6649
Routed  3248/3249 Partitions, Violations =      8394

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      8360
        Diff net spacing : 4146
        Diff net via-cut spacing : 133
        Less than minimum area : 6
        Less than minimum width : 108
        Same net spacing : 143
        Same net via-cut spacing : 3
        Short : 3821

[Iter 0] Elapsed real time: 0:01:10 
[Iter 0] Elapsed cpu  time: sys=0:00:01 usr=0:01:19 total=0:01:20
[Iter 0] Stage (MB): Used   96  Alloctr   98  Proc  286 
[Iter 0] Total (MB): Used  118  Alloctr  122  Proc 6769 

End DR iteration 0 with 3249 parts

Start DR iteration 1: non-uniform partition
Routed  1/166 Partitions, Violations =  8258
Routed  2/166 Partitions, Violations =  8259
Routed  3/166 Partitions, Violations =  8376
Routed  4/166 Partitions, Violations =  8379
Routed  5/166 Partitions, Violations =  8356
Routed  6/166 Partitions, Violations =  8294
Routed  7/166 Partitions, Violations =  8360
Routed  8/166 Partitions, Violations =  8359
Routed  9/166 Partitions, Violations =  8333
Routed  10/166 Partitions, Violations = 8278
Routed  11/166 Partitions, Violations = 8267
Routed  12/166 Partitions, Violations = 8246
Routed  13/166 Partitions, Violations = 8245
Routed  14/166 Partitions, Violations = 8244
Routed  15/166 Partitions, Violations = 8264
Routed  16/166 Partitions, Violations = 8262
Routed  17/166 Partitions, Violations = 8261
Routed  18/166 Partitions, Violations = 8239
Routed  19/166 Partitions, Violations = 8214
Routed  20/166 Partitions, Violations = 8112
Routed  21/166 Partitions, Violations = 8098
Routed  22/166 Partitions, Violations = 8088
Routed  23/166 Partitions, Violations = 8083
Routed  24/166 Partitions, Violations = 8029
Routed  25/166 Partitions, Violations = 8086
Routed  26/166 Partitions, Violations = 8086
Routed  27/166 Partitions, Violations = 8081
Routed  28/166 Partitions, Violations = 8079
Routed  29/166 Partitions, Violations = 8086
Routed  30/166 Partitions, Violations = 7965
Routed  31/166 Partitions, Violations = 8082
Routed  32/166 Partitions, Violations = 8055
Routed  33/166 Partitions, Violations = 8075
Routed  34/166 Partitions, Violations = 8059
Routed  35/166 Partitions, Violations = 8086
Routed  36/166 Partitions, Violations = 8090
Routed  37/166 Partitions, Violations = 8081
Routed  38/166 Partitions, Violations = 8079
Routed  39/166 Partitions, Violations = 8078
Routed  40/166 Partitions, Violations = 8014
Routed  41/166 Partitions, Violations = 7990
Routed  42/166 Partitions, Violations = 7959
Routed  43/166 Partitions, Violations = 7960
Routed  44/166 Partitions, Violations = 7938
Routed  45/166 Partitions, Violations = 7883
Routed  46/166 Partitions, Violations = 7880
Routed  47/166 Partitions, Violations = 7861
Routed  48/166 Partitions, Violations = 7860
Routed  49/166 Partitions, Violations = 7869
Routed  50/166 Partitions, Violations = 7878
Routed  51/166 Partitions, Violations = 7873
Routed  52/166 Partitions, Violations = 7871
Routed  53/166 Partitions, Violations = 7866
Routed  54/166 Partitions, Violations = 7722
Routed  55/166 Partitions, Violations = 7860
Routed  56/166 Partitions, Violations = 7847
Routed  57/166 Partitions, Violations = 7854
Routed  58/166 Partitions, Violations = 7853
Routed  59/166 Partitions, Violations = 7828
Routed  60/166 Partitions, Violations = 7824
Routed  61/166 Partitions, Violations = 7731
Routed  62/166 Partitions, Violations = 7833
Routed  63/166 Partitions, Violations = 7724
Routed  64/166 Partitions, Violations = 7815
Routed  65/166 Partitions, Violations = 7775
Routed  66/166 Partitions, Violations = 7786
Routed  67/166 Partitions, Violations = 7788
Routed  68/166 Partitions, Violations = 7766
Routed  69/166 Partitions, Violations = 7781
Routed  70/166 Partitions, Violations = 7608
Routed  71/166 Partitions, Violations = 7724
Routed  72/166 Partitions, Violations = 7695
Routed  73/166 Partitions, Violations = 7611
Routed  74/166 Partitions, Violations = 7601
Routed  75/166 Partitions, Violations = 7676
Routed  76/166 Partitions, Violations = 7667
Routed  77/166 Partitions, Violations = 7638
Routed  78/166 Partitions, Violations = 7652
Routed  79/166 Partitions, Violations = 7673
Routed  80/166 Partitions, Violations = 7694
Routed  81/166 Partitions, Violations = 7700
Routed  82/166 Partitions, Violations = 7684
Routed  83/166 Partitions, Violations = 7691
Routed  84/166 Partitions, Violations = 7641
Routed  85/166 Partitions, Violations = 7519
Routed  86/166 Partitions, Violations = 7614
Routed  87/166 Partitions, Violations = 7566
Routed  88/166 Partitions, Violations = 7634
Routed  89/166 Partitions, Violations = 7608
Routed  90/166 Partitions, Violations = 7610
Routed  91/166 Partitions, Violations = 7642
Routed  92/166 Partitions, Violations = 7623
Routed  93/166 Partitions, Violations = 7590
Routed  94/166 Partitions, Violations = 7584
Routed  95/166 Partitions, Violations = 7570
Routed  96/166 Partitions, Violations = 7445
Routed  97/166 Partitions, Violations = 7607
Routed  98/166 Partitions, Violations = 7576
Routed  99/166 Partitions, Violations = 7576
Routed  100/166 Partitions, Violations =        7582
Routed  101/166 Partitions, Violations =        7581
Routed  102/166 Partitions, Violations =        7405
Routed  103/166 Partitions, Violations =        7523
Routed  104/166 Partitions, Violations =        7498
Routed  105/166 Partitions, Violations =        7484
Routed  106/166 Partitions, Violations =        7491
Routed  107/166 Partitions, Violations =        7410
Routed  108/166 Partitions, Violations =        7491
Routed  109/166 Partitions, Violations =        7487
Routed  110/166 Partitions, Violations =        7471
Routed  111/166 Partitions, Violations =        7393
Routed  112/166 Partitions, Violations =        7471
Routed  113/166 Partitions, Violations =        7470
Routed  114/166 Partitions, Violations =        7434
Routed  115/166 Partitions, Violations =        7466
Routed  116/166 Partitions, Violations =        7450
Routed  117/166 Partitions, Violations =        7447
Routed  118/166 Partitions, Violations =        7448
Routed  119/166 Partitions, Violations =        7447
Routed  120/166 Partitions, Violations =        7403
Routed  121/166 Partitions, Violations =        7307
Routed  122/166 Partitions, Violations =        7390
Routed  123/166 Partitions, Violations =        7386
Routed  124/166 Partitions, Violations =        7328
Routed  125/166 Partitions, Violations =        7334
Routed  126/166 Partitions, Violations =        7334
Routed  127/166 Partitions, Violations =        7288
Routed  128/166 Partitions, Violations =        7321
Routed  129/166 Partitions, Violations =        7314
Routed  130/166 Partitions, Violations =        7266
Routed  131/166 Partitions, Violations =        7232
Routed  132/166 Partitions, Violations =        7238
Routed  133/166 Partitions, Violations =        7230
Routed  134/166 Partitions, Violations =        7173
Routed  135/166 Partitions, Violations =        7209
Routed  136/166 Partitions, Violations =        7219
Routed  137/166 Partitions, Violations =        7216
Routed  138/166 Partitions, Violations =        7218
Routed  139/166 Partitions, Violations =        7218
Routed  140/166 Partitions, Violations =        7213
Routed  141/166 Partitions, Violations =        7216
Routed  142/166 Partitions, Violations =        7209
Routed  143/166 Partitions, Violations =        7195
Routed  144/166 Partitions, Violations =        7130
Routed  145/166 Partitions, Violations =        7125
Routed  146/166 Partitions, Violations =        7125
Routed  147/166 Partitions, Violations =        7125
Routed  148/166 Partitions, Violations =        7127
Routed  149/166 Partitions, Violations =        7115
Routed  150/166 Partitions, Violations =        7070
Routed  151/166 Partitions, Violations =        7077
Routed  152/166 Partitions, Violations =        7057
Routed  153/166 Partitions, Violations =        7052
Routed  154/166 Partitions, Violations =        7070
Routed  155/166 Partitions, Violations =        7059
Routed  156/166 Partitions, Violations =        7006
Routed  157/166 Partitions, Violations =        6996
Routed  158/166 Partitions, Violations =        7002
Routed  159/166 Partitions, Violations =        6975
Routed  160/166 Partitions, Violations =        6902
Routed  161/166 Partitions, Violations =        6909
Routed  162/166 Partitions, Violations =        6869
Routed  163/166 Partitions, Violations =        6855
Routed  164/166 Partitions, Violations =        6853
Routed  165/166 Partitions, Violations =        6856
Routed  166/166 Partitions, Violations =        6790

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      6790
        Diff net spacing : 3373
        Diff net via-cut spacing : 87
        Less than minimum area : 3
        Less than minimum width : 92
        Same net spacing : 89
        Same net via-cut spacing : 3
        Short : 3143

[Iter 1] Elapsed real time: 0:02:10 
[Iter 1] Elapsed cpu  time: sys=0:00:02 usr=0:02:23 total=0:02:25
[Iter 1] Stage (MB): Used   96  Alloctr   99  Proc  287 
[Iter 1] Total (MB): Used  118  Alloctr  123  Proc 6771 

End DR iteration 1 with 166 parts

Start DR iteration 2: non-uniform partition
Routed  1/129 Partitions, Violations =  6690
Routed  2/129 Partitions, Violations =  6770
Routed  3/129 Partitions, Violations =  6756
Routed  4/129 Partitions, Violations =  6738
Routed  5/129 Partitions, Violations =  6737
Routed  6/129 Partitions, Violations =  6755
Routed  7/129 Partitions, Violations =  6763
Routed  8/129 Partitions, Violations =  6649
Routed  9/129 Partitions, Violations =  6729
Routed  10/129 Partitions, Violations = 6732
Routed  11/129 Partitions, Violations = 6736
Routed  12/129 Partitions, Violations = 6746
Routed  13/129 Partitions, Violations = 6761
Routed  14/129 Partitions, Violations = 6755
Routed  15/129 Partitions, Violations = 6763
Routed  16/129 Partitions, Violations = 6634
Routed  17/129 Partitions, Violations = 6767
Routed  18/129 Partitions, Violations = 6746
Routed  19/129 Partitions, Violations = 6757
Routed  20/129 Partitions, Violations = 6750
Routed  21/129 Partitions, Violations = 6633
Routed  22/129 Partitions, Violations = 6744
Routed  23/129 Partitions, Violations = 6738
Routed  24/129 Partitions, Violations = 6653
Routed  25/129 Partitions, Violations = 6734
Routed  26/129 Partitions, Violations = 6739
Routed  27/129 Partitions, Violations = 6741
Routed  28/129 Partitions, Violations = 6754
Routed  29/129 Partitions, Violations = 6666
Routed  30/129 Partitions, Violations = 6763
Routed  31/129 Partitions, Violations = 6760
Routed  32/129 Partitions, Violations = 6763
Routed  33/129 Partitions, Violations = 6760
Routed  34/129 Partitions, Violations = 6752
Routed  35/129 Partitions, Violations = 6755
Routed  36/129 Partitions, Violations = 6717
Routed  37/129 Partitions, Violations = 6758
Routed  38/129 Partitions, Violations = 6758
Routed  39/129 Partitions, Violations = 6702
Routed  40/129 Partitions, Violations = 6757
Routed  41/129 Partitions, Violations = 6746
Routed  42/129 Partitions, Violations = 6751
Routed  43/129 Partitions, Violations = 6757
Routed  44/129 Partitions, Violations = 6764
Routed  45/129 Partitions, Violations = 6754
Routed  46/129 Partitions, Violations = 6761
Routed  47/129 Partitions, Violations = 6741
Routed  48/129 Partitions, Violations = 6728
Routed  49/129 Partitions, Violations = 6742
Routed  50/129 Partitions, Violations = 6710
Routed  51/129 Partitions, Violations = 6697
Routed  52/129 Partitions, Violations = 6690
Routed  53/129 Partitions, Violations = 6680
Routed  54/129 Partitions, Violations = 6670
Routed  55/129 Partitions, Violations = 6657
Routed  56/129 Partitions, Violations = 6648
Routed  57/129 Partitions, Violations = 6641
Routed  58/129 Partitions, Violations = 6637
Routed  59/129 Partitions, Violations = 6527
Routed  60/129 Partitions, Violations = 6624
Routed  61/129 Partitions, Violations = 6631
Routed  62/129 Partitions, Violations = 6628
Routed  63/129 Partitions, Violations = 6628
Routed  64/129 Partitions, Violations = 6531
Routed  65/129 Partitions, Violations = 6629
Routed  66/129 Partitions, Violations = 6629
Routed  67/129 Partitions, Violations = 6631
Routed  68/129 Partitions, Violations = 6509
Routed  69/129 Partitions, Violations = 6626
Routed  70/129 Partitions, Violations = 6640
Routed  71/129 Partitions, Violations = 6524
Routed  72/129 Partitions, Violations = 6607
Routed  73/129 Partitions, Violations = 6606
Routed  74/129 Partitions, Violations = 6588
Routed  75/129 Partitions, Violations = 6500
Routed  76/129 Partitions, Violations = 6575
Routed  77/129 Partitions, Violations = 6579
Routed  78/129 Partitions, Violations = 6570
Routed  79/129 Partitions, Violations = 6573
Routed  80/129 Partitions, Violations = 6575
Routed  81/129 Partitions, Violations = 6554
Routed  82/129 Partitions, Violations = 6574
Routed  83/129 Partitions, Violations = 6577
Routed  84/129 Partitions, Violations = 6553
Routed  85/129 Partitions, Violations = 6549
Routed  86/129 Partitions, Violations = 6469
Routed  87/129 Partitions, Violations = 6542
Routed  88/129 Partitions, Violations = 6470
Routed  89/129 Partitions, Violations = 6470
Routed  90/129 Partitions, Violations = 6554
Routed  91/129 Partitions, Violations = 6555
Routed  92/129 Partitions, Violations = 6497
Routed  93/129 Partitions, Violations = 6557
Routed  94/129 Partitions, Violations = 6560
Routed  95/129 Partitions, Violations = 6569
Routed  96/129 Partitions, Violations = 6574
Routed  97/129 Partitions, Violations = 6568
Routed  98/129 Partitions, Violations = 6567
Routed  99/129 Partitions, Violations = 6560
Routed  100/129 Partitions, Violations =        6510
Routed  101/129 Partitions, Violations =        6482
Routed  102/129 Partitions, Violations =        6455
Routed  103/129 Partitions, Violations =        6444
Routed  104/129 Partitions, Violations =        6452
Routed  105/129 Partitions, Violations =        6452
Routed  106/129 Partitions, Violations =        6445
Routed  107/129 Partitions, Violations =        6431
Routed  108/129 Partitions, Violations =        6421
Routed  109/129 Partitions, Violations =        6423
Routed  110/129 Partitions, Violations =        6416
Routed  111/129 Partitions, Violations =        6420
Routed  112/129 Partitions, Violations =        6428
Routed  113/129 Partitions, Violations =        6431
Routed  114/129 Partitions, Violations =        6429
Routed  115/129 Partitions, Violations =        6430
Routed  116/129 Partitions, Violations =        6447
Routed  117/129 Partitions, Violations =        6456
Routed  118/129 Partitions, Violations =        6492
Routed  119/129 Partitions, Violations =        6488
Routed  120/129 Partitions, Violations =        6483
Routed  121/129 Partitions, Violations =        6480
Routed  122/129 Partitions, Violations =        6493
Routed  123/129 Partitions, Violations =        6495
Routed  124/129 Partitions, Violations =        6478
Routed  125/129 Partitions, Violations =        6463
Routed  126/129 Partitions, Violations =        6454
Routed  127/129 Partitions, Violations =        6448
Routed  128/129 Partitions, Violations =        6444
Routed  129/129 Partitions, Violations =        6449

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      6449
        Diff net spacing : 3231
        Diff net via-cut spacing : 76
        Less than minimum area : 1
        Less than minimum width : 84
        Same net spacing : 83
        Same net via-cut spacing : 3
        Short : 2971

[Iter 2] Elapsed real time: 0:03:23 
[Iter 2] Elapsed cpu  time: sys=0:00:03 usr=0:03:41 total=0:03:44
[Iter 2] Stage (MB): Used   96  Alloctr   99  Proc  288 
[Iter 2] Total (MB): Used  118  Alloctr  123  Proc 6771 

End DR iteration 2 with 129 parts

Start DR iteration 3: non-uniform partition
Routed  1/120 Partitions, Violations =  6303
Routed  2/120 Partitions, Violations =  6456
Routed  3/120 Partitions, Violations =  6472
Routed  4/120 Partitions, Violations =  6470
Routed  5/120 Partitions, Violations =  6476
Routed  6/120 Partitions, Violations =  6337
Routed  7/120 Partitions, Violations =  6475
Routed  8/120 Partitions, Violations =  6464
Routed  9/120 Partitions, Violations =  6481
Routed  10/120 Partitions, Violations = 6488
Routed  11/120 Partitions, Violations = 6403
Routed  12/120 Partitions, Violations = 6483
Routed  13/120 Partitions, Violations = 6473
Routed  14/120 Partitions, Violations = 6468
Routed  15/120 Partitions, Violations = 6472
Routed  16/120 Partitions, Violations = 6475
Routed  17/120 Partitions, Violations = 6405
Routed  18/120 Partitions, Violations = 6402
Routed  19/120 Partitions, Violations = 6311
Routed  20/120 Partitions, Violations = 6395
Routed  21/120 Partitions, Violations = 6397
Routed  22/120 Partitions, Violations = 6397
Routed  23/120 Partitions, Violations = 6431
Routed  24/120 Partitions, Violations = 6433
Routed  25/120 Partitions, Violations = 6344
Routed  26/120 Partitions, Violations = 6342
Routed  27/120 Partitions, Violations = 6439
Routed  28/120 Partitions, Violations = 6423
Routed  29/120 Partitions, Violations = 6423
Routed  30/120 Partitions, Violations = 6395
Routed  31/120 Partitions, Violations = 6387
Routed  32/120 Partitions, Violations = 6385
Routed  33/120 Partitions, Violations = 6388
Routed  34/120 Partitions, Violations = 6384
Routed  35/120 Partitions, Violations = 6377
Routed  36/120 Partitions, Violations = 6250
Routed  37/120 Partitions, Violations = 6363
Routed  38/120 Partitions, Violations = 6355
Routed  39/120 Partitions, Violations = 6362
Routed  40/120 Partitions, Violations = 6367
Routed  41/120 Partitions, Violations = 6351
Routed  42/120 Partitions, Violations = 6347
Routed  43/120 Partitions, Violations = 6335
Routed  44/120 Partitions, Violations = 6283
Routed  45/120 Partitions, Violations = 6184
Routed  46/120 Partitions, Violations = 6244
Routed  47/120 Partitions, Violations = 6333
Routed  48/120 Partitions, Violations = 6333
Routed  49/120 Partitions, Violations = 6253
Routed  50/120 Partitions, Violations = 6319
Routed  51/120 Partitions, Violations = 6316
Routed  52/120 Partitions, Violations = 6318
Routed  53/120 Partitions, Violations = 6305
Routed  54/120 Partitions, Violations = 6292
Routed  55/120 Partitions, Violations = 6300
Routed  56/120 Partitions, Violations = 6281
Routed  57/120 Partitions, Violations = 6285
Routed  58/120 Partitions, Violations = 6265
Routed  59/120 Partitions, Violations = 6158
Routed  60/120 Partitions, Violations = 6145
Routed  61/120 Partitions, Violations = 6273
Routed  62/120 Partitions, Violations = 6287
Routed  63/120 Partitions, Violations = 6294
Routed  64/120 Partitions, Violations = 6301
Routed  65/120 Partitions, Violations = 6307
Routed  66/120 Partitions, Violations = 6309
Routed  67/120 Partitions, Violations = 6314
Routed  68/120 Partitions, Violations = 6311
Routed  69/120 Partitions, Violations = 6245
Routed  70/120 Partitions, Violations = 6321
Routed  71/120 Partitions, Violations = 6329
Routed  72/120 Partitions, Violations = 6330
Routed  73/120 Partitions, Violations = 6309
Routed  74/120 Partitions, Violations = 6340
Routed  75/120 Partitions, Violations = 6346
Routed  76/120 Partitions, Violations = 6334
Routed  77/120 Partitions, Violations = 6215
Routed  78/120 Partitions, Violations = 6342
Routed  79/120 Partitions, Violations = 6344
Routed  80/120 Partitions, Violations = 6337
Routed  81/120 Partitions, Violations = 6334
Routed  82/120 Partitions, Violations = 6335
Routed  83/120 Partitions, Violations = 6333
Routed  84/120 Partitions, Violations = 6329
Routed  85/120 Partitions, Violations = 6315
Routed  86/120 Partitions, Violations = 6313
Routed  87/120 Partitions, Violations = 6289
Routed  88/120 Partitions, Violations = 6288
Routed  89/120 Partitions, Violations = 6263
Routed  90/120 Partitions, Violations = 6269
Routed  91/120 Partitions, Violations = 6264
Routed  92/120 Partitions, Violations = 6204
Routed  93/120 Partitions, Violations = 6262
Routed  94/120 Partitions, Violations = 6275
Routed  95/120 Partitions, Violations = 6263
Routed  96/120 Partitions, Violations = 6263
Routed  97/120 Partitions, Violations = 6263
Routed  98/120 Partitions, Violations = 6248
Routed  99/120 Partitions, Violations = 6210
Routed  100/120 Partitions, Violations =        6208
Routed  101/120 Partitions, Violations =        6196
Routed  102/120 Partitions, Violations =        6192
Routed  103/120 Partitions, Violations =        6209
Routed  104/120 Partitions, Violations =        6215
Routed  105/120 Partitions, Violations =        6223
Routed  106/120 Partitions, Violations =        6230
Routed  107/120 Partitions, Violations =        6234
Routed  108/120 Partitions, Violations =        6238
Routed  109/120 Partitions, Violations =        6241
Routed  110/120 Partitions, Violations =        6237
Routed  111/120 Partitions, Violations =        6264
Routed  112/120 Partitions, Violations =        6266
Routed  113/120 Partitions, Violations =        6269
Routed  114/120 Partitions, Violations =        6251
Routed  115/120 Partitions, Violations =        6252
Routed  116/120 Partitions, Violations =        6261
Routed  117/120 Partitions, Violations =        6265
Routed  118/120 Partitions, Violations =        6283
Routed  119/120 Partitions, Violations =        6273
Routed  120/120 Partitions, Violations =        6261

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      6261
        Diff net spacing : 3214
        Diff net via-cut spacing : 88
        Less than minimum area : 2
        Less than minimum width : 74
        Same net spacing : 68
        Short : 2815

[Iter 3] Elapsed real time: 0:05:04 
[Iter 3] Elapsed cpu  time: sys=0:00:05 usr=0:05:28 total=0:05:33
[Iter 3] Stage (MB): Used   96  Alloctr   99  Proc  290 
[Iter 3] Total (MB): Used  118  Alloctr  123  Proc 6773 

End DR iteration 3 with 120 parts

Start DR iteration 4: non-uniform partition
Routed  1/113 Partitions, Violations =  6172
Routed  2/113 Partitions, Violations =  6266
Routed  3/113 Partitions, Violations =  6270
Routed  4/113 Partitions, Violations =  6275
Routed  5/113 Partitions, Violations =  6262
Routed  6/113 Partitions, Violations =  6269
Routed  7/113 Partitions, Violations =  6261
Routed  8/113 Partitions, Violations =  6249
Routed  9/113 Partitions, Violations =  6219
Routed  10/113 Partitions, Violations = 6218
Routed  11/113 Partitions, Violations = 6230
Routed  12/113 Partitions, Violations = 6231
Routed  13/113 Partitions, Violations = 6232
Routed  14/113 Partitions, Violations = 6238
Routed  15/113 Partitions, Violations = 6241
Routed  16/113 Partitions, Violations = 6250
Routed  17/113 Partitions, Violations = 6241
Routed  18/113 Partitions, Violations = 6244
Routed  19/113 Partitions, Violations = 6248
Routed  20/113 Partitions, Violations = 6112
Routed  21/113 Partitions, Violations = 6246
Routed  22/113 Partitions, Violations = 6234
Routed  23/113 Partitions, Violations = 6179
Routed  24/113 Partitions, Violations = 6261
Routed  25/113 Partitions, Violations = 6252
Routed  26/113 Partitions, Violations = 6262
Routed  27/113 Partitions, Violations = 6266
Routed  28/113 Partitions, Violations = 6124
Routed  29/113 Partitions, Violations = 6281
Routed  30/113 Partitions, Violations = 6297
Routed  31/113 Partitions, Violations = 6291
Routed  32/113 Partitions, Violations = 6271
Routed  33/113 Partitions, Violations = 6267
Routed  34/113 Partitions, Violations = 6276
Routed  35/113 Partitions, Violations = 6042
Routed  36/113 Partitions, Violations = 6250
Routed  37/113 Partitions, Violations = 6261
Routed  38/113 Partitions, Violations = 6271
Routed  39/113 Partitions, Violations = 6283
Routed  40/113 Partitions, Violations = 6246
Routed  41/113 Partitions, Violations = 6302
Routed  42/113 Partitions, Violations = 6296
Routed  43/113 Partitions, Violations = 6238
Routed  44/113 Partitions, Violations = 6301
Routed  45/113 Partitions, Violations = 6299
Routed  46/113 Partitions, Violations = 6290
Routed  47/113 Partitions, Violations = 6166
Routed  48/113 Partitions, Violations = 6275
Routed  49/113 Partitions, Violations = 6189
Routed  50/113 Partitions, Violations = 6284
Routed  51/113 Partitions, Violations = 6283
Routed  52/113 Partitions, Violations = 6189
Routed  53/113 Partitions, Violations = 6294
Routed  54/113 Partitions, Violations = 6310
Routed  55/113 Partitions, Violations = 6305
Routed  56/113 Partitions, Violations = 6294
Routed  57/113 Partitions, Violations = 6313
Routed  58/113 Partitions, Violations = 6308
Routed  59/113 Partitions, Violations = 6306
Routed  60/113 Partitions, Violations = 6127
Routed  61/113 Partitions, Violations = 6311
Routed  62/113 Partitions, Violations = 6304
Routed  63/113 Partitions, Violations = 6280
Routed  64/113 Partitions, Violations = 6293
Routed  65/113 Partitions, Violations = 6292
Routed  66/113 Partitions, Violations = 6305
Routed  67/113 Partitions, Violations = 6290
Routed  68/113 Partitions, Violations = 6203
Routed  69/113 Partitions, Violations = 6280
Routed  70/113 Partitions, Violations = 6310
Routed  71/113 Partitions, Violations = 6295
Routed  72/113 Partitions, Violations = 6299
Routed  73/113 Partitions, Violations = 6309
Routed  74/113 Partitions, Violations = 6316
Routed  75/113 Partitions, Violations = 6270
Routed  76/113 Partitions, Violations = 6330
Routed  77/113 Partitions, Violations = 6321
Routed  78/113 Partitions, Violations = 6335
Routed  79/113 Partitions, Violations = 6332
Routed  80/113 Partitions, Violations = 6335
Routed  81/113 Partitions, Violations = 6323
Routed  82/113 Partitions, Violations = 6322
Routed  83/113 Partitions, Violations = 6309
Routed  84/113 Partitions, Violations = 6308
Routed  85/113 Partitions, Violations = 6318
Routed  86/113 Partitions, Violations = 6282
Routed  87/113 Partitions, Violations = 6290
Routed  88/113 Partitions, Violations = 6299
Routed  89/113 Partitions, Violations = 6274
Routed  90/113 Partitions, Violations = 6275
Routed  91/113 Partitions, Violations = 6295
Routed  92/113 Partitions, Violations = 6160
Routed  93/113 Partitions, Violations = 6311
Routed  94/113 Partitions, Violations = 6309
Routed  95/113 Partitions, Violations = 6270
Routed  96/113 Partitions, Violations = 6313
Routed  97/113 Partitions, Violations = 6315
Routed  98/113 Partitions, Violations = 6239
Routed  99/113 Partitions, Violations = 6293
Routed  100/113 Partitions, Violations =        6274
Routed  101/113 Partitions, Violations =        6263
Routed  102/113 Partitions, Violations =        6249
Routed  103/113 Partitions, Violations =        6262
Routed  104/113 Partitions, Violations =        6248
Routed  105/113 Partitions, Violations =        6242
Routed  106/113 Partitions, Violations =        6257
Routed  107/113 Partitions, Violations =        6251
Routed  108/113 Partitions, Violations =        6248
Routed  109/113 Partitions, Violations =        6266
Routed  110/113 Partitions, Violations =        6266
Routed  111/113 Partitions, Violations =        6272
Routed  112/113 Partitions, Violations =        6272
Routed  113/113 Partitions, Violations =        6255

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      6255
        Diff net spacing : 3226
        Diff net via-cut spacing : 87
        Less than minimum area : 1
        Less than minimum width : 64
        Same net spacing : 78
        Same net via-cut spacing : 1
        Short : 2798

[Iter 4] Elapsed real time: 0:06:52 
[Iter 4] Elapsed cpu  time: sys=0:00:07 usr=0:07:23 total=0:07:30
[Iter 4] Stage (MB): Used   96  Alloctr   99  Proc  292 
[Iter 4] Total (MB): Used  118  Alloctr  123  Proc 6776 

End DR iteration 4 with 113 parts

Stop DR since reached max number of iterations

Information: Discarded 118 drcs of internal-only type. (ZRT-306)
[DR] Elapsed real time: 0:06:52 
[DR] Elapsed cpu  time: sys=0:00:07 usr=0:07:23 total=0:07:30
[DR] Stage (MB): Used    2  Alloctr    5  Proc  292 
[DR] Total (MB): Used   25  Alloctr   29  Proc 6776 
[DR: Done] Elapsed real time: 0:06:52 
[DR: Done] Elapsed cpu  time: sys=0:00:07 usr=0:07:23 total=0:07:30
[DR: Done] Stage (MB): Used    2  Alloctr    5  Proc  292 
[DR: Done] Total (MB): Used   25  Alloctr   29  Proc 6776 

DR finished with 0 open nets, of which 0 are frozen
Information: Merged away 2502 aligned/redundant DRCs. (ZRT-305)

DR finished with 3753 violations

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      3753
        Diff net spacing : 1903
        Diff net via-cut spacing : 86
        Less than minimum area : 1
        Less than minimum width : 63
        Same net spacing : 62
        Same net via-cut spacing : 1
        Short : 1637



Total Wire Length =                    120702 micron
Total Number of Contacts =             29512
Total Number of Wires =                30152
Total Number of PtConns =              310
Total Number of Routed Wires =       30152
Total Routed Wire Length =           120576 micron
Total Number of Routed Contacts =       29512
        Layer          li1 :       1100 micron
        Layer         met1 :      13494 micron
        Layer         met2 :      47466 micron
        Layer         met3 :      38554 micron
        Layer         met4 :      14871 micron
        Layer         met5 :       5217 micron
        Via        M4M5_PR :        243
        Via        M3M4_PR :       1804
        Via      M3M4_PR_C :        240
        Via        M2M3_PR :       5968
        Via   M2M3_PR(rot) :       1159
        Via        M1M2_PR :      10301
        Via   M1M2_PR(rot) :         51
        Via        L1M1_PR :       8962
        Via   L1M1_PR(rot) :        597
        Via      L1M1_PR_C :        187

 
Redundant via conversion report:
--------------------------------

  Total optimized via conversion rate =  0.00% (0 / 29512 vias)
 
    Layer mcon       =  0.00% (0      / 9746    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (9746    vias)
    Layer via        =  0.00% (0      / 10352   vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (10352   vias)
    Layer via2       =  0.00% (0      / 7127    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (7127    vias)
    Layer via3       =  0.00% (0      / 2044    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (2044    vias)
    Layer via4       =  0.00% (0      / 243     vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (243     vias)
 
  Total double via conversion rate    =  0.00% (0 / 29512 vias)
 
    Layer mcon       =  0.00% (0      / 9746    vias)
    Layer via        =  0.00% (0      / 10352   vias)
    Layer via2       =  0.00% (0      / 7127    vias)
    Layer via3       =  0.00% (0      / 2044    vias)
    Layer via4       =  0.00% (0      / 243     vias)
 
  The optimized via conversion rate based on total routed via count =  0.00% (0 / 29512 vias)
 
    Layer mcon       =  0.00% (0      / 9746    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (9746    vias)
    Layer via        =  0.00% (0      / 10352   vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (10352   vias)
    Layer via2       =  0.00% (0      / 7127    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (7127    vias)
    Layer via3       =  0.00% (0      / 2044    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (2044    vias)
    Layer via4       =  0.00% (0      / 243     vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (243     vias)
 

Total number of nets = 2765
0 open nets, of which 0 are frozen
Total number of excluded ports = 0 ports of 0 unplaced cells connected to 0 nets
                                 0 ports without pins of 0 cells connected to 0 nets
                                 0 ports of 0 cover cells connected to 0 non-pg nets
Total number of DRCs = 3753
Total number of antenna violations = antenna checking not active
Information: Routes in non-preferred voltage areas = 0 (ZRT-559)

Topology ECO not run, no qualifying violations or in frozen nets.
Updating the database ...
Information: Extraction observers are detached as design net change threshold is reached.
Information: Ending 'route_auto' (FLW-8001)
Information: Time: 2024-07-21 02:37:14 / Session: 0.14 hr / Command: 0.12 hr / Memory: 1167 MB (FLW-8100)
[icc2-lic Sun Jul 21 02:37:14 2024] Command 'route_opt' requires licenses
[icc2-lic Sun Jul 21 02:37:14 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:37:14 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:37:14 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:37:14 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:37:14 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:37:14 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:37:14 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:37:14 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:37:14 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:37:14 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:37:14 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:37:14 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:37:14 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:37:14 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:37:14 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:37:14 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:37:14 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:37:14 2024] Check-out of alternate set of keys directly with queueing was successful
Information: Starting 'route_opt' (FLW-8000)
Information: Time: 2024-07-21 02:37:14 / Session: 0.14 hr / Command: 0.00 hr / Memory: 1167 MB (FLW-8100)
INFO: route_opt is running in balanced flow mode
Warning: SI analysis is not enabled for post-route optimization. (ROPT-002)
Warning: No default buffer/inverter available for voltage areas DEFAULT_VA. (OPT-014)
Warning: Can not find available buffer/inverter lib cells for the site unit. (OPT-016)
INFO: Total Power Aware Optimization Enabled (Dynamic + Leakage)
Information: Freeing timing information from routing. (ZRT-574)
Route-opt command begin                   CPU:   515 s (  0.14 hr )  ELAPSE:   517 s (  0.14 hr )  MEM-PEAK:  1167 MB
Warning: SI analysis is not enabled for post-route optimization. (ROPT-002)
Warning: No default buffer/inverter available for voltage areas DEFAULT_VA. (OPT-014)
Warning: Can not find available buffer/inverter lib cells for the site unit. (OPT-016)
INFO: place.legalize.enable_advanced_legalizer not set - need to revert CLO
INFO: Concurrent Legalization and Optimization (CLO) Reverted
INFO: Dynamic Scenario ASR Mode:  0
INFO: place.legalize.enable_advanced_legalizer not set - need to revert CLO
Information: The stitching and editing of coupling caps is turned OFF for design 'vsdbabysocsky130_fd_sc_hd:vsdbabysoc/init_dp.design'. (TIM-125)
Information: Design vsdbabysoc has 2765 nets, 0 global routed, 2740 detail routed. (NEX-024)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Information: The RC mode used is DR for design 'vsdbabysoc'. (NEX-022)
---extraction options---
Corner: func1
Global options:
 reference_direction       : use_from_tluplus
 real_metalfill_extraction : none
 virtual_shield_extraction : true
---app options---
 host.max_cores                   : 8
 extract.connect_open           : true
 extract.incremental_extraction : true
 extract.enable_coupling_cap    : false
Extracting design: vsdbabysoc 
Information: coupling capacitance is lumped to ground. (NEX-030)
Information: 2740 nets are successfully extracted. (NEX-028)
Information: Design Average RC for design vsdbabysoc  (NEX-011)
Information: r = 0.494087 ohm/um, via_r = 3.543520 ohm/cut, c = 0.155222 ff/um, cc = 0.000000 ff/um (X dir) (NEX-017)
Information: r = 0.761295 ohm/um, via_r = 3.588455 ohm/cut, c = 0.129413 ff/um, cc = 0.000000 ff/um (Y dir) (NEX-017)
Warning: Advanced receiver model has not been enabled for detailed routed design. (TIM-204)
Information: Update timing completed net estimation for all the timing graph nets (TIM-111)
Information: Net estimation statistics: timing graph nets = 2763, routed nets = 2740, across physical hierarchy nets = 0, parasitics cached nets = 2763, delay annotated nets = 0, parasitics annotated nets = 0, multi-voltage nets = 19. (TIM-112)
Info: update em.

Route-opt timing update complete          CPU:   519 s (  0.14 hr )  ELAPSE:   518 s (  0.14 hr )  MEM-PEAK:  1167 MB
INFO: Propagating Switching Activities
Information: Activity propagation will be performed for scenario func1.
Information: Doing activity propagation for mode 'func1' and corner 'func1' with effort level 'medium'. (POW-024)
Information: Timer-derived activity data is cached on scenario func1 (POW-052)
Information: Turn on parallel simulation of generator nets.
Information: Running switching activity propagation with 8 threads!

 **** Information : No. of simulation cycles = 6 ****
INFO: Switching Activity propagation took     0.00005 sec
INFO: Propagating Switching Activity for all power flows 

Route-opt initial QoR
_____________________
Scenario Mapping Table
1: func1

Pathgroup Mapping Table
1: **default**
2: **async_default**
3: **clock_gating_default**
4: **in2reg_default**
5: **reg2out_default**
6: **in2out_default**
7: clk

-------------------------------------------------------------------------------------------------------------------------------------------------------------
PATHGROUP QOR 
-------------------------------------------------------------------------------------------------------------------------------------------------------------
Scene  PG      WNS        TNS    NSV      WHV        THV    NHV
    1   1   0.0000     0.0000      0   0.0000     0.0000      0
    1   2   0.0000     0.0000      0   0.0000     0.0000      0
    1   3   0.0000     0.0000      0   0.0000     0.0000      0
    1   4   0.0000     0.0000      0   0.0000     0.0000      0
    1   5   0.0000     0.0000      0   0.0000     0.0000      0
    1   6   0.0000     0.0000      0   0.0000     0.0000      0
    1   7   1.7001    97.4143    101   0.1860    12.0822    225
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
SCENARIO QOR 
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Scene  PG      WNS        TNS   R2RTNS    NSV      WHV        THV    NHV  MaxTrnV   MaxTranC  MaxCapV    Leakage
    1   *   1.7001    97.4143  97.4143    101   0.1860    12.0822    225       42    20.2424       42      8.009
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
DESIGN QOR 
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Scene  PG      WNS        TNS   R2RTNS    NSV      WHV        THV    NHV  MaxTrnV   MaxTranC  MaxCapV    Leakage         Area    InstCnt
    *   *   1.7001    97.4143  97.4143    101   0.1860    12.0822    225       42    20.2424       42      8.009    696592.56       2743
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Route-opt initial QoR Summary       WNS        TNS   R2RTNS    NSV      WHV        THV    NHV  MaxTrnV  MaxCapV    Leakage         Area    InstCnt
Route-opt initial QoR Summary    1.7001    97.4143  97.4143    101   0.1860    12.0822    225       42       42      8.009    696592.56       2743
Information: The netlist change observers are disabled for incremental extraction. (TIM-126)
INFO: using 8 threads
xDensity is not ready for site component checking. The min area module collection degenerate into union-row mode.
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Route-opt initialization complete         CPU:   527 s (  0.15 hr )  ELAPSE:   522 s (  0.15 hr )  MEM-PEAK:  1167 MB
Information: Initializing classic cellmap without advanced rules enabled and with PDC enabled
Information: The following app options are used in cellmap
        place.legalize.enable_color_aware_placement : false
        place.legalize.use_nll_query_cm : false
        place.legalize.enable_advanced_legalizer : false
        place.legalize.enable_prerouted_net_check : true
        place.legalize.enable_advanced_prerouted_net_check : false
        place.legalize.always_continue : true
        place.legalize.limit_legality_checks : false
        place.common.pnet_aware_density : 1.0000
        place.common.pnet_aware_min_width : 0.0000
        place.common.pnet_aware_layers : {}
        place.common.use_placement_model : false
        place.common.enable_advanced_placement_model : true
        cts.placement.cell_spacing_rule_style : maximum
Information: Creating classic rule checker.
Warning: Routing direction of metal layer li1 is neither "horizontal" nor "vertical".  PDC checks will not be performed on this layer. (PDC-003)
Warning: Routing direction of metal layer fieldpoly is neither "horizontal" nor "vertical".  PDC checks will not be performed on this layer. (PDC-003)
PDC app_options settings =========
        place.legalize.enable_prerouted_net_check: 1
        place.legalize.num_tracks_for_access_check: 1
        place.legalize.use_eol_spacing_for_access_check: 0
        place.legalize.allow_touch_track_for_access_check: 1
        place.legalize.reduce_conservatism_in_eol_check: 0
        place.legalize.preroute_shape_merge_distance: 0.0
        place.legalize.enable_non_preferred_direction_span_check: 0

Layer met1: cached 207 shapes out of 10422 total shapes.
Layer met2: cached 0 shapes out of 11714 total shapes.
Cached 20324 vias out of 61638 total vias.
Total 0.2200 seconds to build cellmap data
INFO: creating 188(r) x 188(c) GridCells YDim 8.16 XDim 8.16
INFO: creating 188(r) x 188(c) GridCells YDim 8.16 XDim 8.16
Total 0.1000 seconds to load 2741 cell instances into cellmap, 2741 cells are off site row
Moveable cells: 2741; Application fixed cells: 0; Macro cells: 0; User fixed cells: 0
2740 out of 2991 data nets are detail routed, 0 out of 0 clock nets is detail routed and total 2991 nets have been analyzed
Only data route nets/shapes. Estimated current stage is after clock and data detail route stage
Average cell width 3.3452, cell height 2.7200, cell area 9.0989 for total 2741 placed and application fixed cells
Route-opt optimization summary            SETUP-COST    R2R-COST HOLD-COST LDRC-COST          AREA     LEAKAGE     INSTCNT            ELAPSE       MEM
Warning: Total 23 nets have their routing open (data: 23 nets, clock: 0 nets). This will cause problems for extraction, timing and post-route optimization. Some examples of open nets are listed below. (ROPT-004)
  core/n219
  core/n257
  core/n307
  core/n359
  core/n409
  core/n772
  core/n926
  core/n940
  core/n1094
  core/n1106
Warning: No default buffer/inverter available for voltage areas DEFAULT_VA. (OPT-014)
Warning: Can not find available buffer/inverter lib cells for the site unit. (OPT-016)
INFO: place.legalize.enable_advanced_legalizer not set - need to revert CLO


Route-opt optimization Phase 2 Iter  1        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167

Route-opt optimization Phase 3 Iter  1        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
INFO: New Levelizer turned on

INFO: New Levelizer turned on
Route-opt optimization Phase 4 Iter  1        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 4 Iter  2        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 4 Iter  3        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 4 Iter  4        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167


Route-opt optimization Phase 6 Iter  1        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 6 Iter  2        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 6 Iter  3        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 6 Iter  4        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 6 Iter  5        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 6 Iter  6        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 6 Iter  7        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 6 Iter  8        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 6 Iter  9        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 6 Iter 10        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 6 Iter 11        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 6 Iter 12        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 6 Iter 13        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167

Route-opt optimization Phase 7 Iter  1        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 7 Iter  2        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167

Disable clock slack update for ideal clocks
INFO: cellmap features:  adv-rules=(Y), pdc=(Y), clock-rules=(Y)
Route-opt optimization Phase 8 Iter  1        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
INFO: cellmap features:  adv-rules=(N), pdc=(Y), clock-rules=(Y)

Route-opt optimization Phase 9 Iter  1        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 9 Iter  2        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 9 Iter  3        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 9 Iter  4        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 9 Iter  5        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization summary            SETUP-COST    R2R-COST HOLD-COST LDRC-COST          AREA     LEAKAGE     INSTCNT            ELAPSE       MEM
Route-opt optimization Phase 9 Iter  6        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 9 Iter  7        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 9 Iter  8        139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167

Disable clock slack update for ideal clocks
INFO: cellmap features:  adv-rules=(Y), pdc=(Y), clock-rules=(Y)
Route-opt optimization Phase 10 Iter  1       139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
INFO: cellmap features:  adv-rules=(N), pdc=(Y), clock-rules=(Y)

Route-opt optimization Phase 11 Iter  1       139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
INFO: New Levelizer turned on
Route-opt optimization Phase 11 Iter  2       139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
INFO: New Levelizer turned on

Route-opt optimization Phase 12 Iter  1       139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167

Route-opt optimization Phase 13 Iter  1       139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167
Route-opt optimization Phase 13 Iter  2       139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167

Route-opt optimization complete               139.59      139.59     12.86       587     696592.56        8.01        2743              0.15      1167

Route-opt route preserve complete         CPU:   568 s (  0.16 hr )  ELAPSE:   537 s (  0.15 hr )  MEM-PEAK:  1167 MB
INFO: Sending timing info to router
Generating Timing information  
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Design  Scenario func1 (Mode func1 Corner func1)
Generating Timing information  ... Done
Information: The net parasitics of block vsdbabysoc are cleared. (TIM-123)
[End of Generating Timing Information] Elapsed real time: 0:00:00 
[End of Generating Timing Information] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
Information: The netlist change observers are enabled for incremental extraction. (TIM-127)
[icc2-lic Sun Jul 21 02:37:34 2024] Command 'legalize_placement' requires licenses
[icc2-lic Sun Jul 21 02:37:34 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:37:34 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:37:34 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:37:34 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:37:34 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:37:34 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:37:34 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:37:34 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:37:34 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:37:34 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:37:34 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:37:34 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:37:34 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:37:34 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:37:34 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:37:34 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:37:34 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:37:34 2024] Check-out of alternate set of keys directly with queueing was successful
nplDplc2Placer::setParam(effort,1)
nplDplc2Placer::setParam(debug,0)
nplDplc2Placer::setParam(site_check,2)
nplDplc2Placer::setParam(app_firm_wgt,0)
Warning: Routing direction of metal layer li1 is neither "horizontal" nor "vertical".  PDC checks will not be performed on this layer. (PDC-003)
Warning: Routing direction of metal layer fieldpoly is neither "horizontal" nor "vertical".  PDC checks will not be performed on this layer. (PDC-003)
PDC app_options settings =========
        place.legalize.enable_prerouted_net_check: 1
        place.legalize.num_tracks_for_access_check: 1
        place.legalize.use_eol_spacing_for_access_check: 0
        place.legalize.allow_touch_track_for_access_check: 1
        place.legalize.reduce_conservatism_in_eol_check: 0
        place.legalize.preroute_shape_merge_distance: 0.0
        place.legalize.enable_non_preferred_direction_span_check: 0

Layer met1: cached 207 shapes out of 10422 total shapes.
Layer met2: cached 0 shapes out of 11714 total shapes.
Cached 20324 vias out of 61638 total vias.

Legalizing Top Level Design vsdbabysoc ... 
Information: Initializing classic cellmap without advanced rules enabled and without PDC enabled
Information: The following app options are used in cellmap
        place.legalize.enable_color_aware_placement : false
        place.legalize.use_nll_query_cm : false
        place.legalize.enable_advanced_legalizer : false
        place.legalize.enable_prerouted_net_check : true
        place.legalize.enable_advanced_prerouted_net_check : false
        place.legalize.always_continue : true
        place.legalize.limit_legality_checks : false
        place.common.pnet_aware_density : 1.0000
        place.common.pnet_aware_min_width : 0.0000
        place.common.pnet_aware_layers : {}
        place.common.use_placement_model : false
        place.common.enable_advanced_placement_model : true
        cts.placement.cell_spacing_rule_style : maximum
Total 0.0200 seconds to build cellmap data
Information: Creating classic rule checker.
Warning: Routing direction of metal layer li1 is neither "horizontal" nor "vertical".  PDC checks will not be performed on this layer. (PDC-003)
Warning: Routing direction of metal layer fieldpoly is neither "horizontal" nor "vertical".  PDC checks will not be performed on this layer. (PDC-003)
=====> Processed 42 ref cells (4 fillers) from library

Bounds/Regions In This Design:
        Area    Num Cells  Exclusive Name
 (square um)
 2.32063e+06         2741        Yes DEFAULT_VA

Warning: max_legality_failures=5000 ignored.
        To use it, set limit_legality_checks to true.
Warning: max_legality_check_range=500 ignored.
        To use it, set limit_legality_checks to true.
Starting legalizer.
Warning: Exclusive bound 'DEFAULT' has no cells.
Optimizing Exclusive Bound 'DEFAULT_VA' (2/2)
    Done Exclusive Bound 'DEFAULT_VA' (2/2) (0 sec)
Legalization complete (1 total sec)

****************************************
  Report : Placement Attempts
  Site   : unit
****************************************

number of cells:                   2741
number of references:                42
number of site rows:                560
number of locations attempted:    46820
number of locations failed:           0  (0.0%)

Legality of references at locations:
0 references had failures.

Legality of references in rows:
0 references had row failures.

****************************************
  Report : Cell Displacements
****************************************

number of cells aggregated:        2741 (19933 total sites)
avg row height over cells:        2.720 um
rms cell displacement:            0.823 um ( 0.30 row height)
rms weighted cell displacement:   0.823 um ( 0.30 row height)
max cell displacement:            1.756 um ( 0.65 row height)
avg cell displacement:            0.728 um ( 0.27 row height)
avg weighted cell displacement:   0.728 um ( 0.27 row height)
number of cells moved:             2741
number of large displacements:        0
large displacement threshold:     3.000 row height

Displacements of worst 10 cells:
Cell: core/U172 (sky130_fd_sc_hd__clkinv_1)
  Input location: (792.022,770.285)
  Legal location: (792.06,772.04)
  Displacement:   1.756 um ( 0.65 row height)
Cell: core/U356 (sky130_fd_sc_hd__clkinv_1)
  Input location: (875.755,672.654)
  Legal location: (874.86,674.12)
  Displacement:   1.718 um ( 0.63 row height)
Cell: core/U1463 (sky130_fd_sc_hd__clkinv_1)
  Input location: (835.264,673.081)
  Legal location: (835.3,671.4)
  Displacement:   1.681 um ( 0.62 row height)
Cell: core/U235 (sky130_fd_sc_hd__clkinv_1)
  Input location: (800.614,751.434)
  Legal location: (799.42,750.28)
  Displacement:   1.661 um ( 0.61 row height)
Cell: core/U561 (sky130_fd_sc_hd__nor2_1)
  Input location: (777.929,751.728)
  Legal location: (778.72,750.28)
  Displacement:   1.650 um ( 0.61 row height)
Cell: core/U1322 (sky130_fd_sc_hd__a21oi_1)
  Input location: (835.174,776.005)
  Legal location: (836.22,774.76)
  Displacement:   1.626 um ( 0.60 row height)
Cell: core/U82 (sky130_fd_sc_hd__clkinv_1)
  Input location: (853.384,787.246)
  Legal location: (853.24,785.64)
  Displacement:   1.612 um ( 0.59 row height)
Cell: core/U1937 (sky130_fd_sc_hd__nor2_1)
  Input location: (815.449,814.01)
  Legal location: (815.52,815.56)
  Displacement:   1.552 um ( 0.57 row height)
Cell: core/U293 (sky130_fd_sc_hd__clkinv_1)
  Input location: (862.154,770.503)
  Legal location: (861.98,772.04)
  Displacement:   1.547 um ( 0.57 row height)
Cell: core/U1504 (sky130_fd_sc_hd__clkinv_1)
  Input location: (875.762,621.259)
  Legal location: (875.78,619.72)
  Displacement:   1.539 um ( 0.57 row height)

Information: Extraction observers are detached as design net change threshold is reached.
Legalization succeeded.
Total Legalizer CPU: 0.870
Total Legalizer Wall Time: 0.871
----------------------------------------------------------------

Route-opt legalization complete           CPU:   569 s (  0.16 hr )  ELAPSE:   538 s (  0.15 hr )  MEM-PEAK:  1167 MB
INFO: Router Max Iterations set to : 5 
Warning: Layer li1 does not have a preferred direction, assigning to vertical. (ZRT-025)
Cell Min-Routing-Layer = met1
Cell Max-Routing-Layer = met5
Turn off antenna since no rule is specified
Information: Option route.detail.force_end_on_preferred_grid will be ignored since none of the layers have preferred grid. (ZRT-703)
Warning: Ignore pin (cell avsdpll, pin VDD) on layer nwell. (ZRT-030)
Warning: Ignore pin (cell avsdpll, pin VDD#2_1) on layer nwell. (ZRT-030)
Note - message 'ZRT-030' limit (10) exceeded. Remainder will be suppressed.
Warning: Ignore 2 top cell ports with no pins. (ZRT-027)
Info: number of net_type_blockage: 0 
Information: Via ladder engine would be activated for pattern must join connection in certain commands. Please refer to man-page for the command list. (ZRT-619)
Via on layer (via4) needs more than one tracks
Warning: Layer met4 pitch 0.920 may be too small: wire/via-down 0.615, wire/via-up 1.040. (ZRT-026)
Transition layer name: met4(4)
Information: When applicable Min-max layer allow_pin_connection mode will allow paths of length 5.75 outside the layer range. (ZRT-707)
Information: When applicable Min-max layer allow_pin_connection mode will allow paths of length 5.75 outside the layer range on clock nets. (ZRT-718)
Information: When applicable layer based tapering will taper up to 0.00 in distance from the pin. (ZRT-706)
Warning: Master cell avsddac has duplicated redundant library pin shapes at {0.000 0.000} {0.170 0.170} on layer li1. (ZRT-625)
Warning: Master cell avsddac has duplicated redundant library pin shapes at {0.000 0.000} {0.170 0.170} on layer li1. (ZRT-625)
[ECO: DbIn With Extraction] Elapsed real time: 0:00:00 
[ECO: DbIn With Extraction] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[ECO: DbIn With Extraction] Stage (MB): Used   17  Alloctr   18  Proc   16 
[ECO: DbIn With Extraction] Total (MB): Used   20  Alloctr   22  Proc 7800 
[ECO: Analysis] Elapsed real time: 0:00:00 
[ECO: Analysis] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[ECO: Analysis] Stage (MB): Used   17  Alloctr   18  Proc   16 
[ECO: Analysis] Total (MB): Used   20  Alloctr   22  Proc 7800 
Num of eco nets = 2765
Num of open eco nets = 2495
[ECO: Init] Elapsed real time: 0:00:00 
[ECO: Init] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[ECO: Init] Stage (MB): Used   19  Alloctr   20  Proc   16 
[ECO: Init] Total (MB): Used   22  Alloctr   24  Proc 7800 
Start Global Route ... 
[Init] Elapsed real time: 0:00:00 
[Init] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[Init] Stage (MB): Used    0  Alloctr    0  Proc    0 
[Init] Total (MB): Used   26  Alloctr   28  Proc 7800 
Info: route.global.delay_based_route_rejection is 16.
Printing options for 'route.common.*'

Printing options for 'route.global.*'
global.timing_driven                                    :        true                

Begin global routing.
Loading timing information to the router from design
Timing information loaded to the router.
[End of Loading Timing] Elapsed real time: 0:00:00 
[End of Loading Timing] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
Constructing data structure ...
Design statistics:
Design Bounding Box (0.00um,0.00um,1533.52um,1533.20um)
Number of routing layers = 6
layer li1, dir Ver, min width = 0.17um, min space = 0.17um pitch = 0.46um
layer met1, dir Hor, min width = 0.14um, min space = 0.14um pitch = 0.34um
layer met2, dir Ver, min width = 0.14um, min space = 0.14um pitch = 0.46um
layer met3, dir Hor, min width = 0.3um, min space = 0.3um pitch = 0.68um
layer met4, dir Ver, min width = 0.3um, min space = 0.3um pitch = 0.92um
layer met5, dir Hor, min width = 1.6um, min space = 1.6um pitch = 3.4um
Current Stage stats:
[End of Build Tech Data] Elapsed real time: 0:00:00 
[End of Build Tech Data] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[End of Build Tech Data] Stage (MB): Used    4  Alloctr    4  Proc    0 
[End of Build Tech Data] Total (MB): Used   35  Alloctr   36  Proc 7800 
Warning: No existing global route is available for reuse in incremental global routing. (ZRT-586)
Net statistics:
Total number of nets     = 2765
Number of nets to route  = 2495
2472 nets are partially connected,
 of which 2472 are detail routed and 0 are global routed.
270 nets are fully connected,
 of which 270 are detail routed and 0 are global routed.
Net length statistics: 
Net Count(Ignore Fully Rted) 2495, Total Half Perimeter Wire Length (HPWL) 81202 microns
HPWL   0 ~   50 microns: Net Count     2239     Total HPWL        41589 microns
HPWL  50 ~  100 microns: Net Count      130     Total HPWL         9069 microns
HPWL 100 ~  200 microns: Net Count       55     Total HPWL         7888 microns
HPWL 200 ~  300 microns: Net Count       54     Total HPWL        13726 microns
HPWL 300 ~  400 microns: Net Count       13     Total HPWL         4181 microns
HPWL 400 ~  500 microns: Net Count        0     Total HPWL            0 microns
HPWL 500 ~  600 microns: Net Count        0     Total HPWL            0 microns
HPWL 600 ~  700 microns: Net Count        0     Total HPWL            0 microns
HPWL 700 ~  800 microns: Net Count        0     Total HPWL            0 microns
HPWL 800 ~  900 microns: Net Count        0     Total HPWL            0 microns
HPWL 900 ~ 1000 microns: Net Count        1     Total HPWL          958 microns
HPWL     > 1000 microns: Net Count        3     Total HPWL         3790 microns
Current Stage stats:
[End of Build All Nets] Elapsed real time: 0:00:00 
[End of Build All Nets] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[End of Build All Nets] Stage (MB): Used    1  Alloctr    1  Proc    0 
[End of Build All Nets] Total (MB): Used   36  Alloctr   38  Proc 7800 
Number of partitions: 1 (1 x 1)
Size of partitions: 564 gCells x 564 gCells
Average gCell capacity  4.13     on layer (1)    li1
Average gCell capacity  4.95     on layer (2)    met1
Average gCell capacity  4.07     on layer (3)    met2
Average gCell capacity  2.75     on layer (4)    met3
Average gCell capacity  1.60     on layer (5)    met4
Average gCell capacity  0.32     on layer (6)    met5
Average number of tracks per gCell 5.91  on layer (1)    li1
Average number of tracks per gCell 8.00  on layer (2)    met1
Average number of tracks per gCell 5.91  on layer (3)    met2
Average number of tracks per gCell 4.00  on layer (4)    met3
Average number of tracks per gCell 2.96  on layer (5)    met4
Average number of tracks per gCell 0.80  on layer (6)    met5
Number of gCells = 1908576
Current Stage stats:
[End of Build Congestion Map] Elapsed real time: 0:00:01 
[End of Build Congestion Map] Elapsed cpu  time: sys=0:00:00 usr=0:00:01 total=0:00:01
[End of Build Congestion Map] Stage (MB): Used   27  Alloctr   27  Proc    0 
[End of Build Congestion Map] Total (MB): Used   64  Alloctr   66  Proc 7800 
Number of partitions: 1 (1 x 1)
Size of partitions: 564 gCells x 564 gCells
Current Stage stats:
[End of Add Nets Demand] Elapsed real time: 0:00:00 
[End of Add Nets Demand] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[End of Add Nets Demand] Stage (MB): Used    0  Alloctr    0  Proc    0 
[End of Add Nets Demand] Total (MB): Used   64  Alloctr   66  Proc 7800 
Number of user frozen nets = 0
Timing criticality report: total 257 (9.29)% critical nets.
   Number of criticality 2 nets = 15 (0.54)%
   Number of criticality 3 nets = 16 (0.58)%
   Number of criticality 4 nets = 21 (0.76)%
   Number of criticality 5 nets = 72 (2.60)%
   Number of criticality 6 nets = 19 (0.69)%
   Number of criticality 7 nets = 114 (4.12)%
Information: RC layer preference is turned on for this design. (ZRT-613)
Total stats:
[End of Build Data] Elapsed real time: 0:00:01 
[End of Build Data] Elapsed cpu  time: sys=0:00:00 usr=0:00:01 total=0:00:01
[End of Build Data] Stage (MB): Used   33  Alloctr   34  Proc    0 
[End of Build Data] Total (MB): Used   64  Alloctr   66  Proc 7800 
Number of partitions: 1 (1 x 1)
Size of partitions: 564 gCells x 564 gCells
Current Stage stats:
[End of Blocked Pin Detection] Elapsed real time: 0:00:01 
[End of Blocked Pin Detection] Elapsed cpu  time: sys=0:00:00 usr=0:00:01 total=0:00:01
[End of Blocked Pin Detection] Stage (MB): Used  176  Alloctr  176  Proc    0 
[End of Blocked Pin Detection] Total (MB): Used  240  Alloctr  242  Proc 7800 
Information: Using 8 threads for routing. (ZRT-444)
Information: Multiple gcell levels ON
Information: Buffer distance is estimated to be ~4656.0000um (1711 gCells)

Start GR phase 0
Number of partitions: 1 (1 x 1)
Size of partitions: 564 gCells x 564 gCells
[rtAllBotParts] Elapsed real time: 0:00:00 
[rtAllBotParts] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[rtTop] Elapsed real time: 0:00:00 
[rtTop] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
Current Stage stats:
[End of Initial Routing] Elapsed real time: 0:00:00 
[End of Initial Routing] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[End of Initial Routing] Stage (MB): Used    0  Alloctr    1  Proc    0 
[End of Initial Routing] Total (MB): Used  241  Alloctr  243  Proc 7800 
Initial. Routing result:
Initial. Both Dirs: Dmd-Cap  =  3376 Max = 8 GRCs =  1770 (0.28%)
Initial. H routing: Dmd-Cap  =  2236 Max = 7 (GRCs =  2) GRCs =  1127 (0.35%)
Initial. V routing: Dmd-Cap  =  1140 Max = 8 (GRCs =  1) GRCs =   643 (0.20%)
Initial. Both Dirs: Overflow =  3017 Max = 7 GRCs =  3673 (0.58%)
Initial. H routing: Overflow =  1782 Max = 5 (GRCs =  1) GRCs =  2341 (0.74%)
Initial. V routing: Overflow =  1234 Max = 7 (GRCs =  3) GRCs =  1332 (0.42%)
Initial. li1        Overflow =    82 Max = 2 (GRCs =  1) GRCs =    81 (0.03%)
Initial. met1       Overflow =  1206 Max = 5 (GRCs =  1) GRCs =  1379 (0.43%)
Initial. met2       Overflow =  1037 Max = 7 (GRCs =  3) GRCs =  1141 (0.36%)
Initial. met3       Overflow =   576 Max = 4 (GRCs =  1) GRCs =   962 (0.30%)
Initial. met4       Overflow =   115 Max = 2 (GRCs =  5) GRCs =   110 (0.03%)
Initial. met5       Overflow =     0 Max = 0 (GRCs =  0) GRCs =     0 (0.00%)


Overflow over macro areas

Initial. Both Dirs: Overflow =     0 Max =  0 GRCs =     0 (0.00%)
Initial. H routing: Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
Initial. V routing: Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
Initial. li1        Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
Initial. met1       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
Initial. met2       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
Initial. met3       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
Initial. met4       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
Initial. met5       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)


Density distribution:
Layer    0.0  0.1  0.2  0.3  0.4  0.5  0.6  0.7  0.8  0.9  1.0  1.1  1.2  > 1.2 
li1      99.8 0.02 0.01 0.01 0.00 0.02 0.00 0.00 0.00 0.00 0.03 0.00 0.00 0.03
met1     97.8 0.49 0.00 0.26 0.00 0.25 0.24 0.00 0.22 0.00 0.42 0.00 0.10 0.15
met2     96.8 0.95 0.01 0.47 0.00 0.38 0.37 0.02 0.39 0.00 0.34 0.13 0.04 0.08
met3     96.7 0.00 1.18 0.20 0.00 0.37 0.10 0.51 0.00 0.00 0.74 0.00 0.15 0.07
met4     98.4 0.00 0.00 0.53 0.00 0.14 0.44 0.00 0.00 0.00 0.39 0.00 0.00 0.04
met5     99.2 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.79 0.00 0.00 0.00
Total    98.1 0.24 0.19 0.24 0.00 0.19 0.19 0.08 0.10 0.00 0.45 0.02 0.05 0.06


Initial. Total Wire Length = 2209.32
Initial. Layer li1 wire length = 4.74
Initial. Layer met1 wire length = 833.36
Initial. Layer met2 wire length = 1342.81
Initial. Layer met3 wire length = 18.48
Initial. Layer met4 wire length = 9.92
Initial. Layer met5 wire length = 0.00
Initial. Total Number of Contacts = 2890
Initial. Via L1M1_PR count = 1527
Initial. Via M1M2_PR count = 1302
Initial. Via M2M3_PR count = 52
Initial. Via M3M4_PR count = 9
Initial. Via M4M5_PR count = 0
Initial. completed.

Start GR phase 1
Sun Jul 21 02:37:39 2024
Number of partitions: 1 (1 x 1)
Size of partitions: 564 gCells x 564 gCells
[rtAllParts] Elapsed real time: 0:00:00 
[rtAllParts] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
Current Stage stats:
[End of Phase1 Routing] Elapsed real time: 0:00:00 
[End of Phase1 Routing] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[End of Phase1 Routing] Stage (MB): Used    0  Alloctr    0  Proc    0 
[End of Phase1 Routing] Total (MB): Used  241  Alloctr  243  Proc 7800 
phase1. Routing result:
phase1. Both Dirs: Dmd-Cap  =  3345 Max = 8 GRCs =  1769 (0.28%)
phase1. H routing: Dmd-Cap  =  2244 Max = 7 (GRCs =  2) GRCs =  1134 (0.36%)
phase1. V routing: Dmd-Cap  =  1101 Max = 8 (GRCs =  1) GRCs =   635 (0.20%)
phase1. Both Dirs: Overflow =  2951 Max = 7 GRCs =  3679 (0.58%)
phase1. H routing: Overflow =  1779 Max = 5 (GRCs =  2) GRCs =  2356 (0.74%)
phase1. V routing: Overflow =  1171 Max = 7 (GRCs =  3) GRCs =  1323 (0.42%)
phase1. li1        Overflow =    82 Max = 2 (GRCs =  1) GRCs =    81 (0.03%)
phase1. met1       Overflow =  1205 Max = 5 (GRCs =  2) GRCs =  1391 (0.44%)
phase1. met2       Overflow =   974 Max = 7 (GRCs =  3) GRCs =  1132 (0.36%)
phase1. met3       Overflow =   574 Max = 4 (GRCs =  1) GRCs =   965 (0.30%)
phase1. met4       Overflow =   115 Max = 2 (GRCs =  5) GRCs =   110 (0.03%)
phase1. met5       Overflow =     0 Max = 0 (GRCs =  0) GRCs =     0 (0.00%)


Overflow over macro areas

phase1. Both Dirs: Overflow =     0 Max =  0 GRCs =     0 (0.00%)
phase1. H routing: Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase1. V routing: Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase1. li1        Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase1. met1       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase1. met2       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase1. met3       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase1. met4       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase1. met5       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)


Density distribution:
Layer    0.0  0.1  0.2  0.3  0.4  0.5  0.6  0.7  0.8  0.9  1.0  1.1  1.2  > 1.2 
li1      99.8 0.02 0.01 0.01 0.00 0.02 0.00 0.00 0.00 0.00 0.03 0.00 0.00 0.03
met1     97.8 0.49 0.00 0.26 0.00 0.25 0.24 0.00 0.22 0.00 0.43 0.00 0.11 0.15
met2     96.8 0.95 0.01 0.48 0.00 0.38 0.37 0.02 0.39 0.00 0.36 0.13 0.03 0.08
met3     96.7 0.00 1.18 0.20 0.00 0.37 0.10 0.50 0.00 0.00 0.74 0.00 0.14 0.07
met4     98.4 0.00 0.00 0.53 0.00 0.14 0.43 0.00 0.00 0.00 0.39 0.00 0.00 0.04
met5     99.2 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.79 0.00 0.00 0.00
Total    98.1 0.24 0.19 0.24 0.00 0.19 0.19 0.08 0.10 0.00 0.45 0.02 0.05 0.06


phase1. Total Wire Length = 2453.44
phase1. Layer li1 wire length = 4.74
phase1. Layer met1 wire length = 996.31
phase1. Layer met2 wire length = 1377.20
phase1. Layer met3 wire length = 59.39
phase1. Layer met4 wire length = 15.79
phase1. Layer met5 wire length = 0.00
phase1. Total Number of Contacts = 2932
phase1. Via L1M1_PR count = 1528
phase1. Via M1M2_PR count = 1314
phase1. Via M2M3_PR count = 74
phase1. Via M3M4_PR count = 16
phase1. Via M4M5_PR count = 0
phase1. completed.

Start GR phase 2
Sun Jul 21 02:37:39 2024
Number of partitions: 1 (1 x 1)
Size of partitions: 564 gCells x 564 gCells
[rtAllParts] Elapsed real time: 0:00:00 
[rtAllParts] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
Current Stage stats:
[End of Phase2 Routing] Elapsed real time: 0:00:00 
[End of Phase2 Routing] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[End of Phase2 Routing] Stage (MB): Used    0  Alloctr    0  Proc    0 
[End of Phase2 Routing] Total (MB): Used  241  Alloctr  244  Proc 7800 
phase2. Routing result:
phase2. Both Dirs: Dmd-Cap  =  3365 Max = 8 GRCs =  1794 (0.28%)
phase2. H routing: Dmd-Cap  =  2274 Max = 7 (GRCs =  2) GRCs =  1159 (0.36%)
phase2. V routing: Dmd-Cap  =  1091 Max = 8 (GRCs =  1) GRCs =   635 (0.20%)
phase2. Both Dirs: Overflow =  2933 Max = 7 GRCs =  3704 (0.58%)
phase2. H routing: Overflow =  1786 Max = 5 (GRCs =  2) GRCs =  2380 (0.75%)
phase2. V routing: Overflow =  1147 Max = 7 (GRCs =  3) GRCs =  1324 (0.42%)
phase2. li1        Overflow =    82 Max = 2 (GRCs =  1) GRCs =    81 (0.03%)
phase2. met1       Overflow =  1210 Max = 5 (GRCs =  2) GRCs =  1410 (0.44%)
phase2. met2       Overflow =   950 Max = 7 (GRCs =  3) GRCs =  1133 (0.36%)
phase2. met3       Overflow =   575 Max = 4 (GRCs =  1) GRCs =   970 (0.30%)
phase2. met4       Overflow =   115 Max = 2 (GRCs =  5) GRCs =   110 (0.03%)
phase2. met5       Overflow =     0 Max = 0 (GRCs =  0) GRCs =     0 (0.00%)


Overflow over macro areas

phase2. Both Dirs: Overflow =     0 Max =  0 GRCs =     0 (0.00%)
phase2. H routing: Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase2. V routing: Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase2. li1        Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase2. met1       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase2. met2       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase2. met3       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase2. met4       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)
phase2. met5       Overflow =     0 Max =  0 (GRCs =  0) GRCs =     0 (0.00%)


Density distribution:
Layer    0.0  0.1  0.2  0.3  0.4  0.5  0.6  0.7  0.8  0.9  1.0  1.1  1.2  > 1.2 
li1      99.8 0.02 0.01 0.01 0.00 0.02 0.00 0.00 0.00 0.00 0.03 0.00 0.00 0.03
met1     97.8 0.49 0.00 0.26 0.00 0.25 0.24 0.00 0.22 0.00 0.43 0.00 0.11 0.15
met2     96.8 0.95 0.01 0.47 0.00 0.38 0.37 0.02 0.39 0.00 0.36 0.13 0.03 0.07
met3     96.7 0.00 1.18 0.20 0.00 0.37 0.10 0.50 0.00 0.00 0.75 0.00 0.15 0.07
met4     98.4 0.00 0.00 0.53 0.00 0.14 0.43 0.00 0.00 0.00 0.39 0.00 0.00 0.04
met5     99.2 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.79 0.00 0.00 0.00
Total    98.1 0.24 0.19 0.24 0.00 0.19 0.19 0.08 0.10 0.00 0.46 0.02 0.05 0.06


phase2. Total Wire Length = 2767.61
phase2. Layer li1 wire length = 11.80
phase2. Layer met1 wire length = 1209.83
phase2. Layer met2 wire length = 1417.20
phase2. Layer met3 wire length = 93.41
phase2. Layer met4 wire length = 35.37
phase2. Layer met5 wire length = 0.00
phase2. Total Number of Contacts = 3001
phase2. Via L1M1_PR count = 1527
phase2. Via M1M2_PR count = 1357
phase2. Via M2M3_PR count = 93
phase2. Via M3M4_PR count = 24
phase2. Via M4M5_PR count = 0
phase2. completed.
[End of Whole Chip Routing] Elapsed real time: 0:00:03 
[End of Whole Chip Routing] Elapsed cpu  time: sys=0:00:00 usr=0:00:04 total=0:00:04
[End of Whole Chip Routing] Stage (MB): Used  210  Alloctr  211  Proc    0 
[End of Whole Chip Routing] Total (MB): Used  241  Alloctr  243  Proc 7800 

Congestion utilization per direction:
Average vertical track utilization   =  1.12 %
Peak    vertical track utilization   = 216.67 %
Average horizontal track utilization =  1.46 %
Peak    horizontal track utilization = 180.00 %

[End of Global Routing] Elapsed real time: 0:00:04 
[End of Global Routing] Elapsed cpu  time: sys=0:00:00 usr=0:00:04 total=0:00:04
[End of Global Routing] Stage (MB): Used  188  Alloctr  189  Proc    0 
[End of Global Routing] Total (MB): Used  219  Alloctr  221  Proc 7800 
Updating congestion ...
Updating congestion ...
[End of dbOut] Elapsed real time: 0:00:00 
[End of dbOut] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[End of dbOut] Stage (MB): Used   -8  Alloctr   -8  Proc    0 
[End of dbOut] Total (MB): Used   60  Alloctr   61  Proc 7800 
[ECO: GR] Elapsed real time: 0:00:04 
[ECO: GR] Elapsed cpu  time: sys=0:00:00 usr=0:00:04 total=0:00:04
[ECO: GR] Stage (MB): Used   39  Alloctr   39  Proc    0 
[ECO: GR] Total (MB): Used   60  Alloctr   61  Proc 7800 

Start track assignment

Printing options for 'route.common.*'

Printing options for 'route.track.*'

Information: Using 8 threads for routing. (ZRT-444)

[Track Assign: TA init] Elapsed real time: 0:00:00 
[Track Assign: TA init] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[Track Assign: TA init] Stage (MB): Used    3  Alloctr    3  Proc    0 
[Track Assign: TA init] Total (MB): Used   27  Alloctr   28  Proc 7800 

Start initial assignment
Routed partition 1/4        
Routed partition 2/4        
Routed partition 3/4        
Routed partition 4/4        

Number of wires with overlap after iteration 0 = 14889 of 22322


[Track Assign: Iteration 0] Elapsed real time: 0:00:00 
[Track Assign: Iteration 0] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[Track Assign: Iteration 0] Stage (MB): Used    3  Alloctr    3  Proc    0 
[Track Assign: Iteration 0] Total (MB): Used   27  Alloctr   29  Proc 7800 

Reroute to fix overlaps (iter = 1)
Routed partition 1/4        
Routed partition 2/4        
Routed partition 3/4        
Routed partition 4/4        

[Track Assign: Iteration 1] Elapsed real time: 0:00:01 
[Track Assign: Iteration 1] Elapsed cpu  time: sys=0:00:00 usr=0:00:01 total=0:00:01
[Track Assign: Iteration 1] Stage (MB): Used    3  Alloctr    3  Proc    0 
[Track Assign: Iteration 1] Total (MB): Used   27  Alloctr   29  Proc 7800 

Number of wires with overlap after iteration 1 = 8681 of 17019


Wire length and via report:
---------------------------
Number of li1 wires: 4829                 : 0
Number of met1 wires: 8503               L1M1_PR: 7641
Number of met2 wires: 3573               M1M2_PR: 5881
Number of met3 wires: 103                M2M3_PR: 125
Number of met4 wires: 11                 M3M4_PR: 22
Number of met5 wires: 0                  M4M5_PR: 0
Total number of wires: 17019             vias: 13669

Total li1 wire length: 2003.4
Total met1 wire length: 5220.2
Total met2 wire length: 4096.1
Total met3 wire length: 142.9
Total met4 wire length: 36.4
Total met5 wire length: 0.0
Total wire length: 11498.9

Longest li1 wire length: 3.2
Longest met1 wire length: 15.6
Longest met2 wire length: 8.2
Longest met3 wire length: 7.8
Longest met4 wire length: 7.5
Longest met5 wire length: 0.0

Info: numNewViaInsted = 0 

[Track Assign: Done] Elapsed real time: 0:00:01 
[Track Assign: Done] Elapsed cpu  time: sys=0:00:00 usr=0:00:02 total=0:00:02
[Track Assign: Done] Stage (MB): Used    0  Alloctr    0  Proc    0 
[Track Assign: Done] Total (MB): Used   23  Alloctr   25  Proc 7800 
[ECO: CDR] Elapsed real time: 0:00:06 
[ECO: CDR] Elapsed cpu  time: sys=0:00:00 usr=0:00:07 total=0:00:07
[ECO: CDR] Stage (MB): Used   20  Alloctr   21  Proc   16 
[ECO: CDR] Total (MB): Used   23  Alloctr   25  Proc 7800 
Printing options for 'route.common.*'

Printing options for 'route.detail.*'
detail.eco_route_use_soft_spacing_for_timing_optimization:       false               

Printing options for 'route.auto_via_ladder.*'

Total number of nets = 2765, of which 0 are not extracted
Total number of open nets = 0, of which 0 are frozen
Information: Using 8 threads for routing. (ZRT-444)
Region based early termination has 3249 candidate regions.
Start DR iteration 0: uniform partition
Routed  2675/3249 Partitions, Violations =      0
Routed  2676/3249 Partitions, Violations =      0
Routed  2677/3249 Partitions, Violations =      0
Routed  2678/3249 Partitions, Violations =      0
Routed  2679/3249 Partitions, Violations =      0
Routed  2680/3249 Partitions, Violations =      0
Routed  2681/3249 Partitions, Violations =      0
Routed  2682/3249 Partitions, Violations =      0
Routed  2683/3249 Partitions, Violations =      0
Routed  2684/3249 Partitions, Violations =      0
Routed  2685/3249 Partitions, Violations =      0
Routed  2686/3249 Partitions, Violations =      0
Routed  2687/3249 Partitions, Violations =      0
Routed  2688/3249 Partitions, Violations =      0
Routed  2689/3249 Partitions, Violations =      0
Routed  2690/3249 Partitions, Violations =      0
Routed  2691/3249 Partitions, Violations =      0
Routed  2693/3249 Partitions, Violations =      0
Routed  2693/3249 Partitions, Violations =      0
Routed  2694/3249 Partitions, Violations =      0
Routed  2695/3249 Partitions, Violations =      0
Routed  2696/3249 Partitions, Violations =      0
Routed  2697/3249 Partitions, Violations =      0
Routed  2698/3249 Partitions, Violations =      0
Routed  2699/3249 Partitions, Violations =      18
Routed  2700/3249 Partitions, Violations =      18
Routed  2701/3249 Partitions, Violations =      18
Routed  2702/3249 Partitions, Violations =      18
Routed  2703/3249 Partitions, Violations =      18
Routed  2704/3249 Partitions, Violations =      18
Routed  2705/3249 Partitions, Violations =      18
Routed  2706/3249 Partitions, Violations =      18
Routed  2707/3249 Partitions, Violations =      18
Routed  2708/3249 Partitions, Violations =      18
Routed  2709/3249 Partitions, Violations =      18
Routed  2710/3249 Partitions, Violations =      18
Routed  2711/3249 Partitions, Violations =      18
Routed  2712/3249 Partitions, Violations =      18
Routed  2713/3249 Partitions, Violations =      18
Routed  2714/3249 Partitions, Violations =      18
Routed  2715/3249 Partitions, Violations =      18
Routed  2716/3249 Partitions, Violations =      18
Routed  2717/3249 Partitions, Violations =      18
Routed  2718/3249 Partitions, Violations =      18
Routed  2719/3249 Partitions, Violations =      18
Routed  2720/3249 Partitions, Violations =      18
Routed  2721/3249 Partitions, Violations =      18
Routed  2722/3249 Partitions, Violations =      18
Routed  2723/3249 Partitions, Violations =      18
Routed  2724/3249 Partitions, Violations =      18
Routed  2725/3249 Partitions, Violations =      20
Routed  2726/3249 Partitions, Violations =      20
Routed  2727/3249 Partitions, Violations =      20
Routed  2728/3249 Partitions, Violations =      20
Routed  2729/3249 Partitions, Violations =      20
Routed  2730/3249 Partitions, Violations =      20
Routed  2731/3249 Partitions, Violations =      20
Routed  2732/3249 Partitions, Violations =      20
Routed  2733/3249 Partitions, Violations =      20
Routed  2734/3249 Partitions, Violations =      20
Routed  2735/3249 Partitions, Violations =      20
Routed  2736/3249 Partitions, Violations =      20
Routed  2737/3249 Partitions, Violations =      20
Routed  2738/3249 Partitions, Violations =      20
Routed  2739/3249 Partitions, Violations =      20
Routed  2740/3249 Partitions, Violations =      20
Routed  2741/3249 Partitions, Violations =      20
Routed  2742/3249 Partitions, Violations =      20
Routed  2743/3249 Partitions, Violations =      20
Routed  2744/3249 Partitions, Violations =      20
Routed  2745/3249 Partitions, Violations =      20
Routed  2746/3249 Partitions, Violations =      20
Routed  2747/3249 Partitions, Violations =      20
Routed  2748/3249 Partitions, Violations =      20
Routed  2749/3249 Partitions, Violations =      20
Routed  2750/3249 Partitions, Violations =      20
Routed  2751/3249 Partitions, Violations =      20
Routed  2752/3249 Partitions, Violations =      20
Routed  2753/3249 Partitions, Violations =      20
Routed  2754/3249 Partitions, Violations =      20
Routed  2755/3249 Partitions, Violations =      20
Routed  2756/3249 Partitions, Violations =      20
Routed  2757/3249 Partitions, Violations =      20
Routed  2758/3249 Partitions, Violations =      20
Routed  2759/3249 Partitions, Violations =      20
Routed  2760/3249 Partitions, Violations =      20
Routed  2761/3249 Partitions, Violations =      20
Routed  2762/3249 Partitions, Violations =      20
Routed  2763/3249 Partitions, Violations =      20
Routed  2764/3249 Partitions, Violations =      20
Routed  2765/3249 Partitions, Violations =      20
Routed  2766/3249 Partitions, Violations =      20
Routed  2767/3249 Partitions, Violations =      20
Routed  2768/3249 Partitions, Violations =      20
Routed  2769/3249 Partitions, Violations =      20
Routed  2770/3249 Partitions, Violations =      20
Routed  2771/3249 Partitions, Violations =      20
Routed  2772/3249 Partitions, Violations =      20
Routed  2773/3249 Partitions, Violations =      20
Routed  2774/3249 Partitions, Violations =      20
Routed  2775/3249 Partitions, Violations =      20
Routed  2776/3249 Partitions, Violations =      20
Routed  2777/3249 Partitions, Violations =      20
Routed  2778/3249 Partitions, Violations =      20
Routed  2779/3249 Partitions, Violations =      20
Routed  2781/3249 Partitions, Violations =      20
Routed  2781/3249 Partitions, Violations =      20
Routed  2782/3249 Partitions, Violations =      20
Routed  2783/3249 Partitions, Violations =      20
Routed  2784/3249 Partitions, Violations =      20
Routed  2785/3249 Partitions, Violations =      20
Routed  2786/3249 Partitions, Violations =      20
Routed  2787/3249 Partitions, Violations =      20
Routed  2788/3249 Partitions, Violations =      114
Routed  2789/3249 Partitions, Violations =      114
Routed  2790/3249 Partitions, Violations =      114
Routed  2791/3249 Partitions, Violations =      114
Routed  2792/3249 Partitions, Violations =      114
Routed  2793/3249 Partitions, Violations =      114
Routed  2794/3249 Partitions, Violations =      114
Routed  2795/3249 Partitions, Violations =      114
Routed  2796/3249 Partitions, Violations =      114
Routed  2797/3249 Partitions, Violations =      114
Routed  2798/3249 Partitions, Violations =      114
Routed  2799/3249 Partitions, Violations =      114
Routed  2800/3249 Partitions, Violations =      114
Routed  2801/3249 Partitions, Violations =      114
Routed  2802/3249 Partitions, Violations =      114
Routed  2803/3249 Partitions, Violations =      61
Routed  2804/3249 Partitions, Violations =      61
Routed  2805/3249 Partitions, Violations =      61
Routed  2806/3249 Partitions, Violations =      61
Routed  2807/3249 Partitions, Violations =      61
Routed  2808/3249 Partitions, Violations =      61
Routed  2809/3249 Partitions, Violations =      61
Routed  2810/3249 Partitions, Violations =      61
Routed  2811/3249 Partitions, Violations =      61
Routed  2812/3249 Partitions, Violations =      61
Routed  2813/3249 Partitions, Violations =      61
Routed  2814/3249 Partitions, Violations =      61
Routed  2815/3249 Partitions, Violations =      61
Routed  2816/3249 Partitions, Violations =      61
Routed  2817/3249 Partitions, Violations =      61
Routed  2818/3249 Partitions, Violations =      61
Routed  2819/3249 Partitions, Violations =      61
Routed  2820/3249 Partitions, Violations =      61
Routed  2821/3249 Partitions, Violations =      61
Routed  2822/3249 Partitions, Violations =      61
Routed  2823/3249 Partitions, Violations =      61
Routed  2824/3249 Partitions, Violations =      61
Routed  2825/3249 Partitions, Violations =      61
Routed  2826/3249 Partitions, Violations =      61
Routed  2827/3249 Partitions, Violations =      61
Routed  2828/3249 Partitions, Violations =      61
Routed  2829/3249 Partitions, Violations =      61
Routed  2830/3249 Partitions, Violations =      61
Routed  2832/3249 Partitions, Violations =      61
Routed  2832/3249 Partitions, Violations =      61
Routed  2833/3249 Partitions, Violations =      61
Routed  2834/3249 Partitions, Violations =      61
Routed  2835/3249 Partitions, Violations =      61
Routed  2836/3249 Partitions, Violations =      61
Routed  2837/3249 Partitions, Violations =      61
Routed  2838/3249 Partitions, Violations =      61
Routed  2839/3249 Partitions, Violations =      61
Routed  2840/3249 Partitions, Violations =      61
Routed  2841/3249 Partitions, Violations =      61
Routed  2842/3249 Partitions, Violations =      61
Routed  2843/3249 Partitions, Violations =      61
Routed  2844/3249 Partitions, Violations =      61
Routed  2845/3249 Partitions, Violations =      155
Routed  2846/3249 Partitions, Violations =      155
Routed  2847/3249 Partitions, Violations =      155
Routed  2848/3249 Partitions, Violations =      155
Routed  2849/3249 Partitions, Violations =      155
Routed  2851/3249 Partitions, Violations =      155
Routed  2851/3249 Partitions, Violations =      155
Routed  2852/3249 Partitions, Violations =      155
Routed  2853/3249 Partitions, Violations =      155
Routed  2864/3249 Partitions, Violations =      161
Routed  2880/3249 Partitions, Violations =      155
Routed  2896/3249 Partitions, Violations =      174
Routed  2913/3249 Partitions, Violations =      174
Routed  2928/3249 Partitions, Violations =      174
Routed  2944/3249 Partitions, Violations =      174
Routed  2960/3249 Partitions, Violations =      174
Routed  2976/3249 Partitions, Violations =      220
Routed  2992/3249 Partitions, Violations =      349
Routed  3008/3249 Partitions, Violations =      349
Routed  3024/3249 Partitions, Violations =      332
Routed  3040/3249 Partitions, Violations =      332
Routed  3056/3249 Partitions, Violations =      548
Routed  3072/3249 Partitions, Violations =      548
Routed  3088/3249 Partitions, Violations =      594
Routed  3104/3249 Partitions, Violations =      594
Routed  3120/3249 Partitions, Violations =      877
Routed  3136/3249 Partitions, Violations =      913
Routed  3152/3249 Partitions, Violations =      916
Routed  3168/3249 Partitions, Violations =      1532
Routed  3184/3249 Partitions, Violations =      1880
Routed  3200/3249 Partitions, Violations =      2440
Routed  3216/3249 Partitions, Violations =      2447
Routed  3232/3249 Partitions, Violations =      2690
Routed  3248/3249 Partitions, Violations =      2842

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      2747
        Diff net spacing : 1331
        Diff net via-cut spacing : 81
        Less than minimum area : 4
        Less than minimum width : 77
        Same net spacing : 140
        Same net via-cut spacing : 7
        Short : 1107

[Iter 0] Elapsed real time: 0:00:24 
[Iter 0] Elapsed cpu  time: sys=0:00:00 usr=0:00:30 total=0:00:31
[Iter 0] Stage (MB): Used   95  Alloctr   98  Proc   40 
[Iter 0] Total (MB): Used  118  Alloctr  123  Proc 7841 

End DR iteration 0 with 3249 parts

Start DR iteration 1: non-uniform partition
Routed  1/95 Partitions, Violations =   2743
Routed  2/95 Partitions, Violations =   2743
Routed  3/95 Partitions, Violations =   2711
Routed  4/95 Partitions, Violations =   2664
Routed  5/95 Partitions, Violations =   2662
Routed  6/95 Partitions, Violations =   2651
Routed  7/95 Partitions, Violations =   2649
Routed  8/95 Partitions, Violations =   2645
Routed  9/95 Partitions, Violations =   2634
Routed  10/95 Partitions, Violations =  2613
Routed  11/95 Partitions, Violations =  2608
Routed  12/95 Partitions, Violations =  2519
Routed  13/95 Partitions, Violations =  2468
Routed  14/95 Partitions, Violations =  2459
Routed  15/95 Partitions, Violations =  2305
Routed  16/95 Partitions, Violations =  2302
Routed  17/95 Partitions, Violations =  2241
Routed  18/95 Partitions, Violations =  2092
Routed  19/95 Partitions, Violations =  2050
Routed  20/95 Partitions, Violations =  2046
Routed  21/95 Partitions, Violations =  2034
Routed  22/95 Partitions, Violations =  2023
Routed  23/95 Partitions, Violations =  1985
Routed  24/95 Partitions, Violations =  1857
Routed  25/95 Partitions, Violations =  1848
Routed  26/95 Partitions, Violations =  1848
Routed  27/95 Partitions, Violations =  1827
Routed  28/95 Partitions, Violations =  1819
Routed  29/95 Partitions, Violations =  1749
Routed  30/95 Partitions, Violations =  1732
Routed  31/95 Partitions, Violations =  1687
Routed  32/95 Partitions, Violations =  1673
Routed  33/95 Partitions, Violations =  1557
Routed  34/95 Partitions, Violations =  1525
Routed  35/95 Partitions, Violations =  1524
Routed  36/95 Partitions, Violations =  1524
Routed  37/95 Partitions, Violations =  1515
Routed  38/95 Partitions, Violations =  1498
Routed  39/95 Partitions, Violations =  1496
Routed  40/95 Partitions, Violations =  1448
Routed  41/95 Partitions, Violations =  1446
Routed  42/95 Partitions, Violations =  1398
Routed  43/95 Partitions, Violations =  1394
Routed  44/95 Partitions, Violations =  1377
Routed  45/95 Partitions, Violations =  1367
Routed  46/95 Partitions, Violations =  1288
Routed  47/95 Partitions, Violations =  1287
Routed  48/95 Partitions, Violations =  1278
Routed  49/95 Partitions, Violations =  1268
Routed  50/95 Partitions, Violations =  1243
Routed  51/95 Partitions, Violations =  1165
Routed  52/95 Partitions, Violations =  1157
Routed  53/95 Partitions, Violations =  1150
Routed  54/95 Partitions, Violations =  1148
Routed  55/95 Partitions, Violations =  1087
Routed  56/95 Partitions, Violations =  1076
Routed  57/95 Partitions, Violations =  1073
Routed  58/95 Partitions, Violations =  1067
Routed  59/95 Partitions, Violations =  1055
Routed  60/95 Partitions, Violations =  1047
Routed  61/95 Partitions, Violations =  1017
Routed  62/95 Partitions, Violations =  1017
Routed  63/95 Partitions, Violations =  1016
Routed  64/95 Partitions, Violations =  1010
Routed  65/95 Partitions, Violations =  979
Routed  66/95 Partitions, Violations =  977
Routed  67/95 Partitions, Violations =  889
Routed  68/95 Partitions, Violations =  851
Routed  69/95 Partitions, Violations =  804
Routed  70/95 Partitions, Violations =  773
Routed  71/95 Partitions, Violations =  632
Routed  72/95 Partitions, Violations =  569
Routed  73/95 Partitions, Violations =  522
Routed  74/95 Partitions, Violations =  445
Routed  75/95 Partitions, Violations =  396
Routed  76/95 Partitions, Violations =  396
Routed  77/95 Partitions, Violations =  385
Routed  78/95 Partitions, Violations =  369
Routed  79/95 Partitions, Violations =  369
Routed  80/95 Partitions, Violations =  339
Routed  81/95 Partitions, Violations =  272
Routed  82/95 Partitions, Violations =  264
Routed  83/95 Partitions, Violations =  264
Routed  84/95 Partitions, Violations =  199
Routed  85/95 Partitions, Violations =  199
Routed  86/95 Partitions, Violations =  197
Routed  87/95 Partitions, Violations =  195
Routed  88/95 Partitions, Violations =  183
Routed  89/95 Partitions, Violations =  162
Routed  90/95 Partitions, Violations =  119
Routed  91/95 Partitions, Violations =  119
Routed  92/95 Partitions, Violations =  118
Routed  93/95 Partitions, Violations =  115
Routed  94/95 Partitions, Violations =  68
Routed  95/95 Partitions, Violations =  43

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      43
        Diff net spacing : 5
        Diff net via-cut spacing : 1
        Less than minimum area : 1
        Same net spacing : 1
        Short : 35

[Iter 1] Elapsed real time: 0:00:30 
[Iter 1] Elapsed cpu  time: sys=0:00:00 usr=0:00:36 total=0:00:37
[Iter 1] Stage (MB): Used   95  Alloctr   98  Proc   40 
[Iter 1] Total (MB): Used  118  Alloctr  123  Proc 7841 

End DR iteration 1 with 95 parts

Start DR iteration 2: non-uniform partition
Routed  1/11 Partitions, Violations =   39
Routed  2/11 Partitions, Violations =   39
Routed  3/11 Partitions, Violations =   36
Routed  4/11 Partitions, Violations =   36
Routed  5/11 Partitions, Violations =   35
Routed  6/11 Partitions, Violations =   33
Routed  7/11 Partitions, Violations =   25
Routed  8/11 Partitions, Violations =   14
Routed  9/11 Partitions, Violations =   13
Routed  10/11 Partitions, Violations =  13
Routed  11/11 Partitions, Violations =  12

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      12
        Less than minimum area : 1
        Short : 11

[Iter 2] Elapsed real time: 0:00:31 
[Iter 2] Elapsed cpu  time: sys=0:00:01 usr=0:00:38 total=0:00:39
[Iter 2] Stage (MB): Used   95  Alloctr   98  Proc   40 
[Iter 2] Total (MB): Used  118  Alloctr  123  Proc 7841 

End DR iteration 2 with 11 parts

Start DR iteration 3: non-uniform partition
Routed  1/4 Partitions, Violations =    5
Routed  2/4 Partitions, Violations =    8
Routed  3/4 Partitions, Violations =    16
Routed  4/4 Partitions, Violations =    15

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      15
        Diff net spacing : 5
        Less than minimum area : 1
        Short : 9

[Iter 3] Elapsed real time: 0:00:31 
[Iter 3] Elapsed cpu  time: sys=0:00:01 usr=0:00:38 total=0:00:39
[Iter 3] Stage (MB): Used   95  Alloctr   98  Proc   40 
[Iter 3] Total (MB): Used  118  Alloctr  123  Proc 7841 

End DR iteration 3 with 4 parts

Start DR iteration 4: non-uniform partition
Routed  1/3 Partitions, Violations =    7
Routed  2/3 Partitions, Violations =    15
Routed  3/3 Partitions, Violations =    14

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      14
        Diff net spacing : 2
        Less than minimum area : 1
        Short : 11

[Iter 4] Elapsed real time: 0:00:32 
[Iter 4] Elapsed cpu  time: sys=0:00:01 usr=0:00:39 total=0:00:40
[Iter 4] Stage (MB): Used   95  Alloctr   98  Proc   40 
[Iter 4] Total (MB): Used  118  Alloctr  123  Proc 7841 

End DR iteration 4 with 3 parts

Stop DR since reached max number of iterations


Begin revisit internal-only type DRCs ...

Checked 1/1 Partitions, Violations =    14

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      14

[REVISIT DRC] Elapsed real time: 0:00:00 
[REVISIT DRC] Elapsed cpu  time: sys=0:00:00 usr=0:00:00 total=0:00:00
[REVISIT DRC] Stage (MB): Used    0  Alloctr    0  Proc    0 
[REVISIT DRC] Total (MB): Used  118  Alloctr  122  Proc 7841 

Nets that have been changed:
Net 1 = reset
Net 2 = ENb_CP
Net 3 = CLK
Net 4 = RV_TO_DAC[9]
Net 5 = RV_TO_DAC[8]
Net 6 = RV_TO_DAC[7]
Net 7 = RV_TO_DAC[6]
Net 8 = RV_TO_DAC[5]
Net 9 = RV_TO_DAC[3]
Net 10 = RV_TO_DAC[2]
Net 11 = RV_TO_DAC[1]
Net 12 = RV_TO_DAC[0]
Net 13 = core/CPU_imm_a1_0
Net 14 = core/CPU_is_add_a2
Net 15 = core/CPU_is_add_a3
Net 16 = core/CPU_is_addi_a2
Net 17 = core/CPU_is_addi_a3
Net 18 = core/CPU_is_beq_a2
Net 19 = core/CPU_is_beq_a3
Net 20 = core/CPU_is_bne_a2
Net 21 = core/CPU_is_bne_a3
Net 22 = core/CPU_is_sub_a2
Net 23 = core/CPU_is_sub_a3
Net 24 = core/CPU_rd_valid_a2
Net 25 = core/CPU_rd_valid_a3
Net 26 = core/CPU_reset_a1
Net 27 = core/CPU_reset_a2
Net 28 = core/CPU_reset_a3
Net 29 = core/CPU_valid_taken_br_a4
Net 30 = core/CPU_valid_taken_br_a3
Net 31 = core/CPU_valid_taken_br_a5
Net 32 = core/CPU_Xreg_value_a4[27][31]
Net 33 = core/CPU_Xreg_value_a4[27][30]
Net 34 = core/CPU_Xreg_value_a4[27][29]
Net 35 = core/CPU_Xreg_value_a4[27][28]
Net 36 = core/CPU_Xreg_value_a4[27][27]
Net 37 = core/CPU_Xreg_value_a4[27][26]
Net 38 = core/CPU_Xreg_value_a4[27][25]
Net 39 = core/CPU_Xreg_value_a4[27][24]
Net 40 = core/CPU_Xreg_value_a4[27][23]
Net 41 = core/CPU_Xreg_value_a4[27][22]
Net 42 = core/CPU_Xreg_value_a4[27][21]
Net 43 = core/CPU_Xreg_value_a4[27][20]
Net 44 = core/CPU_Xreg_value_a4[27][19]
Net 45 = core/CPU_Xreg_value_a4[27][18]
Net 46 = core/CPU_Xreg_value_a4[27][17]
Net 47 = core/CPU_Xreg_value_a4[27][16]
Net 48 = core/CPU_Xreg_value_a4[27][15]
Net 49 = core/CPU_Xreg_value_a4[27][14]
Net 50 = core/CPU_Xreg_value_a4[27][13]
Net 51 = core/CPU_Xreg_value_a4[27][12]
Net 52 = core/CPU_Xreg_value_a4[27][11]
Net 53 = core/CPU_Xreg_value_a4[27][10]
Net 54 = core/CPU_Xreg_value_a4[27][9]
Net 55 = core/CPU_Xreg_value_a4[27][8]
Net 56 = core/CPU_Xreg_value_a4[27][7]
Net 57 = core/CPU_Xreg_value_a4[27][6]
Net 58 = core/CPU_Xreg_value_a4[27][5]
Net 59 = core/CPU_Xreg_value_a4[27][4]
Net 60 = core/CPU_Xreg_value_a4[27][3]
Net 61 = core/CPU_Xreg_value_a4[27][2]
Net 62 = core/CPU_Xreg_value_a4[27][1]
Net 63 = core/CPU_Xreg_value_a4[27][0]
Net 64 = core/CPU_Xreg_value_a4[26][31]
Net 65 = core/CPU_Xreg_value_a4[26][30]
Net 66 = core/CPU_Xreg_value_a4[26][29]
Net 67 = core/CPU_Xreg_value_a4[26][28]
Net 68 = core/CPU_Xreg_value_a4[26][27]
Net 69 = core/CPU_Xreg_value_a4[26][26]
Net 70 = core/CPU_Xreg_value_a4[26][25]
Net 71 = core/CPU_Xreg_value_a4[26][24]
Net 72 = core/CPU_Xreg_value_a4[26][23]
Net 73 = core/CPU_Xreg_value_a4[26][22]
Net 74 = core/CPU_Xreg_value_a4[26][21]
Net 75 = core/CPU_Xreg_value_a4[26][20]
Net 76 = core/CPU_Xreg_value_a4[26][19]
Net 77 = core/CPU_Xreg_value_a4[26][18]
Net 78 = core/CPU_Xreg_value_a4[26][17]
Net 79 = core/CPU_Xreg_value_a4[26][16]
Net 80 = core/CPU_Xreg_value_a4[26][15]
Net 81 = core/CPU_Xreg_value_a4[26][14]
Net 82 = core/CPU_Xreg_value_a4[26][13]
Net 83 = core/CPU_Xreg_value_a4[26][12]
Net 84 = core/CPU_Xreg_value_a4[26][11]
Net 85 = core/CPU_Xreg_value_a4[26][10]
Net 86 = core/CPU_Xreg_value_a4[26][9]
Net 87 = core/CPU_Xreg_value_a4[26][8]
Net 88 = core/CPU_Xreg_value_a4[26][7]
Net 89 = core/CPU_Xreg_value_a4[26][6]
Net 90 = core/CPU_Xreg_value_a4[26][5]
Net 91 = core/CPU_Xreg_value_a4[26][4]
Net 92 = core/CPU_Xreg_value_a4[26][3]
Net 93 = core/CPU_Xreg_value_a4[26][2]
Net 94 = core/CPU_Xreg_value_a4[26][1]
Net 95 = core/CPU_Xreg_value_a4[26][0]
Net 96 = core/CPU_Xreg_value_a4[25][31]
Net 97 = core/CPU_Xreg_value_a4[25][30]
Net 98 = core/CPU_Xreg_value_a4[25][29]
Net 99 = core/CPU_Xreg_value_a4[25][28]
Net 100 = core/CPU_Xreg_value_a4[25][27]
.... and 2654 other nets
Total number of changed nets = 2754 (out of 2765)

[DR: Done] Elapsed real time: 0:00:32 
[DR: Done] Elapsed cpu  time: sys=0:00:01 usr=0:00:40 total=0:00:41
[DR: Done] Stage (MB): Used    1  Alloctr    4  Proc   40 
[DR: Done] Total (MB): Used   24  Alloctr   29  Proc 7841 
[ECO: DR] Elapsed real time: 0:00:38 
[ECO: DR] Elapsed cpu  time: sys=0:00:01 usr=0:00:47 total=0:00:49
[ECO: DR] Stage (MB): Used   21  Alloctr   25  Proc   56 
[ECO: DR] Total (MB): Used   24  Alloctr   29  Proc 7841 

ECO Route finished with 0 open nets, of which 0 are frozen
Information: Merged away 5 aligned/redundant DRCs. (ZRT-305)

ECO Route finished with 9 violations

DRC-SUMMARY:
        @@@@@@@ TOTAL VIOLATIONS =      9
        Diff net spacing : 2
        Less than minimum area : 1
        Short : 6



Total Wire Length =                    120814 micron
Total Number of Contacts =             29797
Total Number of Wires =                28849
Total Number of PtConns =              297
Total Number of Routed Wires =       28849
Total Routed Wire Length =           120694 micron
Total Number of Routed Contacts =       29797
        Layer          li1 :        135 micron
        Layer         met1 :      14592 micron
        Layer         met2 :      47800 micron
        Layer         met3 :      38039 micron
        Layer         met4 :      15058 micron
        Layer         met5 :       5189 micron
        Via        M4M5_PR :        236
        Via        M3M4_PR :       1981
        Via      M3M4_PR_C :         94
        Via        M2M3_PR :       6528
        Via   M2M3_PR(rot) :        275
        Via        M1M2_PR :      10819
        Via   M1M2_PR(rot) :         20
        Via        L1M1_PR :       8490
        Via   L1M1_PR(rot) :       1274
        Via      L1M1_PR_C :         80

 
Redundant via conversion report:
--------------------------------

  Total optimized via conversion rate =  0.00% (0 / 29797 vias)
 
    Layer mcon       =  0.00% (0      / 9844    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (9844    vias)
    Layer via        =  0.00% (0      / 10839   vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (10839   vias)
    Layer via2       =  0.00% (0      / 6803    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (6803    vias)
    Layer via3       =  0.00% (0      / 2075    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (2075    vias)
    Layer via4       =  0.00% (0      / 236     vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (236     vias)
 
  Total double via conversion rate    =  0.00% (0 / 29797 vias)
 
    Layer mcon       =  0.00% (0      / 9844    vias)
    Layer via        =  0.00% (0      / 10839   vias)
    Layer via2       =  0.00% (0      / 6803    vias)
    Layer via3       =  0.00% (0      / 2075    vias)
    Layer via4       =  0.00% (0      / 236     vias)
 
  The optimized via conversion rate based on total routed via count =  0.00% (0 / 29797 vias)
 
    Layer mcon       =  0.00% (0      / 9844    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (9844    vias)
    Layer via        =  0.00% (0      / 10839   vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (10839   vias)
    Layer via2       =  0.00% (0      / 6803    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (6803    vias)
    Layer via3       =  0.00% (0      / 2075    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (2075    vias)
    Layer via4       =  0.00% (0      / 236     vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (236     vias)
 

Total number of nets = 2765
0 open nets, of which 0 are frozen
Total number of excluded ports = 0 ports of 0 unplaced cells connected to 0 nets
                                 0 ports without pins of 0 cells connected to 0 nets
                                 0 ports of 0 cover cells connected to 0 non-pg nets
Total number of DRCs = 9
Total number of antenna violations = antenna checking not active
Information: Routes in non-preferred voltage areas = 0 (ZRT-559)

Total Wire Length =                    120814 micron
Total Number of Contacts =             29797
Total Number of Wires =                28849
Total Number of PtConns =              297
Total Number of Routed Wires =       28849
Total Routed Wire Length =           120694 micron
Total Number of Routed Contacts =       29797
        Layer          li1 :        135 micron
        Layer         met1 :      14592 micron
        Layer         met2 :      47800 micron
        Layer         met3 :      38039 micron
        Layer         met4 :      15058 micron
        Layer         met5 :       5189 micron
        Via        M4M5_PR :        236
        Via        M3M4_PR :       1981
        Via      M3M4_PR_C :         94
        Via        M2M3_PR :       6528
        Via   M2M3_PR(rot) :        275
        Via        M1M2_PR :      10819
        Via   M1M2_PR(rot) :         20
        Via        L1M1_PR :       8490
        Via   L1M1_PR(rot) :       1274
        Via      L1M1_PR_C :         80

 
Redundant via conversion report:
--------------------------------

  Total optimized via conversion rate =  0.00% (0 / 29797 vias)
 
    Layer mcon       =  0.00% (0      / 9844    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (9844    vias)
    Layer via        =  0.00% (0      / 10839   vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (10839   vias)
    Layer via2       =  0.00% (0      / 6803    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (6803    vias)
    Layer via3       =  0.00% (0      / 2075    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (2075    vias)
    Layer via4       =  0.00% (0      / 236     vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (236     vias)
 
  Total double via conversion rate    =  0.00% (0 / 29797 vias)
 
    Layer mcon       =  0.00% (0      / 9844    vias)
    Layer via        =  0.00% (0      / 10839   vias)
    Layer via2       =  0.00% (0      / 6803    vias)
    Layer via3       =  0.00% (0      / 2075    vias)
    Layer via4       =  0.00% (0      / 236     vias)
 
  The optimized via conversion rate based on total routed via count =  0.00% (0 / 29797 vias)
 
    Layer mcon       =  0.00% (0      / 9844    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (9844    vias)
    Layer via        =  0.00% (0      / 10839   vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (10839   vias)
    Layer via2       =  0.00% (0      / 6803    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (6803    vias)
    Layer via3       =  0.00% (0      / 2075    vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (2075    vias)
    Layer via4       =  0.00% (0      / 236     vias)
        Un-optimized =  0.00% (0       vias)
        Un-mapped    = 100.00% (236     vias)
 
Topology ECO not run, no qualifying violations or in frozen nets.
Updating the database ...
...updated 5508 nets with eco mode
[ECO: End] Elapsed real time: 0:00:39 
[ECO: End] Elapsed cpu  time: sys=0:00:01 usr=0:00:48 total=0:00:49
[ECO: End] Stage (MB): Used   -2  Alloctr   -2  Proc   56 
[ECO: End] Total (MB): Used    0  Alloctr    1  Proc 7841 
Information: The stitching and editing of coupling caps is turned OFF for design 'vsdbabysocsky130_fd_sc_hd:vsdbabysoc/init_dp.design'. (TIM-125)
Information: Design vsdbabysoc has 2765 nets, 0 global routed, 2763 detail routed. (NEX-024)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Information: Serialized np data
INFO: timer data saved to /tmp/vsdbabysoc_22469_534472320.timdat

Route-opt ECO routing complete            CPU:   617 s (  0.17 hr )  ELAPSE:   577 s (  0.16 hr )  MEM-PEAK:  1207 MB
Co-efficient Ratio Summary:
4.193421775166  6.578037814158  2.479638928695  7.744197640401  0.485050018954  3.179566031904  5.567236254587  2.894458587461  6.565923231003  9.017931705874  3.104670600794  9.822088755001  6.078397264085
2.744208411134  8.318114345325  9.699224767401  2.464633050064  1.382370221226  9.387184029619  3.142429406669  0.968756989964  6.613182747434  4.146572993224  7.876358718973  2.150784359846  0.963908441094
2.700148513892  3.840459705868  3.750205794587  7.663468942299  6.212201175551  7.759679671187  7.862265556717  0.402381177150  0.032847019037  0.596115712371  4.701287196653  7.264784768319  8.278525698268
1.183725260902  3.334435149813  5.867608472291  7.173804485364  9.669819999761  7.591487347087  7.632106393266  7.679072263326  9.831316202870  1.878809017928  3.230072143390  1.285829283576  7.050624794780
2.403928243646  6.139851285623  4.100219707789  1.274728689655  4.570746653063  5.624878808820  7.826857253678  4.759137618263  5.409029940517  2.609827852834  0.626142338435  6.340225808169  9.187648322495
0.513656866636  5.027579359131  6.119812187772  6.181482412107  1.581675365776  7.486041822107  9.584562469756  2.041721587119  3.921162881578  0.650482882345  9.472458702260  3.327033540144  4.897385849020
6.860149314467  2.000103792260  9.095344648258  2.197051809512  0.915900067443  5.466092308426  8.142690055883  4.607197465224  8.244465718707  3.504874905451  1.682716812649  7.132082964760  9.939736008373
3.617852842698  8.946771378221  1.169968273376  4.529956756262  3.090979409795  5.580726136993  1.131397763510  0.721703825254  7.515949431830  0.649326409371  1.017031843374  5.870168812419  5.826904183342
4.349383362440  0.216215659986  9.612141163196  7.311166882104  0.823519141628  0.356161286693  8.022019569284  2.603136785844  5.024804575503  1.359353721353  5.407563251062  2.863772623768  1.265574352200
0.417760483548  1.833871106650  6.448556478757  4.848383212548  2.936801055033  8.671494124225  4.212305316610  9.007621470112  3.308109192692  5.607475309985  8.514743440284  2.723225125684  9.324480238770
1.551199059902  2.300507943294  0.754684771748  1.961431511696  2.781303341418  3.355375155650  9.437909893602  9.136706842545  9.020201222604  9.978475695117  7.467457534808  3.130823977671  9.815194040044
4.331414707143  0.413523625930  3.309919839288  7.452708306103  1.902827924529  5.623407259547  7.269363008696  3.367407303784  7.093643539942  7.411330356476  6.944246776411  5.298114177237  4.802353947382
3.894401533847  4.531619150911  1.605154398472  8.173740347963  9.287277782019  7.540402246725  0.000375817956  5.833788756721  4.754589203685  4.387465856592  1.217863701554  7.564423566617  0.800107286343
9.509851777827  3.964084915099  8.341122572480  5.604917069922  5.026083254063  3.950065336457  0.221248438718  4.029613514242  9.406661010015  2.789968861318  0.723294214418  8.752873933885  8.918386519113
5.103696166388  4.141093911683  8.443880025614  0.064450475020  6.053169774946  8.842290829440  1.167972275967  8.473961986224  3.056719064478  7.977154203284  5.095897859472  1.571920626378  7.396066020513
Information: The stitching and editing of coupling caps is turned OFF for design 'vsdbabysocsky130_fd_sc_hd:vsdbabysoc/init_dp.design'. (TIM-125)
Information: Design vsdbabysoc has 2765 nets, 0 global routed, 2763 detail routed. (NEX-024)
Warning: Technology layer 'li1' setting 'routing-direction' is not valid (NEX-001)
Information: The RC mode used is DR for design 'vsdbabysoc'. (NEX-022)
---extraction options---
Corner: func1
Global options:
 reference_direction       : use_from_tluplus
 real_metalfill_extraction : none
 virtual_shield_extraction : true
---app options---
 host.max_cores                   : 8
 extract.connect_open           : true
 extract.incremental_extraction : true
 extract.enable_coupling_cap    : false
Extracting design: vsdbabysoc 
Information: coupling capacitance is lumped to ground. (NEX-030)
Information: 2763 nets are successfully extracted. (NEX-028)
Warning: Advanced receiver model has not been enabled for detailed routed design. (TIM-204)
Information: Update timing completed net estimation for all the timing graph nets (TIM-111)
Information: Net estimation statistics: timing graph nets = 2763, routed nets = 2763, across physical hierarchy nets = 0, parasitics cached nets = 2763, delay annotated nets = 0, parasitics annotated nets = 0, multi-voltage nets = 19. (TIM-112)
Information: Freeing timing information from routing. (ZRT-574)

Route-opt final QoR
___________________
Scenario Mapping Table
1: func1

Pathgroup Mapping Table
1: **default**
2: **async_default**
3: **clock_gating_default**
4: **in2reg_default**
5: **reg2out_default**
6: **in2out_default**
7: clk

-------------------------------------------------------------------------------------------------------------------------------------------------------------
PATHGROUP QOR 
-------------------------------------------------------------------------------------------------------------------------------------------------------------
Scene  PG      WNS        TNS    NSV      WHV        THV    NHV
    1   1   0.0000     0.0000      0   0.0000     0.0000      0
    1   2   0.0000     0.0000      0   0.0000     0.0000      0
    1   3   0.0000     0.0000      0   0.0000     0.0000      0
    1   4   0.0000     0.0000      0   0.0000     0.0000      0
    1   5   0.0000     0.0000      0   0.0000     0.0000      0
    1   6   0.0000     0.0000      0   0.0000     0.0000      0
    1   7   1.6974    96.4549    100   0.1873    12.4723    236
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
SCENARIO QOR 
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Scene  PG      WNS        TNS   R2RTNS    NSV      WHV        THV    NHV  MaxTrnV   MaxTranC  MaxCapV    Leakage
    1   *   1.6974    96.4549  96.4549    100   0.1873    12.4723    236       42    20.3542       41      8.009
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
DESIGN QOR 
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Scene  PG      WNS        TNS   R2RTNS    NSV      WHV        THV    NHV  MaxTrnV   MaxTranC  MaxCapV    Leakage         Area    InstCnt
    *   *   1.6974    96.4549  96.4549    100   0.1873    12.4723    236       42    20.3542       41      8.009    696592.56       2743
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Route-opt final QoR Summary         WNS        TNS   R2RTNS    NSV      WHV        THV    NHV  MaxTrnV  MaxCapV    Leakage         Area    InstCnt
Route-opt final QoR Summary      1.6974    96.4549  96.4549    100   0.1873    12.4723    236       42       41      8.009    696592.56       2743

Route-opt command complete                CPU:   621 s (  0.17 hr )  ELAPSE:   578 s (  0.16 hr )  MEM-PEAK:  1207 MB
Route-opt command statistics  CPU=107 sec (0.03 hr) ELAPSED=61 sec (0.02 hr) MEM-PEAK=1.179 GB
Information: Ending 'route_opt' (FLW-8001)
Information: Time: 2024-07-21 02:38:15 / Session: 0.16 hr / Command: 0.02 hr / Memory: 1208 MB (FLW-8100)
[icc2-lic Sun Jul 21 02:38:15 2024] Command 'create_stdcell_fillers' requires licenses
[icc2-lic Sun Jul 21 02:38:15 2024] Attempting to check-out alternate set of keys directly with queueing
[icc2-lic Sun Jul 21 02:38:15 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:38:15 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:38:15 2024] Sending check-out request for 'ICCompilerII-8' (1) with wait option
[icc2-lic Sun Jul 21 02:38:15 2024] Check-out request for 'ICCompilerII-8' with wait option succeeded
[icc2-lic Sun Jul 21 02:38:15 2024] Sending checkout check request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:38:15 2024] Checkout check request for 'ICCompilerII-8' returned 0 
[icc2-lic Sun Jul 21 02:38:15 2024] Sending count request for 'ICCompilerII-8' 
[icc2-lic Sun Jul 21 02:38:15 2024] Count request for 'ICCompilerII-8' returned 1 
[icc2-lic Sun Jul 21 02:38:15 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:38:15 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:38:15 2024] Sending check-out request for 'ICCompilerII-NX' (1) with wait option
[icc2-lic Sun Jul 21 02:38:15 2024] Check-out request for 'ICCompilerII-NX' with wait option succeeded
[icc2-lic Sun Jul 21 02:38:15 2024] Sending checkout check request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:38:15 2024] Checkout check request for 'ICCompilerII-NX' returned 0 
[icc2-lic Sun Jul 21 02:38:15 2024] Sending count request for 'ICCompilerII-NX' 
[icc2-lic Sun Jul 21 02:38:15 2024] Count request for 'ICCompilerII-NX' returned 1 
[icc2-lic Sun Jul 21 02:38:15 2024] Check-out of alternate set of keys directly with queueing was successful
* Disjointed site row process : FALSE
INFO:: begin filler insertions...
CHF::NPL_CHKQUERY_PNET checking turned off by default
master sky130_fd_sc_hd__fill_8 has site unit
master sky130_fd_sc_hd__fill_4 has site unit
master sky130_fd_sc_hd__fill_2 has site unit
master sky130_fd_sc_hd__fill_1 has site unit
 Use site unit (4600)
CHF: Multi-threading turned off because variant design not supported. 
Warning: Routing direction of metal layer li1 is neither "horizontal" nor "vertical".  PDC checks will not be performed on this layer. (PDC-003)
Warning: Routing direction of metal layer fieldpoly is neither "horizontal" nor "vertical".  PDC checks will not be performed on this layer. (PDC-003)
PDC app_options settings =========
        place.legalize.enable_prerouted_net_check: 1
        place.legalize.num_tracks_for_access_check: 1
        place.legalize.use_eol_spacing_for_access_check: 0
        place.legalize.allow_touch_track_for_access_check: 1
        place.legalize.reduce_conservatism_in_eol_check: 0
        place.legalize.preroute_shape_merge_distance: 0.0
        place.legalize.enable_non_preferred_direction_span_check: 0

Layer met1: cached 207 shapes out of 10493 total shapes.
Layer met2: cached 0 shapes out of 12056 total shapes.
Cached 20324 vias out of 61923 total vias.
CHF: loading design...
CHF:: Loading blkInsts from NDM...
CHF:: Building row/inst relations...
INFO:: Use fillers in order(1000): sky130_fd_sc_hd__fill_8 (8 x 1), sky130_fd_sc_hd__fill_4 (4 x 1), sky130_fd_sc_hd__fill_2 (2 x 1), sky130_fd_sc_hd__fill_1 (1 x 1), 
        10% complete ...
        20% complete ...
        30% complete ...
        40% complete ...
        50% complete ...
        60% complete ...
        70% complete ...
        80% complete ...
        90% complete ...
Regular Filler Insertion Complete
CHF:: Create all new fillers...
... 160441 of regular filler sky130_fd_sc_hd__fill_8 inserted
... 761 of regular filler sky130_fd_sc_hd__fill_4 inserted
... 1033 of regular filler sky130_fd_sc_hd__fill_2 inserted
... 1297 of regular filler sky130_fd_sc_hd__fill_1 inserted
Saving all libraries...
1
icc2_shell> start_gui
icc2_shell> 
# SFAL-VSD-SoC Design and Implementation

I am excited to share the details of my 12-week SoC Design and Implementation course. The course kicks off with 6 weeks dedicated to foundational semiconductor design concepts, covering essentials such as floorplanning, placement, CTS, routing, optimization, and verification. During this time, I will gain comprehensive knowledge ranging from chip size estimation to signal integrity solutions.

Following the initial phase, I have the opportunity to select a specialization topic. This allows me to delve deeper into an area of interest, guided by industry experts, ensuring targeted and in-depth learning.

The course then transitions to a 4-week segment focused on advanced, industry-grade concepts. These sessions are led by professionals and culminate in a challenging design project that simulates real-world scenarios, providing practical, hands-on experience.

In the final 2 weeks, the emphasis shifts to professional skills development. This includes mastering GitHub documentation and navigating internship selection processes, essential for a successful career in the semiconductor industry.

This program offers a perfect blend of foundational learning, specialization, and practical application. It is designed to prepare me for a successful career in SoC design and implementation, balancing theoretical knowledge with hands-on experience. Through this program, I will:

- Master the entire SoC design process, from floorplanning to physical verification.
- Personalize my learning by selecting a specialization topic.
- Gain valuable insights from industry experts through an in-depth design project.
- Enhance my professional portfolio with GitHub documentation skills.
- Increase my chances of securing internships in the semiconductor industry.

This is a unique opportunity to transform my passion into expertise and advance my career in SoC design and implementation!

## Day To Day Progress

<details>
    <summary>🛠️ System Set Up and Tools Installation</summary>
    
## System Set Up and Tools Installation

**Instructions on how to install tools:**

Download the Oracle Virtual Machine - [VirtualBox Downloads](https://www.virtualbox.org/wiki/Downloads)

**System Configuration:**

- 6GB RAM
- 50GB HDD
- Ubuntu 20.04+
- 4vCPU

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/bde8d833-abdd-4a08-bbc3-fcc341b97227)

**Tool Installation:**

**1. Yosys**

```sh
$ sudo apt-get update
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys
$ sudo apt install make (If make is not installed please install it)
$ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make config-gcc
$ make
$ sudo make install
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/136cb330-2c9a-4d3b-93d0-8ec554c1f742)

**2. iverilog**

```sh
$sudo apt-get update
$sudo apt-get install iverilog
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/e9585bf3-4f92-4dbb-a18a-7d018e64467a)

**3. GTKwave**

```sh
$sudo apt-get update
$sudo apt install gtkwave
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/5d789652-3560-4a5a-8c36-1c9556fea5be)

</details>

<details>
    <summary>🛠️ Introduction to Verilog RTL Design and Synthesis</summary>

## Introduction to Verilog RTL Design and Synthesis

### Introduction to iVerilog OpenSource Simulator

**iverilog**: Icarus Verilog, commonly known as Iverilog, is an open-source tool used for the simulation and synthesis of digital circuits described in Verilog hardware description language (HDL).

Primarily, Iverilog is used to simulate Verilog designs, allowing designers to verify the functionality of their digital circuits before physical implementation.

Let's take `good_mux.v` design as an example:

```verilog
module good_mux (input i0 , input i1 , input sel , output reg y);
always @ (*)
begin
    if(sel)
        y <= i1;
    else 
        y <= i0;
end
endmodule
```

Let's see its Testbench `tb_good_mux.v`:

```verilog
`timescale 1ns / 1ps
module tb_good_mux;
// Inputs
reg i0,i1,sel;
// Outputs
wire y;

// Instantiate the Unit Under Test (UUT)
good_mux uut (
    .sel(sel),
    .i0(i0),
    .i1(i1),
    .y(y)
);

initial begin
    $dumpfile("tb_good_mux.vcd");
    $dumpvars(0,tb_good_mux);
    // Initialize Inputs
    sel = 0;
    i0 = 0;
    i1 = 0;
    #300 $finish;
end

always #75 sel = ~sel;
always #10 i0 = ~i0;
always #55 i1 = ~i1;
endmodule
```

**Simulation of good_mux:**

```sh
$ iverilog good_mux.v tb_good_mux.v
```

The output will be **a.out** which specifies the output file for the compiled simulation.

```sh
$ ./a.out
```

This command runs the simulation and generates a waveform dump file (in this case, `tb_good_mux.vcd`) as specified in the testbench.

To view the simulation results, use a waveform viewer like GTKWave. Open GTKWave and load the generated VCD file:

```sh
$ gtkwave tb_good_mux.vcd
```

GTKWave will open, allowing you to inspect the signals and verify the behavior of your design.

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d19c2602-6247-4ba2-8980-92dd27f52394)

### Introduction to Yosys

Yosys is an open-source framework for Verilog RTL synthesis. It is used primarily for converting high-level Verilog descriptions of digital circuits into gate-level netlists that can be implemented on FPGAs or used for ASIC design. Yosys is highly versatile and supports various front-end and back-end tools, making it a valuable tool for digital design and synthesis.

**Inputs for Yosys tool:** Design (.v), Liberty (.lib)  
**Output:** Netlist file (.net.v)

### Synthesis of a good_mux by opening Yosys:

**Read the Liberty source file:**

```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/615c393f-ca85-4d53-8ef2-0df77da89bed)

**Read the Verilog source file:**

```sh
yosys> read_verilog good_mux.v
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/fd82b4ae-3614-407b-9ded-5969d212e413)

**Perform synthesis:**

```sh
yosys> synth -top good_mux
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/49a325bf-c11c-4160-ba3c-540cc42b8066)

**Technology Mapping to the Design using the `abc` tool which is integrated with Yosys:**

```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/1d3345aa-668b-4753-bc33-f26b5c47cd89)

**Write the synthesized netlist to a Verilog file:**

```sh
yosys> write_verilog -noattr good_mux.net.v
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d1506467-d9fe-4513-9bfc-108c4c73375b)

**View the generated gate level netlist:**

```sh
yosys> show
```

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/8909cc50-ba0f-4ed7-b2b3-11782656e566)

</details>

<details>
	<summary>🛠️ Timing libs, Hierarchical vs Flat Synthesis and Efficient Flop Coding Styles </summary>

## Timing libs, Hierarchical vs Flat Synthesis and Efficient Flop Coding Styles

## Introduction to timing.lib

A .lib file, also known as a Liberty file, is a standard format used in the electronic design automation (EDA) industry to describe the timing, power, and area characteristics of the standard cells in a digital library.

**Key Contents of a .lib File:** Timing information, Power information, Area information, Operating conditions, Pin descriptions.

In this lab, we use **sky130_fd_sc_hd_tt_025C_1v80.lib**. Here's a breakdown of the filename and what each part signifies:

- **sky130:** Refers to the 130nm technology node provided by SkyWater Technology Foundry.
- **fd:** Stands for fully-depleted, indicating the type of process technology.
- **sc:** Stands for standard cell.
- **hd:** Stands for high-density standard cell library.
- **tt:** Typical process corner (typical-typical).
- **025C:** The temperature condition at which the library data is characterized (25°C).
- **1v80:** The operating voltage condition (1.8V).

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/c8748f93-e472-492c-bdf3-a71b3a26eeb5)

## Hierarchical vs Flat Synthesis

**Hierarchical synthesis:** Hierarchical synthesis involves organizing the design into a hierarchy of modules or blocks, where each module represents a functional unit or a logical partition of the design.

**Read the Liberty source file:**

```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/615c393f-ca85-4d53-8ef2-0df77da89bed)

**Read the Verilog source file:**

```sh
yosys> read_verilog multiple_modules.v
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/48d780ce-5c8b-4c4b-aa63-fc87c373b0d0)

**Perform synthesis:**

```sh
yosys> synth -top multiple_modules
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/b5fdec51-ceaa-4de6-a425-5d5ca0c20d7a)

**Technology Mapping to the Design using *abc* tool which is integrated with Yosys:**

```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/27ce5ad2-5528-4b44-b466-6a4f7bac390a)

**View the generated gate level netlist:**

```sh
yosys> show multiple_modules
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/dc5ef610-0836-424d-93a4-9506f576a33e)

**Flat synthesis:** Flat synthesis involves synthesizing the entire design as a single, monolithic entity without hierarchical organization.

**To flatten the design:**

```sh
yosys> flatten
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/41ea4f42-dfd8-4cfd-9b0a-215cec6f2a3c)

**To write a netlist to .v file:**

```sh
yosys> write_verilog -noattr multiple_modules_flat.v
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/a33036ba-4305-48db-a714-d438269da691)

**To view the netlist (.v):**

```sh
yosys> !givim multiple_modules_flat.v
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/04d6e3f8-2307-452b-87f4-0ff5cbab5025)

**To view the flattened gate level netlist:**

```sh
yosys> show
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/1d37fa5f-6bbf-402e-94d7-5232a2348038)

## Sub-Module level synthesis

**Read the Liberty source file:**

```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/615c393f-ca85-4d53-8ef2-0df77da89bed)

**Read the Verilog source file:**

```sh
yosys> read_verilog multiple_modules.v
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/48d780ce-5c8b-4c4b-aa63-fc87c373b0d0)

**Perform synthesis:**

```sh
yosys> synth -top sub_module1
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/10a7fcae-1eea-4453-88cf-12d8ff4fbe21)

**Technology Mapping to the Design using *abc* tool which is integrated with Yosys:**

```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/2393c805-815c-45af-9046-187d3648a313)

**To view the generated gate level netlist:**

```sh
yosys> show 
```
![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/eee9dda1-5adb-46db-9f2b-f8a7c5203adc)

## Different Methods for Flip-Flop Coding and Performance Enhancement

**Why are flops necessary, and how do they mitigate glitches in the circuit?**

Glitches can manifest in digital circuits due to factors like signal propagation delays, noise interference, or timing discrepancies. Flops play a crucial role in preventing glitches during circuit operation through the following mechanisms:

- **Synchronization:** Flops operate as edge-triggered components, responding exclusively to transitions in the input signal, such as rising or falling edges. This synchronization mechanism ensures that the output changes occur only at specific moments, minimizing the potential for glitches induced by transient signal fluctuations.
- **Timing Control:** Flops are typically governed by a clock signal, ensuring that all circuit activities unfold synchronously. This synchronized operation eliminates timing discrepancies that could otherwise lead to glitches stemming from data arriving at disparate times.

**Flop coding styles:**

- **Simulation of Asynchronous Reset D-Flip Flop using iverilog followed by GTKWave**

  ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/4398cdb4-a0ef-489b-a7cd-17b14378227a)

- **Simulation of Asynchronous Set D-Flip Flop using iverilog followed by GTKWave**

  ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/485cab5e-6c70-4176-9d5f-a4de986bf69d)

- **Simulation of Synchronous Reset D-Flip Flop using iverilog followed by GTKWave**

  ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/7d1ced01-3428-4308-8d2f-ad07fe6d6afb)

**Synthesis of Asynchronous Reset D-Flip Flop using yosys:**

  Here, we have to map the Flip-Flops to the *dfflib* which is present in *sky130_fd_sc_hd_tt_025C_1v80.lib*

  Here's the command for mapping the flipflops to the dfflib:

  ```sh
  yosys> dfflibmap -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib
  ```
  ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/c784d898-40e0-4c8e-a2eb-ca1a7c55e61f)

-

 **Synthesis of Asynchronous set D-Flip Flop using yosys:**

  ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/5ac9f787-30ca-4d22-860c-5a163fd10d40)

- **Synthesis of Synchronous Reset D-Flip Flop using yosys:**

  ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/c44d9314-3979-490e-9a33-131d8494b550)

</details>

<details>
    <summary>🛠️ Combinational and Sequential Optimization</summary>
    <ul>
        <li>
            <details>
                <summary>Combinational Logic Optimization</summary>
                <ul>
                    <li>
                        <details>
                            <summary><strong>PART 1: For opt_check Modules</strong></summary>
                            <ol>
                                <li>
                                    <strong>Step 1: Read Library</strong>
                                    <p>In Yosys, execute the command to read the library:</p>
                                    <img width="728" alt="Read Library" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/bf2a8b14-da19-41ff-96ac-ee1fc0572722">
                                </li>
                                <li>
                                    <strong>Step 2: Read Verilog File</strong>
                                    <p>Load the Verilog file for the 'opt_check' module:</p>
                                    <img width="652" alt="Verilog File" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/1c68e3e0-f349-4a91-9aa0-df769f531e71">
                                </li>
                                <li>
                                    <strong>Step 3: Define Module for Synthesis</strong>
                                    <p>Define the module to be synthesized and view the number of cells in the module:</p>
                                    <img width="286" alt="Define Module" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/7e9cb74a-e0d5-4779-b786-eb7d62618c60">
                                    <img width="420" alt="Cell Count" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/ea4b5d4a-122a-4881-903f-66d5a37996c0">
                                </li>
                                <li>
                                    <strong>Step 4: Execute opt_clean</strong>
                                    <p>Run opt_clean to remove unused cells and wires:</p>
                                    <img width="623" alt="opt_clean Execution" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/b9d682ef-5229-4092-9dff-ac44c408aff0">
                                </li>
                                <li>
                                    <strong>Step 5: Generate Netlist</strong>
                                    <p>Generate the netlist and observe the reduction in the number of cells:</p>
                                    <img width="611" alt="Netlist Generation" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/4d887668-b080-4942-a05d-5350f1ac6e51">
                                    <img width="598" alt="Cell Reduction" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/78dbf1ca-d2c6-458c-9b3e-8ef595bd0c82">
                                </li>
                                <li>
                                    <strong>Step 6: View Netlist Design</strong>
                                    <p>Execute the show command to view the netlist design:</p>
                                    <img width="611" alt="View Netlist" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/0992c28f-063b-4a43-90b0-b2abdfad762b">
                                </li>
                                <li>
                                    <strong>Steps 7-12: Repeat for Additional Modules</strong>
                                    <p>Repeat the above steps for additional modules (opt_check2, opt_check3, opt_check4), observing the changes and improvements each time:</p>
                                    <img width="418" alt="Further Steps" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/16b97a65-65b6-4b51-ac38-3218c9d2865d">
                                    <img width="556" alt="ABC Command" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/17c9dd9f-fd22-4312-bb51-0be49eadb040">
                                    <img width="497" alt="Further ABC Command" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/3607a834-f446-462b-ab11-25856870721a">
                                </li>
                            </ol>
                        </details>
                    </li>
                    <li>
                        <details>
                            <summary><strong>PART 2: multiple_modules Optimization</strong></summary>
                            <ol>
                                <li>
                                    <strong>Step 1: Read Verilog File</strong>
                                    <p>Load the Verilog file for 'multiple_modules_opt.v'.</p>
                                    <img width="744" alt="Read Verilog File" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/4ef7a599-3701-4953-8c1a-450a923a9876">
                                </li>
                                <li>
                                    <strong>Step 2: Define the Module for Synthesis</strong>
                                    <p>Specify which module to synthesize.</p>
                                    <img width="398" alt="Define Module" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/73c1c313-ae77-45ad-9814-436b4d1cdebf">
                                </li>
                                <li>
                                    <strong>Step 3: Flatten the Design</strong>
                                    <p>Apply design flattening techniques to simplify the hierarchy.</p>
                                    <img width="475" alt="Flatten Design" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/7dd4686a-7897-4ba8-8a65-52111a04903d">
                                </li>
                                <li>
                                    <strong>Step 4: Execute opt_clean</strong>
                                    <p>Remove unused cells and wires to optimize the design.</p>
                                    <img width="635" alt="Execute opt_clean" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/4bfecaba-c49b-4ca9-958e-f1e7486aaa62">
                                </li>
                                <li>
                                    <strong>Step 5: Generate the Netlist</strong>
                                    <p>Generate the netlist and note the reduction in the number of cells.</p>
                                    <img width="617" alt="Generate Netlist" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/c7b85d9d-f18d-4992-bf0b-151f22301e3d">
                                    <img width="571" alt="Cell Reduction" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/cf863f0b-27aa-4781-8b36-0e6bd0f6275a">
                                </li>
                                <li>
                                    <strong>Step 6: View Netlist Design</strong>
                                    <p>Display the synthesized netlist design to verify correctness and optimization.</p>
                                    <img width="595" alt="View Netlist Design" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/83c65d81-da59-49a9-8735-3938a35d7c18">
                                </li>
                                <li>
                                    <strong>Step 7: Repeat Optimization for Additional Module</strong>
                                    <p>Repeat the optimization steps for 'multiple_modules_opt2.v' and observe changes.</p>
                                </li>
                                <li>
                                    <strong>Step 8: View Netlist Design for Additional Module</strong>
                                    <p>Review the final netlist design for 'multiple_modules_opt2.v'.</p>
                                    <img width="353" alt="Final Netlist Design" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/18374a6c-27ec-4bb3-9094-b057cbc6a9e2">
                                </li>
                            </ol>
                        </details>
                    </li>
                </ul>
            </details>
        </li>
        <li>
            <details>
                <summary>Sequential Logic Optimization</summary>
                <ul>
                    <li>
                        <details>
                            <summary><strong>PART 1: Dff_const Synthesis</strong></summary>
                            <ol>
                                <li>
                                    <strong>Step 1: Read the Library</strong>
                                    <p>Load the required library in Yosys.</p>
                                    <img width="735" alt="Read Library" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/c4b43fb9-4a90-4695-842f-d68680ce4f0b">
                                </li>
                                <li>
                                    <strong>Step 2: Read the Verilog File</strong>
                                    <p>Load the Verilog file for 'dff_const1.v'.</p>
                                    <img width="669" alt="Read Verilog File" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/e85367f2-5aa3-4692-9684-a5726016a08e">
                                </li>          
                                <li>
                                    <strong>Step 3: Define the Module for Synthesis</strong>
                                    <p>Specify the module to be synthesized and view the design hierarchy.</p>
                                    <img width="306" alt="Define Module" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/c5e25901-ce0f-4520-9e21-ac1d5bc2666c">
                                    <img width="422" alt="Design Hierarchy" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/80bc0503-0ade-47e4-bb10-a6773c53051f">
                                </li>
                                <li>
                                    <strong>Step 4: Run dfflibmap</strong>
                                    <p>Map the D flip-flop cells to sequential cells using dfflibmap.</p>
                                    <img width="870" alt="Run dfflibmap" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/76ce4935-130d-4692-a5b3-211c7211afdc">
                                </li>
                                <li>
                                    <strong>Step 5: Generate the Netlist</strong>
                                    <p>Create the netlist for the design.</p>
                                    <img width="611" alt="Generate Netlist" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/4802d8a6-2763-4975-be62-805a4473ac2a">
                                </li>
                                <li>
                                    <strong>Step 6: View the Design</strong>
                                    <p>Execute the 'show' command to view the synthesized design.</p>
                                    <img width="594" alt="View Design" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/8dcc75b9-0bc2-43a3-89ae-9b0a1a8b6122">
                                </li>
                                <li>
                                    <strong>Steps 7-12: Repeat for Additional Files</strong>
                                    <p>Repeat the above steps for 'dff_const2.v', 'dff_const3.v', and 'dff_const4.v', viewing the design after each synthesis.</p>
                                    <img width="609" alt="View Design 2" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/481c7486-83c1-4fe3-8301-ba1f930bc791">
                                    <img width="1359" alt="View Design 3" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/5f8109ea-1f69-4d4f-bdb5-fb59e37cc882">
                                    <img width="616" alt="View Design 4" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/69a155b4-d899-4597-9631-59e497c7edb5">
                                </li>
                            </ol>
                        </details>
                    </li>
                    <li>
                        <details>
                            <summary><strong>PART 2: Sequential Optimizations for Unused Outputs</strong></summary>
                            <ol>
                                <li>
                                    <strong>Step 1: Read the Library</strong>
                                    <p>Load the required library in Yosys.</p>
                                    <img width="744" alt="Read Library" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/f2b3de6b-a2c1-498b-b957-aec0487c08be">
                                </li>
                                <li>
                                    <strong>Step 2: Read the Verilog File</strong>
                                    <p>Load the Verilog file for 'counter_opt.v'.</p>
                                    <img width="663" alt="Read Verilog File" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/c603c612-d903-4e05-ab1d-bde84a52fd3c">
                                </li>          
                                <li>
                                    <strong>Step 3: Define the Module for Synthesis</strong>
                                    <p>Specify the module to be synthesized and view the design hierarchy.</p>
                                    <img width="324" alt="Define Module" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/1a4979c3-0da7-4cf9-9bb2-a698f2b4c51d">
                                    <img width="407" alt="Design Hierarchy" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/ecd2ad69-1e69-44cf-8403-d8670bf7028e">
                                </li>
                                <li>
                                    <strong>Step 4: Run dfflibmap</strong>
                                    <p>Map the D flip-flop cells to sequential cells using dfflibmap.</p>
                                    <img width="873" alt="Run dfflibmap" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/65e34b53-4b7a-4d55-a43f-0cf59489a386">
                                </li>
                                <li>
                                    <strong>Step 5: Generate the Netlist</strong>
                                    <p>Create the netlist for the design.</p>
                                    <img width="621" alt="Generate Netlist" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/d4b49572-0224-4ca6-978d-71ace7464ce3">
                                </li>
                                <li>
                                    <strong>Step 6: View the Design</strong>
                                    <p>Execute the 'show' command to view the synthesized design.</p>
                                    <img width="1361" alt="View Design" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/26807584-5733-4728-bc9f-de30d5d18d4d">
                                </li>
                                <li>
                                    <strong>Steps 7-8: Repeat for Additional Files</strong>
                                    <p>Repeat the above steps for 'counter_opt2.v', viewing the design after synthesis.</p>
                                    <img width="425" alt="View Design 2" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/2fdaa020-ca5a-4bf7-9ca2-b4087914ac52">
                                    <img width="1370" alt="View Design 3" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/848db3bb-35bd-4fd2-8e92-122e76d86f70">
                                </li>
                            </ol>
                        </details>
                    </li>
                </ul>
            </details>
        </li>
    </ul>
</details>


<details>
    <summary>🛠️ Gate Level Simulation, Synthesis Simulation Mismatch, and Blocking & Non-Blocking Statements </summary>
    <ul>
        <li>
            <details>
                <summary>Lab on GLS and Synth Simulation Mismatch</summary>
                <ul>
                    <li>
                        <details>
                            <summary>PART 1: For ternary_operator_mux</summary>
                            <p>Step 1</p>
                            <pre>
Load ternary_operator_mux.v & its testbench to Iverilog.
<img width="1333" alt="Screenshot 2024-05-26 at 2 25 18 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/5fb664a3-428b-40b5-95c3-6e5e76385d2e">
                            </pre>
                            <p>Step 2</p>
                            <pre>
Execute a.out file.
<img width="831" alt="Screenshot 2024-05-26 at 2 25 28 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/0845279c-0fdb-458a-b911-593f7f138990">
                            </pre>
                            <p>Step 3</p>
                            <pre>
Load the .vcd file genrated into GTKWave.
<img width="895" alt="Screenshot 2024-05-26 at 2 25 46 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/a17efccf-4528-4b08-856c-e3366dea9441">
The ternary_operator_mux's behavior is analyzed on GTKWave                            
<img width="1374" alt="Screenshot 2024-05-26 at 2 26 56 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/b2f81f99-be77-4d8f-8c5f-c76efa0db555">
                            </pre>
                            <p>Step 4</p>
                            <pre>
Invoke Yosys by using command yosys
<img width="817" alt="Screenshot 2024-05-26 at 2 27 23 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/5aeace9b-91d1-43df-b7a7-95a22d2f7dca">
                            </pre>
                            <p>Step 5</p>
                            <pre>
Read the library using read_liberty
<img width="733" alt="Screenshot 2024-05-26 at 2 27 36 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/c7df753c-4859-4517-b10b-844be452fbc0">
                            </pre>
                            <p>Step 6</p>
                            <pre>
Read the ternary_operator_mux.v using read_verilog
<img width="754" alt="Screenshot 2024-05-26 at 2 27 54 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/9bd5d509-2035-4be9-b17f-084f80f58835">
                            </pre>
                            <p>Step 7</p>
                            <pre>
Define the module that needs to be synthesized
<img width="397" alt="Screenshot 2024-05-26 at 2 28 29 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/6b23f59f-4e02-43ec-932b-750d183b30f0">
                            </pre>
                            <p>Step 8</p>
                            <pre>
Generate the netlist using abc command
<img width="622" alt="Screenshot 2024-05-26 at 2 28 54 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/9565e3d5-66ba-45ef-8464-7cec13c5cdd4">
                            </pre>
                            <p>Step 9</p>
                            <pre>
Write the netlist to ternary_operator_mux_net.v
<img width="490" alt="Screenshot 2024-05-26 at 2 29 20 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/61065d52-6532-4a74-b8ae-34b8734fbce6">
                            </pre>
                            <p>Step 10</p>
                            <pre>
Execute show to view the design
<img width="609" alt="Screenshot 2024-05-26 at 2 29 30 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/76ee78c8-95fe-47e6-a9aa-ec2b6fa1e9c1">
                            </pre>
                            <p>Step 11</p>
                            <pre>
Exit yosys and load the ternary_operator_mux_net.v to iverilog.
<img width="1372" alt="Screenshot 2024-05-26 at 2 32 32 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/30cf677a-e529-4f09-a0e2-7d77f5ab48e5">
                            </pre>
                            <p>Step 12</p>
                            <pre>
Execute a.out file.
<img width="829" alt="Screenshot 2024-05-26 at 2 32 47 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/9194b986-d637-4919-9a9a-a66ef5fd8150">
                            </pre>
                            <p>Step 13</p>
                            <pre>
Load the generated .vcd file into GTKWave
<img width="1107" alt="Screenshot 2024-05-26 at 2 33 19 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/4b04c1e3-5aff-4937-9670-706364e92fa3">
                            </pre>
                            <p>Step 14</p>
                            <pre>
Observe the GLS of ternary_operator_mux
<img width="1357" alt="Screenshot 2024-05-26 at 2 33 52 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/746f372e-4dd9-4da4-a4ff-57836f00945e">
                            </pre>
                        </details>
                    </li>
                    <li>
                        <details>
                            <summary>PART 2: For bad_mux</summary>
                            <p>Step 1</p>
                            <pre>
Load bad_mux.v & its testbench to Iverilog.
<img width="1072" alt="Screenshot 2024-05-26 at 2 47 47 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/3886f80b-b39e-4f91-9cfb-d7fd26f87a2b">
                            </pre>
                            <p>Step 2</p>
                            <pre>
Execute a.out file.
<img width="836" alt="Screenshot 2024-05-26 at 2 47 59 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/80bff7b8-6f38-42e9-b7ee-b527d9e06de7">
                            </pre>
                            <p>Step 3</p>
                            <pre>
Load the .vcd file genrated into GTKWave.
<img width="980" alt="Screenshot 2024-05-26 at 2 48 17 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/94c68039-9cd8-435f-b02d-914619642094">
The bad_mux's behavior is analyzed on GTKWave                            
<img width="1362" alt="Screenshot 2024-05-26 at 2 49 01 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/cb432d9f-023b-4b28-ac59-c25e9abff098">
                            </pre>
                            <p>Step 4</p>
                            <pre>
Invoke Yosys by using command yosys
<img width="815" alt="Screenshot 2024-05-26 at 2 49 12 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/3a80b296-11d9-4150-ae87-17d6d2c971d1">
                            </pre>
                            <p>Step 5</p>
                            <pre>
Read the library using read_liberty
<img width="731" alt="Screenshot 2024-05-26 at 2 49 25 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/4e9fade6-5f81-41ac-a5d1-f3dbf9c35e98">
                            </pre>
                            <p>Step 6</p>
                            <pre>
Read the bad_mux.v using read_verilog
<img width="756" alt="Screenshot 2024-05-26 at 2 49 40 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/3d74ecc6-a028-4c2f-bf0b-9567d5270b26">
                            </pre>
                            <p>Step 7</p>
                            <pre>
Define the module that needs to be synthesized
<img width="268" alt="Screenshot 2024-05-26 at 2 50 33 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/ac8165b4-4a23-46ad-8196-242af9a8a9a8">
                            </pre>
                            <p>Step 8</p>
                            <pre>
Generate the netlist using abc command
<img width="612" alt="Screenshot 2024-05-26 at 2 50 51 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/ca4dc976-5645-4c1f-878c-650c93136276">
                            </pre>
                            <p>Step 9</p>
                            <pre>
Write the netlist to bad_mux_net.v
<img width="364" alt="Screenshot 2024-05-26 at 2 51 09 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/87c4ebdf-95ff-4a34-847e-d49d5fe62e2a">
                            </pre>
                            <p>Step 10</p>
                            <pre>
Execute show to view the design
<img width="603" alt="Screenshot 2024-05-26 at 2 51 21 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/287737a1-3d21-4fde-8578-7a65d568b560">
                            </pre>
                            <p>Step 11</p>
                            <pre>
Exit yosys and load the bad_mux_net.v to iverilog.
<img width="1368" alt="Screenshot 2024-05-26 at 2 52 52 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/f4bc8f2c-e08a-4675-be9a-e905b879c28a">
                            </pre>
                            <p>Step 12</p>
                            <pre>
Execute a.out file.
<img width="835" alt="Screenshot 2024-05-26 at 2 53 02 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/7813fe2a-bb20-43de-9cf7-ec53c1459f51">
                            </pre>
                            <p>Step 13</p>
                            <pre>
Load the generated .vcd file into GTKWave
<img width="979" alt="Screenshot 2024-05-26 at 2 53 25 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/87f26967-6d37-45e9-963e-15d4107e767f">
                            </pre>
                            <p>Step 14</p>
                            <pre>
Observe the behavior of GLS of ternary_operator_mux due to Simulation Mismatch
<img width="1365" alt="Screenshot 2024-05-26 at 2 53 59 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/16ececd7-a724-43be-91a3-b394763a3678">
                            </pre>          
                        </details>
                    </li>
                </ul>
            </details>
        </li>
        <li>
            <details>
                <summary>Synthesis Simulation Mismatch</summary>
                <p>Step 1</p>
                <pre>
Load blocking_caveat.v & its testbench to Iverilog.
<img width="1232" alt="Screenshot 2024-05-26 at 3 20 01 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/98e89aec-e1e0-4371-9865-7f4d46970466">
                </pre>
                <p>Step 2</p>
                <pre>
Execute a.out file.
<img width="831" alt="Screenshot 2024-05-26 at 3 20 12 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/cabfbd67-5caf-467e-a4fe-36b9dd3613ae">
                </pre>
                <p>Step 3</p>
                <pre>
Load the .vcd file genrated into GTKWave.
<img width="1056" alt="Screenshot 2024-05-26 at 3 20 37 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/cd3b4d01-c92d-40e8-842f-e41fcb512967">
The blocking_caveat's behavior is analyzed on GTKWave                            
<img width="1362" alt="Screenshot 2024-05-26 at 3 21 17 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/0313fa9c-df40-46d2-9e31-0e18b9eb5b84">
                </pre>
                <p>Step 4</p>
                <pre>
Invoke Yosys by using command yosys
<img width="809" alt="Screenshot 2024-05-26 at 3 21 29 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/45aa14d6-5cc0-4a4d-b629-74bc87fda4fa">
                </pre>
                <p>Step 5</p>
                <pre>
Read the library using read_liberty
<img width="727" alt="Screenshot 2024-05-26 at 3 21 43 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/e4aa45a5-c575-4ecc-9fe6-e10553ba31da">
                </pre>
                <p>Step 6</p>
                <pre>
Read the blocking_caveat.v using read_verilog
<img width="709" alt="Screenshot 2024-05-26 at 3 21 55 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/2c84de8b-63bc-4394-b833-79569ab86d0c">
                </pre>
                <p>Step 7</p>
                <pre>
Define the module that needs to be synthesized
<img width="596" alt="Screenshot 2024-05-26 at 3 23 34 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/886c9129-5cf7-4622-8857-326ffef0597f">
                </pre>
                <p>Step 8</p>
                <pre>
Generate the netlist using abc command
<img width="615" alt="Screenshot 2024-05-26 at 3 24 14 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/3035c361-2a06-41ca-8363-5a40d4827d87">
                </pre>
                <p>Step 9</p>
                <pre>
Write the netlist to blocking_caveat_net.v
<img width="523" alt="Screenshot 2024-05-26 at 3 25 02 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/cef23d28-8d32-4c0c-941f-a0420db792d6">
                </pre>
                <p>Step 10</p>
                <pre>
Execute show to view the design
<img width="603" alt="Screenshot 2024-05-26 at 3 25 14 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/f05de1e7-98ce-4ac3-bbaa-fbf14a31336c">
                </pre>
                <p>Step 11</p>
                <pre>
Exit yosys and load the blocking_caveat_net.v to iverilog.
<img width="1370" alt="Screenshot 2024-05-26 at 3 27 05 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/db39196e-3fba-424f-95ad-6b2f2e069d03">
                </pre>
                <p>Step 12</p>
                <pre>
Execute a.out file.
<img width="832" alt="Screenshot 2024-05-26 at 3 27 14 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/897f10b8-5491-46d7-a7ff-ca0eac28123a">
                </pre>
                <p>Step 13</p>
                <pre>
Load the generated .vcd file into GTKWave
<img width="1059" alt="Screenshot 2024-05-26 at 3 27 38 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/2f6188c0-d262-48f1-a756-24f1c709de6f">
                </pre>
                <p>Step 14</p>
                <pre>
Observe the behavior of GLS of blocking_caveat due to Simulation Mismatch
<img width="1357" alt="Screenshot 2024-05-26 at 3 28 12 PM" src="https://github.com/c-dhanush-p/SFAL-VSD/assets/170220133/4f2dcd1c-5d73-474e-bf68-12c82ff30205">
                </pre>
            </details>
        </li>
    </ul>
</details>
<!--End of Day 4-->




<details>
  <summary>🛠️ Design for Testability (DFT)</summary>
 
# **Design for Testability (DFT)**

### **What is DFT?**

Design for Testability (DFT) in VLSI involves techniques to ensure ICs can be effectively tested post-manufacturing, improving yield, reliability, and quality.

### **Why DFT?**

DFT is essential for:
- Ensuring high-quality, reliable ICs
- Efficient manufacturing and debugging
- Meeting industry standards
- Supporting testing of complex designs

### **Testing Levels:**
1. **Wafer-Level Testing:** Tests individual dies on a wafer.
2. **Package-Level Testing (Die-Level):** Tests ICs post-packaging.
3. **System-Level Testing (Board-Level):** Tests ICs within complete systems.

### **Pros and Cons of DFT:**

**Pros:**
- Enhanced Fault Detection
- Improved Product Quality and Reliability
- Cost Reduction
- Increased Yield
- Simplified Debugging and Diagnosis

**Cons:**
- Increased Design Complexity
- Area and Power Overhead
- Performance Impact
- Test Development Time
- Cost of Test Equipment

### **Basic Terminologies in DFT:**

- **Controllability:** Ease of setting a specific internal node to a desired logic value.
- **Observability:** Ability to observe internal states and outputs.
- **Fault:** An unintended defect causing incorrect behavior.
- **Error:** Deviation from expected behavior or functionality.
- **Failure:** Undesirable outcome during testing indicating a defect.
- **Fault Coverage:** Percentage of detectable faults in a circuit.
- **Defect Level:** Severity of defects and corresponding measures during testing.

### **DFT Techniques:**

![DFT Techniques](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/a731deb7-aac1-4d9a-8397-90830b5f05ff)

### **Scan Flip-Flops:**

A scan flip-flop is used in digital circuits for testing and debugging, enabling access to its input and output in scan mode.

![Scan Flip-Flop](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/c43e74d8-2587-400b-bbcf-57ddf0601da8)

### **Scan Chain Techniques:**

1. **Scan Chain Configuration:**
   - **Serial Connectivity:** Scan flip-flops connected in series.
   - **Scan-In and Scan-Out:** Test patterns loaded through SI and read through SO.

2. **Shift Operation:**
   - **Shift Mode:** Serially loads test patterns into the scan chain.
   - **Test Pattern Application:** ATPG tools generate and load test patterns for fault detection.

3. **Benefits:**
   - **Improved Testability:** Direct access to internal states.
   - **Observability and Controllability:** Better control and observation during testing.
   - **Efficient Debugging:** Allows observation and analysis of internal signals.

4. **Implementation Considerations:**
   - **Scan Chain Insertion:** Inserted during synthesis using DFT-aware tools.
   - **Test Pattern Generation:** ATPG tools generate test patterns for fault coverage.

### **ATE Functionality:**

Automatic Test Equipment (ATE) is used for testing and validating ICs and PCBs. It integrates ATPG for generating test patterns and applies them to the device under test.

![ATE Functionality](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d1922705-9d20-41b2-8f0a-33597c34d472)

### **DFT Compiler Overview:**

![DFT Compiler Overview](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/00dee519-6ff3-4ccb-b087-591b6b242121)

![DFT Compiler](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/177f6250-e888-4787-876a-f816713a3424)

### **Scan Chain Length:**

The length of scan chains varies based on design complexity and testing requirements. Optimal length balances test coverage, test time, data volume, and manufacturability.

### **Waveform Drawing:**

For drawing the waveform of a given circuit:

![Waveform](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/cf6875fb-772b-4ea3-93ea-edae230c3902)

</details>


<details>
  <summary>🛠️ Introduction to Logic Synthesis</summary>

# Introduction to Logic Synthesis

## Logic Synthesis

### What is Logic Synthesis?
- Converts RTL description of a circuit into a netlist of logic gates and their connections.
- **HDL Compiler**: Converts RTL to Generic Boolean (without timing info).
- **Design Compiler**: Converts Generic Boolean into Target Technology (with timing info).

### What is Design Compiler?
- **Design Compiler (DC)**: EDA tool by Synopsys for Synthesis.
- **Interfaces**: `dc_shell` (text), Design Vision (graphical).
- **File Formats**:
  - `.db`: Library files.
  - `.ddc`: Design information, used across various Synopsys tools.
  - **SDC**: Design constraints (power, timing, area), uses TCL script.

### Design Compiler Flow

1. Set & link .db
2. Read .v file
3. Read SDC
4. Integrate the Design
5. Synthesize
6. Report
7. Check Quality of Results (qor) files
8. Write Netlist

### Netlist & Libraries

- Design is written using standard cells (gates, mux, flops, etc.) in `.db`.
- **Target Library**: Database with standard cell information (area, pin names, timing).
- Multiple libraries can be appended using link library.

### Getting Started

- **Specify Libraries**: Target & link libraries.
- **File Formats**:
  - **GUI**: `design_vision`
  - **Non-GUI**: `dc_shell`
  - **Switching to GUI**: `gui_start/start_gui`
- `.synopsys_dc.setup`: Used for integrating libraries at startup.

### TCL Tips

- Track bracket types.
- Use `$` to refer a variable.
- No `$` needed when assigning a variable.
- `"*"`: Matches string of characters.

## Design Compiler Introduction

### PART 1: Invoking DC Basic Setup

1. **Go to work directory**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/05fd724f-6b7a-4b91-a7c9-d08cf0f2826f)
2. **Enable C Shell**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/ed468576-859b-4aec-a9be-1ac7bb2b786a)
3. **Invoke DC using `dc_shell`**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/e0006db5-dce9-4972-af55-d3affea258f9)
4. **Read the Design**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/fd189762-574c-42d0-b626-39d98ba2c467)
   - Note: The register information indicates a 1-bit Flip Flop.
5. **Read library**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/7bf6ba5d-d903-4ffd-86f2-751fbee19ad3)
6. **Set target library**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/8d9456a7-e591-4f7a-808e-47ebfc862a6f)
7. **Set link library**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/3ffff2e4-e67e-45d9-82d3-6e82bdd998be)
   - Note: Important to specify the library for design. `*` represents already loaded library.
8. **Check library locations with echo**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/fb84601c-ea20-423d-a05e-538e3e0213ad)
9. **Compile the design**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/ecfa5ff2-fcab-486f-816d-54320cf55bf4)
   ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/1d976cf9-7421-4ea4-b89b-8a49d5ecef6f)
   ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/5a2c6fd2-03d3-46a8-82e8-4fcb27105b2e)
10. **Write verilog**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/cc2249e8-5f39-48e0-bb51-69916001ec0f)
    - Note: `-f` refers to the format (verilog) of the file to be written.
11. **View the written netlist**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/66c21a3e-bb64-4e3e-b346-95b9e22b4133)
    ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/2487f1dc-b3ff-4064-8040-456244a9beae)

## PART 2: Intro to Design Vision

1. **Write .ddc in dc_shell**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d999cd31-d708-478c-a30a-61cc6ac2dd9d)
2. **Open new tab and enable C Shell**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/ed468576-859b-4aec-a9be-1ac7bb2b786a)
3. **Launch Design Compiler in GUI mode**: `design_vision`
   ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/c7194e75-d314-4626-8ed2-9a7a52bd6565)
4. **Read .ddc in Design Vision GUI**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/993077c6-e525-496e-ba38-5e0ce8e7f094)
5. **Open Schematic View and double-click to view standard cells**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/128e2c23-4b41-44b3-a92d-a3271e1b0d28)

### Design Compiler Synopsys Setup

1. **Open `.synopsys_dc.setup` using gvim**: Write commands to target and link the library.
   ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/32977e52-3883-43fa-b7bb-639019991e78)
2. **Verify by invoking the DC shell and echoing libraries**: ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/19d5fa37-213a-492f-8f6b-7bb22ea870a6)

</details>
 
<details>
  <summary>🔧 TCL Scripting Lab</summary>

# TCL Scripting Lab

Welcome to the TCL Scripting Lab! Follow these steps to get started with basic and advanced TCL commands.

## Step 1: Basic Commands

Initialize and view variables:

- **Set a Variable:**
  ```tcl
  set i 5
  ```
- **View Variable:**
  ```tcl
  echo $i
  # or
  puts $i
  ```
- **Increment Variable:**
  ```tcl
  incr i
  ```

![Basic Commands](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/752afc67-4e5f-441e-a52c-b8a11207a042)

## Step 2: Run 'For' Loop

Execute a 'for' loop to iterate over a range of values.

![For Loop](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/ce149b55-1f4c-4ca5-91d6-810e3a00ee0b)

## Step 3: Run 'While' Loop

Use a 'while' loop to execute commands as long as a condition is true.

![While Loop](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d6633c9a-cab2-4760-b461-70575cef3605)

## Step 4: Creating a List

Create a list using the `set` command.

![Creating a List](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/ba50215b-7ed6-4dce-891a-c3d547b462e7)

## Step 5: Looping through the List

Iterate over the elements of a list.

![Looping through List](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/1c00b3f9-e4f0-4699-8ffb-2f7baf2b740a)

## Step 6: Looping through a Collection

View all AND gates in a .db file and instantiate variables.

![Looping through Collection](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/b2c05b88-bbc1-4b95-8cfa-9baf5ab63ca6)

> Note: `get_lib_cells`, `get_object_name`, and `foreach_in_collection` are used in Synopsys.

## Step 7: Creating a TCL Script from DC

Launch `gvim` from within DC and edit documents with TCL commands.

![Creating TCL Script](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/a4ebd9cd-da52-4d12-aa12-d48658468cbb)

Press `i` to enter 'insert mode' and edit your document. Save the file as `testing.tcl`.

## Step 8: Executing a TCL Script from DC

Source the saved file to execute it.

![Executing TCL Script](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/5e9b63e8-307d-44a1-be45-cce2e055e30e)

Check the output to ensure all commands in `myscript.tcl` are executed correctly.

![Output](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d526ccc5-adbb-4eff-b524-9e4662e4c80b)

</details>

<details>
	<summary>Introduction to SoC </summary>

 
# System on Chip (SoC)

## What is SoC?

A System on Chip (SoC) is an integrated circuit (IC) that consolidates all components of a computer or other electronic systems into a single chip. These components typically include a central processing unit (CPU), memory, input/output ports, and secondary storage – all on a single substrate. SoCs are used in mobile devices, embedded systems, and increasingly in PCs and servers due to their compactness and efficiency.

## Advantages of SoC

1. **Compact Size**: SoCs integrate multiple components into a single chip, significantly reducing the overall size of the system.
2. **Power Efficiency**: With all components on a single chip, power consumption is reduced due to shorter interconnections and optimized power management.
3. **Performance**: Integration leads to faster communication between components, enhancing overall system performance.
4. **Cost-Effective**: Reducing the number of separate components and interconnects can lower manufacturing and assembly costs.
5. **Reliability**: Fewer components and interconnections mean fewer potential points of failure, increasing system reliability.

## Disadvantages of SoC

1. **Design Complexity**: Integrating multiple components on a single chip increases design complexity, requiring sophisticated tools and methodologies.
2. **Development Cost**: The initial development and design of SoCs can be costly, involving advanced fabrication processes.
3. **Limited Flexibility**: Once designed and manufactured, modifying or upgrading an SoC can be challenging compared to discrete component systems.
4. **Thermal Management**: High integration density can lead to thermal issues, requiring advanced cooling solutions.

## Examples of SoC

1. **Apple A14 Bionic**: Used in iPhones and iPads.
2. **Qualcomm Snapdragon 888**: Commonly used in high-end Android smartphones.
3. **NVIDIA Tegra X1**: Used in devices like the Nintendo Switch and various automotive applications.
4. **Samsung Exynos 2100**: Used in Samsung's Galaxy S21 series.
5. **Broadcom BCM2837**: Found in the Raspberry Pi 3.

## Popular Applications of SoC

1. **Mobile Devices**: Smartphones, tablets, and smartwatches.
2. **Embedded Systems**: IoT devices, automotive control systems, and industrial automation.
3. **Consumer Electronics**: Smart TVs, set-top boxes, and gaming consoles.
4. **Computing Devices**: Laptops, desktops, and servers.
5. **Networking Equipment**: Routers, switches, and modems.

## Types of SoC

1. **Application-Specific SoC (ASIC)**: Customized for specific applications like graphics processing or AI acceleration.
2. **Programmable SoC (PSoC)**: Combines programmable logic with traditional SoC components.
3. **Standard SoC**: General-purpose SoCs used in a wide range of applications, such as those found in mobile devices.

## SoC Design Flow

1. **Specification and Planning**:
    - Define the system requirements, including performance, power, area, and functionality.
    - Choose the target applications and markets.

2. **Architecture Design**:
    - Develop the overall architecture, including CPU, GPU, memory, and peripherals.
    - Define the interconnect scheme and data flow.

3. **Component Selection**:
    - Choose standard IP cores (e.g., processors, memory controllers) or design custom components.
    - Ensure compatibility and integration capability of all components.

4. **Integration and Verification**:
    - Integrate the chosen components into a unified design.
    - Perform extensive verification using simulation, emulation, and formal methods to ensure functionality and performance.

5. **Physical Design**:
    - Perform synthesis to convert the high-level design into a gate-level netlist.
    - Conduct floorplanning, placement, and routing to create the physical layout of the chip.
    - Optimize for power, performance, and area (PPA).

6. **Fabrication and Testing**:
    - Send the final design to a semiconductor foundry for fabrication.
    - Perform post-fabrication testing to ensure the chip meets specifications.

7. **Software Development**:
    - Develop and optimize software to run on the SoC, including drivers, operating systems, and applications.
    - Ensure seamless integration between hardware and software components.
  
      ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/684603bf-947f-4242-ac51-3f752b4b735f)


## SoC Structure

1. **Processing Elements**: CPUs, GPUs, and DSPs that handle computation.
2. **Memory Components**: SRAM, DRAM, and cache memory for data storage.
3. **Interconnects**: Buses, crossbars, and networks-on-chip (NoCs) for communication between components.
4. **Peripherals**: Input/output controllers, timers, and communication interfaces (e.g., USB, Ethernet).
5. **Power Management**: Circuits to manage power distribution and consumption.
6. **Security Modules**: Hardware encryption and secure boot capabilities.
7. **Analog Components**: ADCs, DACs, and other mixed-signal elements.

## Conclusion

System on Chip (SoC) technology offers significant advantages in terms of size, power efficiency, performance, and cost. However, it also presents challenges in design complexity, development cost, flexibility, and thermal management. Effective SoC design requires careful planning, integration, verification, and optimization to meet the desired specifications and achieve market success.

</details>

<details>
	
 <summary>Introduction to VSDBabySoC</summary>
 
# VSDBabySoC

## Overview

VSDBabySoC is a compact and robust RISCV-based System on Chip. The primary goal of designing this small SoC is to integrate and test three open-source IP cores together for the first time and to calibrate its analog components. VSDBabySoC includes an RVMYTH microprocessor, an 8x Phase-Locked Loop (PLL) for generating a stable clock, and a 10-bit Digital-to-Analog Converter (DAC) for communication with other analog devices.

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/76536a0c-2f25-488f-b73d-30d1210b3476)

## BabySoC Components

### RVMYTH
- **RVMYTH**: The RVMYTH core is a simple RISC V-based CPU designed for educational purposes and small-scale applications. It provides a practical example of a RISC-V processor implementation.

### PLL
- **Phase-Locked Loop (PLL)**: A phase-locked loop or PLL is a control system that generates an output signal whose phase is related to the phase of an input signal. PLLs are widely used for synchronization purposes, including clock generation and distribution.

### DAC
- **Digital-to-Analog Converter (DAC)**: A DAC is a system that converts a digital signal into an analog signal. DACs are widely used in modern communication systems, enabling the generation of digitally-defined transmission signals.

## What is a PLL?

A phase-locked loop (also phase lock loop or PLL) is a system that generates an output signal whose phase is related to its input. The two signals will have the same frequency and either no phase difference or a constant phase difference between them.

A PLL typically consists of the following components:
- **Phase Detector**: Compares the reference signal with the oscillator frequency and outputs an error signal.
- **Loop Filter**: Usually a low-pass filter that generates an error voltage from the error signal.
- **Voltage-Controlled Oscillator (VCO)**: Adjusts the oscillator frequency to lock to the input frequency, producing an output frequency equal to the input frequency with a constant phase shift.

A PLL may also include a frequency divider in its feedback loop to create an output that is a multiple of the reference frequency instead of one that is exactly equal to it.



![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/78a02602-6eac-4448-941f-598b2c212274)



## Why Off-Chip Clocks Can't Be Used All the Time?

Using off-chip clocks for all on-chip operations in VLSI design is often impractical due to several key factors:

### Clock Distribution Delays
The clock signal needs to be distributed to a multitude of blocks on the chip. If a single off-chip clock source is used, the long wires required for distribution can introduce significant delays.

### Clock Jitter
Off-chip clocks are more susceptible to clock jitter, which can degrade the performance and reliability of the system.

### Different Frequency Requirements
Various blocks on the chip may require different clock frequencies. For example, one block might need 200 MHz while another needs 100 MHz. It is challenging to accommodate these varying frequency requirements with a single off-chip clock source.

### Clock Accuracy (ppm)
Quartz crystals used for clock generation have a specified accuracy in parts per million (ppm). A higher ppm error implies greater deviation from the desired frequency, impacting timing precision in electronic systems.

### Frequency Stability
Frequency stability denotes the maximum frequency variation over the operating temperature range. Crystals with higher ppm errors may exhibit significant frequency variations with temperature changes, affecting the reliability of timing references, especially in extreme temperature conditions.

### Total Frequency Error
The total frequency error of a crystal is the sum of errors from frequency tolerance, frequency stability, and aging. A higher ppm error in any of these components can lead to a larger total frequency error, affecting the crystal's overall accuracy in maintaining precise timing references.

## Digital-to-Analog Converter (DAC)

A Digital-to-Analog Converter (DAC) converts a digital input signal into an analog output signal. The digital signal is represented by a binary code, a combination of bits 0 and 1.

### Components of a DAC
- **Binary Inputs**: The number of binary inputs of a DAC is generally a power of two.
- **Analog Output**: A single output representing the analog signal.

### Types of DACs
- **Weighted Resistor DAC**: Uses resistors with weighted values to convert the binary input into an analog output.

  ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/7969a5fa-6bb4-49c7-ba92-7bd2dc074439)
  
  

- **R-2R Ladder DAC**: Utilizes a repetitive structure of resistors with values of R and 2R to achieve the conversion.

  ![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/3b134b8b-dd13-4b1a-b30b-71335d60e010)


In our VSDBabySoC design, we are using a 10-bit DAC.

</details>

<details>
	<summary>VSDBabySoC Modelling</summary>

 # VSDBabySoC Modelling Overview

## What is Modelling?

Modelling and Simulation (M&S) involves the use of physical or logical representations of systems to generate data that informs decisions and predictions. This approach is particularly prevalent in the VLSI domain where it aids in design, analysis, verification, and validation of electronic systems.

## Purpose of Modelling

System models are crafted to:
- Support the analysis and specification of systems.
- Aid in system design and architectural planning.
- Facilitate system verification and validation.
- Enhance communication related to system details and functionality.

## Modelling the VSDBabySoC

### System Overview

The VSDBabySoC is modeled to integrate and simulate the interactions between various IP cores within a system on a chip (SoC), which includes:
- **Initial Inputs**: Entry signals fed into the VSDBabySoC module.
- **Phase-Locked Loop (PLL)**: Responsible for generating the appropriate clock signals for the system operations.
- **RVMYTH Processor**: Utilizes the clock signals to execute instructions and generate values.
- **Digital-to-Analog Converter (DAC)**: Converts the digital signals produced by the RVMYTH into analog output signals.

### Components
1. **RVMYTH**: A digital processing unit modeled using Hardware Description Language (HDL).
2. **PLL**: An analog component that generates stable clock signals.
3. **DAC**: An analog component that converts digital signals into analog outputs.

### Challenges in Modelling Mixed Signal Blocks

While digital blocks like RVMYTH can be easily modeled and simulated using HDLs such as Verilog, analog components like DACs and PLLs present a unique challenge as Verilog cannot directly synthesize analog designs. To overcome this, we use `real` data-types in Verilog to simulate analog functionalities and verify their logical correctness.

### Repositories Used as References
- [RVMYTH Repository](https://github.com/shivanishah269/risc-v-core)
- [PLL Repository](https://github.com/vsdip/rvmyth_avsdpll_interface)
- [DAC Repository](https://github.com/vsdip/rvmyth_avsddac_interface)

### Tools and Process for Modelling
RVMYTH is developed using TL-Verilog. To integrate it into our SoC model, we use tools like SandPiper SaaS to compile and transform TL-Verilog code into standard Verilog.

#### Step-by-Step Installation and Setup
1. **Install Required Packages**
   ```bash
   pip3 install pyyaml click sandpiper-saas
   ```
2. **Clone the VSDBabySoC Repository**
   ```bash
   git clone https://github.com/manili/VSDBabySoC.git
    cd VSDBabySoC
   ```

3. **Compile and Simulate the SoC Design**
   ```bash
   sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
   ```
   **sandpiper-saas** is used to to translate .tlv (transaction level verilog) files to .v (verilog) files.

   **Navigate to the VSDBabySoC directory and compile the design using Icarus Verilog**
   
   ```bash
   iverilog -o output/pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module
   ```

5. **Run the Simulation**
   Change to the output directory and execute the simulation to generate the VCD file:
   ```bash
   cd output
   ./pre_synth_sim.out
   ```

6. **Viewing the Waveform**
   Open the simulation waveform in GTKWave for analysis:
   ```bash
   gtkwave pre_synth_sim.out
   ```
<img width="1500" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/8a18adc6-70b2-42fc-bc65-8e8f39cf8320">


<img width="1500" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/04905917-6988-463e-afe8-d5c55592f645">




## Understanding the Simulation Waveform

The simulation waveform provides insights into various signals within the VSDBabySoC module:

- **CLK**: The input clock signal to the RVMYTH core, generated by the PLL.
- **reset**: An input reset signal to the RVMYTH core, sourced externally.
- **OUT**: The output signal from the VSDBabySoC module, typically provided by the DAC. Due to simulation constraints, this appears as a digital signal but represents an analog value in a real application.
- **RV_TO_DAC[9:0]**: A 10-bit output from the RVMYTH core's register #17, representing data sent to the DAC.
- **OUT (Real Data Type)**: This represents an analog output signal from the DAC, simulated as a real data type in Verilog.

### Key Points

- **Signal Origin**: The CLK and reset signals are crucial for the operational timing and initialization of the RVMYTH core.
- **Output Analysis**: The OUT signal, as well as RV_TO_DAC, show how digital values are processed and converted into analog outputs within the system, reflecting the functionality of digital-to-analog conversion.
- **Real Data Type Usage**: This simulation uses Verilog's real data types to mimic the behavior of analog signals, providing a closer approximation to how the system would perform in a real-world scenario.


</details>

<details>
	<summary> BabySoC Post-Synthesis Simulation/Gate Level Simulation </summary>

 # VSDBabySoC Simulation Guide

## Importance of Pre-Synthesis and Post-Synthesis Simulation

### Why Pre-Synthesis Simulation?

Pre-synthesis simulation, often referred to as RTL simulation, is crucial for verifying the functional correctness of a design before the synthesis process. This type of simulation:

- **Focuses on Functionality**: It checks the logic functionality according to the design and code written, without the influence of physical constraints and delays.
- **Verifies Design Intent**: Ensures that the written code behaves as expected in a zero-delay environment, where events typically occur precisely at the active clock edge.
- **Detects Logical Errors**: Helps identify issues in the design logic before moving on to the more complex stage of synthesis, making debugging simpler and less costly.

### Why Post-Synthesis Simulation?

Post-synthesis simulation, also known as gate-level simulation (GLS), is performed after the synthesis process and includes delays and physical characteristics:

- **Includes Gate Delays**: This simulation incorporates the actual delays of gates and interconnects modeled in the synthesized netlist, providing a more accurate depiction of circuit behavior under real conditions.
- **Verifies Timing and Functionality**: It helps to detect both functional and timing errors introduced during the synthesis process, such as setup and hold violations.
- **Confirms Implementation Fidelity**: Ensures that the synthesis process has faithfully translated the RTL design into a gate-level netlist without altering its intended functionality.
- **Checks for Mismatches**: Identifies mismatches that might occur due to incorrect operator usage or unintended latch inference, which are not apparent in RTL simulation.
- **Dynamic Behavior Verification**: Gate-level simulation is critical for verifying dynamic behaviors of the circuit, such as clock gating and power-on reset, which cannot be accurately checked using static timing analysis alone.

### GLS: A Brief Introduction

The term "gate level" refers to the netlist view of a circuit, typically produced after logic synthesis. This view includes:

- **Netlist**: A complete connection list consisting of gates and IP models that exhibit both functional and timing behavior.
- **Simulation Modes**: While GLS can be performed with zero delay, it is commonly executed in unit delay mode or full timing mode to reflect real operational conditions.
- **Enhanced Confidence**: Performing GLS boosts confidence in the physical implementation of a design, ensuring that it meets all specified operational and performance criteria.

By conducting both pre-synthesis and post-synthesis simulations, designers can ensure that the VSDBabySoC operates correctly from both a functional and a timing perspective, thus significantly reducing the risk of costly revisions and silicon respins.

---

# Synthesis Process for VSDBabySoC

To synthesize the VSDBabySoC design, we need to convert the standard cell libraries into a `.db` format that can be used by synthesis tools such as Synopsys Design Compiler. Here's how you can accomplish this using the Synopsys Library Compiler (lc_shell):

## Step-by-Step Guide to Convert Libraries to `.db` Format

### Setting Up Your Environment
Ensure you are in the correct directory where the libraries are stored:
```bash
cd /home/venkatesh/VSDBabySoC/src/lib
```

### Reading the Library
Load the library file into the Library Compiler:
```bash
read_lib avsddac.lib
```

### Writing to `.db` Format
Convert the library to `.db` format using the `write_lib` command:
```bash
write_lib -format db avsddac -output avsddac.db
```
<img width="1324" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/22c23f84-11a2-4ff7-9fba-69566841003f">



### Reading the Library
Load the library file into the Library Compiler:
```bash
read_lib avsdpll.lib
```

### Writing to `.db` Format
Convert the library to `.db` format using the `write_lib` command:
```bash
write_lib avsdpll -format db -output avsdpll.db
```
<img width="1092" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/a60b45f9-e55b-4f87-9483-648ab6a06a91">


### Reading the Library
Load the library file into the Library Compiler:
```bash
read_lib sky130_fd_sc_hd__tt_025C_1v80.lib
```

### Writing to `.db` Format
Convert the library to `.db` format using the `write_lib` command:
```bash
write_lib sky130_fd_sc_hd__tt_025C_1v80 -format db  -output sky130_fd_sc_hd__tt_025C_1v80.db
```

<img width="1323" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/0fac3f1d-1d25-4249-aa09-26d71b3276a7">


This process will generate a `.db` file, which is a binary format that can be read by Synopsys tools for further synthesis and analysis steps. This file will be essential for the synthesis of the VSDBabySoC, as it contains all necessary information about the standard cells used in the design, including their timing, power, and area characteristics.

---

## Synthesis Commands

The following commands are used in the Synopsys Design Compiler environment to prepare and execute the synthesis of the VSDBabySoC design:

### Setting Libraries

- **Target Library**
  ```bash
  set target_library "/home/venkatesh/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.db"
  ```
- **Link Libraries**
  ```bash
  set link_library {* /home/venkatesh/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.db /home/venkatesh/VSDBabySoC/src/lib/avsdpll.db /home/venkatesh/VSDBabySoC/src/lib/avsddac.db}
  ```

### Setting Search Path

Specifies where the tool should look for design modules and files.

```bash
set search_path {/home/venkatesh/VSDBabySoC/src/include /home/venkatesh/VSDBabySoC/src/module}
```

### Breakdown of the Command

1. **set search_path**:
   - This command sets the search path for the tool to locate the design files. The `search_path` variable tells the tool where to look for the files it needs during processing.

2. **{...}**:
   - The curly braces `{}` enclose a list of directory paths. These paths are added to the search path for the tool.

3. **/home/venkatesh/VSDBabySoC/src/include**:
   - This is the first directory path specified in the search path. It typically contains include files or header files that are used in the design.

4. **/home/venkatesh/VSDBabySoC/src/module**:
   - This is the second directory path specified in the search path. It usually contains module source files or other Verilog files needed for the design.

### Purpose of the Command

The purpose of setting the search path is to inform the synthesis or simulation tool where to find all the necessary files required for the design. By specifying these directories, the tool can automatically locate and include the files during the design process.


### Reading Design Files

Loads all relevant design files and sets the top-level design.

```bash
read_file {sandpiper_gen.vh sandpiper.vh sp_default.vh sp_verilog.vh clk_gate.v rvmyth.v rvmyth_gen.v vsdbabysoc.v} -autoread -top vsdbabysoc
```


### Breakdown of the Command

1. **read_file**:
   - This is the primary command used to read or load files into the design tool. It tells the tool to read the specified files for further processing.

2. **{sandpiper_gen.vh sandpiper.vh sp_default.vh sp_verilog.vh clk_gate.v rvmyth.v rvmyth_gen.v vsdbabysoc.v}**:
   - This is a list of files to be read by the tool. The files are enclosed in curly braces `{}` and separated by spaces. They include:
     - `sandpiper_gen.vh`: Verilog header file likely containing parameter definitions or macro definitions.
     - `sandpiper.vh`: Another Verilog header file with potentially different or additional definitions.
     - `sp_default.vh`: A header file with default settings or configurations.
     - `sp_verilog.vh`: A Verilog header file, possibly containing utility macros or includes.
     - `clk_gate.v`: A Verilog source file, likely defining a clock gating module.
     - `rvmyth.v`: A Verilog source file, likely containing the definition of the RISC-V microcontroller core (RVMyth).
     - `rvmyth_gen.v`: A generated Verilog file, likely containing synthesized or pre-processed code for RVMyth.
     - `vsdbabysoc.v`: The top-level Verilog source file for the VSDBabySoC design.

3. **-autoread**:
   - This option tells the tool to automatically determine the order in which the files should be read. It helps the tool resolve dependencies between files without requiring manual ordering by the user.

4. **-top vsdbabysoc**:
   - This option specifies the top-level module of the design. The tool will treat `vsdbabysoc` as the root module for hierarchical processing and analysis. All other modules and files will be considered as part of the hierarchy under this top-level module.

### Purpose of the Command

The purpose of this command is to load all necessary Verilog source and header files into the design tool and to specify the top-level module of the design. This setup allows the tool to process the design hierarchy correctly and to prepare it for further steps such as synthesis, simulation, or implementation.

### Usage Context

This command is typically used in the setup phase of a design flow within EDA tools such as FPGA design tools (e.g., Xilinx Vivado, Intel Quartus), ASIC design tools (e.g., Synopsys Design Compiler, Cadence Genus), or simulation tools (e.g., ModelSim, QuestaSim).

- **Read and Parse Files**: The command ensures all necessary files are read and parsed by the tool.
- **Dependency Resolution**: The `-autoread` option helps the tool automatically resolve dependencies between the included files.
- **Top-Level Specification**: The `-top vsdbabysoc` option tells the tool which module to treat as the top-level module, guiding the tool in hierarchical design processing.

<img width="1324" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/f0f03d16-4837-476d-8af2-3c44c68ce701">



### Linking and Compilation

- **Linking**
  ```bash
  link
  ```

<img width="1324" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/9bdaa4c5-7b16-4c4f-91b9-3b63d473816b">


  This command links the design with the libraries and resolves all instantiations.

- **Ultra Compilation**
  ```bash
  compile_ultra
  ```

The purpose of the `compile_ultra` command is to perform a high-effort synthesis of a digital design. This process involves various optimization techniques and advanced algorithms to meet the design constraints more effectively than the standard `compile` command.

  
<img width="1324" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/3e5de126-f883-431b-8ccd-768c9b03d353">

---

### Writing Out the Netlist

Outputs the synthesized netlist in Verilog format to the specified location.

```bash
write_file -format verilog -hierarchy -output /home/venkatesh/VSDBabySoC/output/vsdbabysoc_net.v
```

### Breakdown of the Command

1. **write_file**:
   - This is the primary command used to write a file. It is typically used in synthesis or design tools to export a design into a file.

2. **-format verilog**:
   - This option specifies the format of the output file. In this case, `verilog` indicates that the file should be written in Verilog format.

3. **-hierarchy**:
   - This option indicates that the file should include the hierarchical structure of the design. It means the generated Verilog file will contain the full hierarchy, showing how modules are instantiated within other modules.

4. **-output /home/venkatesh/VSDBabySoC/output/vsdbabysoc_net.v**:
   - This specifies the path and filename for the output file. The `-output` option indicates that the next argument is the location where the file should be saved. In this case, the file will be saved as `vsdbabysoc_net.v` in the specified directory.

### Purpose of the Command

The purpose of this command is to export a design into a Verilog netlist file with hierarchical information included. This is useful for various purposes, such as:

- **Post-Synthesis Simulation**: The generated netlist can be used to simulate the design after synthesis to verify its behavior with gate-level accuracy.
- **Documentation**: The hierarchical netlist can serve as a detailed document of the design structure.
- **Verification**: The netlist can be used as an input to formal verification tools to ensure that the synthesized design matches the intended high-level design.

### Usage Context

This command is often used in electronic design automation (EDA) tools such as synthesis tools (e.g., Synopsys Design Compiler, Cadence Genus) or in scripts that automate parts of the design flow. It takes the synthesized design and writes it out as a Verilog netlist file, including all hierarchical information, which can then be used for further simulation, verification, or documentation purposes.

<img width="1324" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/1f2eb488-b28e-47d9-bbdc-798217f3217b">

---

## Post-Synthesis Simulation Commands

Execute the following commands in the `/VSDBabySoC` directory to perform the post-synthesis simulation:

### Compile the Design

Use `iverilog` to compile the design files, specifying functional and unit delay definitions. This command generates the simulation executable `post_synth_sim.out`.

```bash
iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 -o ./output/post_synth_sim.out ./src/gls_model/primitives.v ./src/gls_model/sky130_fd_sc_hd.v ./output/vsdbabysoc_net.v ./src/module/avsdpll.v ./src/module/avsddac.v ./src/module/testbench.v
```

### Breakdown of the Command

1. **iverilog**:
   - This is the command to invoke Icarus Verilog, the tool used for compiling Verilog source files into a simulation executable.

2. **-DFUNCTIONAL**:
   - This defines a macro called `FUNCTIONAL` during compilation. It is often used to conditionally include or exclude parts of the code, allowing for different versions or modes of the design (e.g., functional simulation vs. synthesis).

3. **-DUNIT_DELAY=#1**:
   - This defines a macro `UNIT_DELAY` with a value of `#1`. This can be used to standardize delays across the simulation. The `#1` syntax indicates a time unit delay of 1 time unit, often used for simplifying timing analysis and simulations.

4. **-o ./output/post_synth_sim.out**:
   - This specifies the output file for the compiled simulation. The `-o` flag indicates that the next argument is the name of the output file. In this case, `./output/post_synth_sim.out` is the file where the compiled simulation will be written.

5. **./src/gls_model/primitives.v**:
   - This is one of the Verilog source files included in the compilation. It likely contains basic primitives used by the design.

6. **./src/gls_model/sky130_fd_sc_hd.v**:
   - This file probably contains the standard cell definitions from the SkyWater 130nm PDK (Process Design Kit), specifically for the high-density standard cell library.

7. **./output/vsdbabysoc_net.v**:
   - This file contains the netlist of the `vsdbabysoc` design after synthesis. The netlist is a detailed list of the components and connections in the design, generated from the high-level Verilog code.

8. **./src/module/avsdpll.v**:
   - This file contains the Verilog code for the PLL (Phase-Locked Loop) module used in the design.

9. **./src/module/avsddac.v**:
   - This file contains the Verilog code for the DAC (Digital-to-Analog Converter) module used in the design.

10. **./src/module/testbench.v**:
    - This is the testbench file, which contains the code to simulate and verify the functionality of the design. It typically includes stimulus and expected results for the modules under test.

### Purpose of the Command
The purpose of this command is to compile various Verilog source files, including primitives, standard cells, synthesized netlist, specific modules (PLL and DAC), and a testbench, into a single simulation executable. This executable (`post_synth_sim.out`) can then be run to simulate the post-synthesis behavior of the design, including the functionality of the individual modules and the overall system, as described by the testbench. The `-DFUNCTIONAL` and `-DUNIT_DELAY=#1` flags are used to control the simulation mode and timing characteristics.

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/0ef482dd-4bbf-439e-916a-009a70f9fe77)


From the snippet, it appears we encountered errors during the elaboration phase of our Verilog simulation with Icarus Verilog. The errors indicate that specific ports (`GND`, `VDD`, `VSSA`, and `VDDA`) are not found in the modules `pll` and `dac`:

```
./output/vsdbabysoc_net.v:5479: error: port `GND` is not a port of pll.
./output/vsdbabysoc_net.v:5479: error: port `VDD` is not a port of pll.
./output/vsdbabysoc_net.v:5481: error: port `VSSA` is not a port of dac.
./output/vsdbabysoc_net.v:5481: error: port `VDDA` is not a port of dac.
4 error(s) during elaboration.
```

### Resolution Steps

1. **Verify Module Declarations**:
   - Ensure that the `pll` and `dac` modules in their respective Verilog files (`avsdpll.v` and `avsddac.v`) have the correct port declarations, including `GND`, `VDD`, `VSSA`, and `VDDA`.

2. **Check Connectivity in Netlist**:
   - Verify that the synthesized netlist (`vsdbabysoc_net.v`) correctly connects these ports to the `pll` and `dac` modules.

3. **Cross-Check with Library Files**:
   - Ensure that the ports `GND`, `VDD`, `VSSA`, and `VDDA` are defined in the corresponding library files (`avsdpll.lib` and `avsddac.lib` files) and match those expected by the synthesized netlist.

4. **Modify Verilog Files**:
   - If the ports are indeed missing, we can add them to the module declarations.

After modifying the Verilog files, we can re-run your simulation command:

```bash
iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 -o ./output/post_synth_sim.out ./src/gls_model/primitives.v ./src/gls_model/sky130_fd_sc_hd.v ./output/vsdbabysoc_net.v ./src/module/avsdpll.v ./src/module/avsddac.v ./src/module/testbench.v
```

By ensuring the correct port declarations are present in our Verilog source files, we should resolve the elaboration errors and successfully simulate your design. If the issues persist, double-check the module instantiations in the netlist file and ensure all connections are properly defined.


### Run the Simulation

Navigate to the output directory and run the simulation executable to generate the `dump.vcd` file.

```bash
cd output
./post_synth_sim.out
```

### Open the Simulation Waveform

Use GTKWave to visualize the simulation results stored in `dump.vcd`.

```bash
gtkwave dump.vcd
```


## Observing the Results

The images below show that our post-synthesis (top) and pre-synthesis (bottom) simulation results are identical, confirming the functional equivalence of the design before and after synthesis.

 **Post-Synthesis:**

![Post-Synthesis Image 1](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d977ee32-04c1-40af-b8c3-f7711d300a2f)

![Post-Synthesis Image 2](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/9b4efa1a-57c7-49a2-81a0-98c202d275f1)

 **Pre-Synthesis:**

![Pre-Synthesis Image 1](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/8a18adc6-70b2-42fc-bc65-8e8f39cf8320)

![Pre-Synthesis Image 2](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/04905917-6988-463e-afe8-d5c55592f645)

*Post-synthesis simulation waveform (top) and pre-synthesis simulation waveform (bottom).*

---


## Summary

These steps validate that the post-synthesis behavior of the VSDBabySoC design matches the pre-synthesis expectations, ensuring the correctness of the synthesis process.

</details>


<details>
	<summary>Synopsys DC and Timing Analysis</summary>

 # Understanding PVT Corners in Semiconductor Design


## Introduction
In semiconductor design, ensuring the reliability and performance of integrated circuits (ICs) across various conditions is crucial. This README provides an overview of PVT (Process, Voltage, Temperature) corners, explaining what they are, their importance, and why they are used in design and testing.

## What is PVT?
PVT stands for Process, Voltage, and Temperature, three critical parameters that influence the behavior and performance of semiconductor devices. 

- **Process**: Refers to variations in the manufacturing process. No two chips are exactly the same due to slight differences in the fabrication process.
- **Voltage**: Refers to the operating voltage supplied to the device. Variations in supply voltage can affect the performance and power consumption of the IC.
- **Temperature**: Refers to the operating temperature of the device. Temperature changes can impact the speed, leakage, and overall functionality of the semiconductor.


![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/91e8ccab-56a7-4cd4-9678-96107aada0b8)


## PVT Corners
PVT corners are specific combinations of the extreme values of Process, Voltage, and Temperature used to simulate and test the performance of semiconductor devices under worst-case scenarios. These corners help in predicting the behavior of ICs in real-world conditions.

### Common PVT Corners:
1. **SS (Slow-Slow)**: Both the NMOS and PMOS transistors are slow. Represents the worst-case scenario for speed.
2. **FF (Fast-Fast)**: Both the NMOS and PMOS transistors are fast. Represents the best-case scenario for speed.
3. **TT (Typical-Typical)**: Represents typical or nominal process conditions.
4. **SF (Slow-Fast)**: NMOS is slow, and PMOS is fast.
5. **FS (Fast-Slow)**: NMOS is fast, and PMOS is slow.

### Voltage and Temperature Extremes:
- **Voltage Corners**: Nominal voltage, maximum voltage (e.g., +10% of nominal), and minimum voltage (e.g., -10% of nominal).
- **Temperature Corners**: Typical operating temperature (e.g., 25°C), high temperature (e.g., 125°C), and low temperature (e.g., -40°C).

By combining these extremes, designers create various PVT corners, such as:
- **SS, Nominal Voltage, High Temperature**
- **FF, Max Voltage, Low Temperature**
- **TT, Min Voltage, Typical Temperature**

## Why Corners?
PVT corners are essential for several reasons:

1. **Reliability**: Ensures the IC operates correctly under different environmental and manufacturing conditions.
2. **Performance Verification**: Validates that the IC meets speed, power, and functionality requirements across all conditions.
3. **Yield Improvement**: Helps in identifying potential failure points, thereby improving the overall yield of the manufacturing process.
4. **Optimization**: Assists in optimizing the design for power, performance, and area by understanding the limits of the IC's operation.


Understanding PVT corners with real-world examples can provide clarity on their impact on semiconductor device performance. Let's consider a practical scenario involving a digital circuit, such as a simple CMOS inverter, and examine how PVT corners affect its operation.

## PVT Corners in a CMOS Inverter Example

A CMOS inverter consists of an NMOS and a PMOS transistor. The performance of this inverter can vary significantly based on process, voltage, and temperature variations.

### 1. Process Variation

Process variations arise due to manufacturing inconsistencies, affecting transistor dimensions and threshold voltages. Typical process corners are:
- **SS (Slow-Slow)**: Both NMOS and PMOS transistors are slow, resulting in slower switching.
- **FF (Fast-Fast)**: Both NMOS and PMOS transistors are fast, resulting in faster switching.
- **TT (Typical-Typical)**: Represents nominal process conditions.

### 2. Voltage Variation

Voltage variations refer to changes in the supply voltage (VDD). Typical voltage corners include:
- **Min Voltage (e.g., 0.9V)**: Lower supply voltage, reducing the drive strength of transistors.
- **Nominal Voltage (e.g., 1.0V)**: Standard operating voltage.
- **Max Voltage (e.g., 1.1V)**: Higher supply voltage, increasing the drive strength of transistors.

### 3. Temperature Variation

Temperature variations affect the carrier mobility and threshold voltage. Typical temperature corners include:
- **Low Temperature (e.g., -40°C)**: Increases carrier mobility but may affect threshold voltage.
- **Typical Temperature (e.g., 25°C)**: Standard room temperature.
- **High Temperature (e.g., 125°C)**: Decreases carrier mobility and increases leakage currents.

### Combining PVT Corners

Let's see how combining these variations into PVT corners can impact the inverter's performance:

### Example Scenario

Assume we have a CMOS inverter with the following nominal characteristics at TT, Nominal Voltage, and Typical Temperature:
- **Propagation Delay (Tpd)**: 100 ps
- **Power Consumption**: 10 µW

#### PVT Corners Analysis

1. **SS, Min Voltage, High Temperature (Worst-Case Delay)**
   - Process: Slow transistors
   - Voltage: 0.9V
   - Temperature: 125°C
   - **Impact**:
     - Increased propagation delay (e.g., 150 ps)
     - Reduced drive strength
     - Increased leakage power

2. **FF, Max Voltage, Low Temperature (Best-Case Delay)**
   - Process: Fast transistors
   - Voltage: 1.1V
   - Temperature: -40°C
   - **Impact**:
     - Reduced propagation delay (e.g., 70 ps)
     - Increased drive strength
     - Reduced leakage power

3. **TT, Nominal Voltage, Typical Temperature (Nominal Case)**
   - Process: Typical transistors
   - Voltage: 1.0V
   - Temperature: 25°C
   - **Impact**:
     - Nominal propagation delay (100 ps)
     - Nominal power consumption

4. **SF, Nominal Voltage, Typical Temperature (Mixed Process)**
   - Process: Slow NMOS, Fast PMOS
   - Voltage: 1.0V
   - Temperature: 25°C
   - **Impact**:
     - Slightly increased delay (e.g., 110 ps)
     - Imbalanced rise and fall times

5. **FS, Nominal Voltage, Typical Temperature (Mixed Process)**
   - Process: Fast NMOS, Slow PMOS
   - Voltage: 1.0V
   - Temperature: 25°C
   - **Impact**:
     - Slightly decreased delay (e.g., 90 ps)
     - Imbalanced rise and fall times

### Summary of Impact

- **Propagation Delay**: Varies significantly across different PVT corners. Designers must ensure that the circuit meets timing requirements in the worst-case scenario.
- **Power Consumption**: Leakage power increases at higher temperatures and lower supply voltages. Dynamic power depends on the switching activity and supply voltage.
- **Reliability**: Ensuring the circuit operates correctly across all PVT corners is crucial for robust design.

### Conclusion

By analyzing the performance of a CMOS inverter across various PVT corners, we can understand how process, voltage, and temperature variations affect its operation. This approach helps designers ensure that their circuits are reliable and meet performance specifications under all possible conditions.



---

# Converting Timing Libraries from .lib to .db


## Steps

### 1. Download Timing Libraries

Download the timing libraries from the GitHub repository to the specified path on your local machine.

#### Download Path

`/home/venkatesh/VSDBabySoC/src/lib_files`

#### GitHub Repository

[SkyWater PDK Libraries - SKY130 High Density Standard Cells](https://github.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/tree/master/timing)

You can download the files manually or use the following command:

```sh
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ff_100C_1v65.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ff_n40C_1v76.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ss_n40C_1v28.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ss_n40C_1v60.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ff_100C_1v95.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ff_n40C_1v95.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ss_n40C_1v35.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ss_n40C_1v76.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ff_n40C_1v56.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ss_100C_1v40.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ss_n40C_1v40.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__tt_025C_1v80.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ff_n40C_1v65.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ss_100C_1v60.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__ss_n40C_1v44.lib
wget -P /home/venkatesh/VSDBabySoC/src/timing_libs https://raw.githubusercontent.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/master/timing/sky130_fd_sc_hd__tt_100C_1v80.lib
```

### 2. Convert .lib Files to .db Files

Use the following TCL script to automate the conversion of `.lib` files to `.db` files. The script reads each `.lib` file from the source directory and writes the corresponding `.db` file to the target directory.

**Syntax to convert .lib to .db for each**

```sh
cd /home/venkatesh/VSDBabySoC/src/lib_files
lc_shell
read_lib sky130_fd_sc_hd__ff_100C_1v65.lib
write_lib sky130_fd_sc_hd__ff_100C_1v65 -format db -output sky130_fd_sc_hd__ff_100C_1v65.db
```
To process multiple `.lib` files manually would be very time-consuming, so I automated the conversion using a TCL script.

#### TCL Script

```tcl
# Base directory where .lib files are located
set base_dir /home/venkatesh/VSDBabySoC/src/lib_files

# Directory to store the converted .db files
set db_dir /home/venkatesh/VSDBabySoC/src/db_files # ( Already created a db_files directory)



# List of .lib files to be converted
set lib_files {
    sky130_fd_sc_hd__ff_100C_1v65.lib
    sky130_fd_sc_hd__ff_n40C_1v76.lib
    sky130_fd_sc_hd__ss_n40C_1v28.lib
    sky130_fd_sc_hd__ss_n40C_1v60.lib
    sky130_fd_sc_hd__ff_100C_1v95.lib
    sky130_fd_sc_hd__ff_n40C_1v95.lib
    sky130_fd_sc_hd__ss_n40C_1v35.lib
    sky130_fd_sc_hd__ss_n40C_1v76.lib
    sky130_fd_sc_hd__ff_n40C_1v56.lib
    sky130_fd_sc_hd__ss_100C_1v40.lib
    sky130_fd_sc_hd__ss_n40C_1v40.lib
    sky130_fd_sc_hd__tt_025C_1v80.lib
    sky130_fd_sc_hd__ff_n40C_1v65.lib
    sky130_fd_sc_hd__ss_100C_1v60.lib
    sky130_fd_sc_hd__ss_n40C_1v44.lib
    sky130_fd_sc_hd__tt_100C_1v80.lib
}

# Loop through each .lib file and convert to .db format
foreach lib_file $lib_files {
    # Construct the full path to the .lib file
    set lib_path $base_dir/$lib_file
    
    # Extract the base name (without extension) for output file naming
    set base_name [file rootname $lib_file]
    
    # Read the .lib file
    read_lib $lib_path
    
    # Write the .db file to the db_files directory
    write_lib $base_name -format db -output "$db_dir/${base_name}.db"
}

# Print completion message
echo "All .lib files are converted to .db format and stored in $db_dir."
```

<img width="1023" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/9a4be7d6-fbef-42a4-801b-921eb69007b9">


### Instructions to Run the TCL Script

1. **Ensure Directories Exist**:
   - Verify that the `/home/venkatesh/VSDBabySoC/src/lib_files` directory contains the downloaded `.lib` files.
   - The script will create the `/home/venkatesh/VSDBabySoC/src/db_files` directory if it does not exist.

2. **Save the Script**:
   Save the script as `convert_libs_to_db.tcl`.

3. **Execute the Script in LC Shell**:
   ```sh
   lc_shell source convert_libs_to_db.tcl
   ```

<img width="930" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/d42ed010-7250-49ef-81e7-176dc0d6cd42">


# VSDBabySoC Synthesis Process

## Synthesis Commands

### Step-by-Step Process for ff_100C_1v65

```bash
set target_library /home/venkatesh/VSDBabySoC/src/db_files/sky130_fd_sc_hd__ff_100C_1v65.db
set link_library {* /home/venkatesh/VSDBabySoC/src/db_files/sky130_fd_sc_hd__ff_100C_1v65.db  /home/venkatesh/VSDBabySoC/src/lib/avsdpll.db /home/venkatesh/VSDBabySoC/src/lib/avsddac.db}
set search_path {/home/venkatesh/VSDBabySoC/src/include /home/venkatesh/VSDBabySoC/src/module}

read_file {sandpiper_gen.vh sandpiper.vh sp_default.vh sp_verilog.vh clk_gate.v rvmyth.v rvmyth_gen.v vsdbabysoc.v} -autoread -top vsdbabysoc
link
read_sdc /home/venkatesh/VSDBabySoC/src/sdc/vsdbabysoc_synthesis.sdc
compile_ultra
write_file -format verilog -hierarchy -output /home/venkatesh/VSDBabySoC/output/vsdbabysoc_net_ff_100C_1v65.v
report_qor > ../report/timing_report_ff_100C_1v65.txt

get_attribute [get_timing_paths -delay_type max -max_paths 1] slack
get_attribute [get_timing_paths -delay_type min -max_paths 1] slack
```

Repeat the above commands for every `.db` file by adjusting the `target_library` and `output` paths accordingly.

## Graphical Representation of WNS and WHS with Respect to PVT Corners

The data is plotted to visualize the Worst Negative Slack (WNS) and Worst Hold Slack (WHS) across different PVT (Process, Voltage, Temperature) corners:

<img width="272" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/a4bf9b3d-a7a7-4138-9117-1c963e339e13">


### Visualization

The graph below provides a visual comparison of the Worst Negative Slack (WNS) and Worst Hold Slack (WHS) across different PVT conditions:


![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/45da0dc4-53c9-42ed-a679-96e20ba92850)





The plot provides a clear visual comparison of WNS and WHS across different PVT conditions, helping to identify the conditions where timing issues might occur.


## Conclusion

By following these steps, you will download the required timing libraries and convert them from `.lib` to `.db` format using the provided automated TCL script. This process simplifies the handling of timing libraries, ensuring they are ready for use in your design workflows. Additionally, performing synthesis and timing analysis using different PVT corners allows for a comprehensive understanding of the design's performance under various conditions. The graphical representation of WNS and WHS aids in identifying potential timing issues and optimizing the design accordingly.

</details>

<details>
	<summary>VSDBabySoC: Floorplanning and Power Planning</summary>
	
 ### Physical Design Flow 

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/c0551561-d567-4423-a6e0-3a6f776198ec)



# Physical Design Flow for VSDBabySoC

Physical Design is the process of translating the gate-level netlist into a physical layout. This layout consists of various metal shapes and sizes that can be drawn onto masks and manufactured on the silicon wafer.

The Physical Design process can be broken down into multiple stages, as illustrated below. It is often an iterative process where a number of optimizations are performed at each step to meet the design performance, area, and power requirements.

## Floorplanning

**Floorplanning** is the first step of physical design. The design is partitioned into various smaller subsystems based on the system architecture and design requirements. Floorplanning determines the aspect ratio and area of the layout. Here we create the placement rows for standard cells and fix the placement of I/Os around the boundary. Any macros in the design are also placed during the floorplan stage.

### Key Aspects of Floorplanning
- **Inputs**: Gate level netlist (.v), Physical & Logical Libraries (.lefs & .libs), Synopsys design constraints (.sdc), RC Tech File (TLU+ file), Technology File (.tf), Physical Partitioning information, and Floorplanning Parameters.
- **Outputs**: Die/Core Area, IO Pad Information, Placed Macros Information, Standard Cell Placement Areas, Power Grid Design, and Blockages.

### Types of Floorplan Techniques
1. **Abutted Floorplan**: Channel-less placement of blocks with no gaps.
2. **Non-Abutted Floorplan**: Blocks are placed with gaps and connected through routing nets.
3. **Mix of Both**: Combines abutted and non-abutted techniques.

![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/76ca6fa7-3a6b-48b1-906e-c6ee8671ab8a)


### Floorplan Steps
1. Define Width and Height
2. IO Pin Placement
3. Power Planning
4. Macro Placement
5. Standard Cell Row Creation
6. Blockages Definition


![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/176980af-1e0b-456a-9827-a689996591b3)


## Logic Placement

**Logic Placement** involves placing all the standard cells in the design at legal locations. After placement, EDA tools perform several optimizations to improve placement and reduce congestion.

### Example
- **Inputs**: Floorplan outputs, design constraints.
- **Outputs**: Placed standard cells, optimized layout.

## Clock Tree Synthesis (CTS)

During Floorplanning & placement, the clock is considered an ideal network. **CTS** creates a clock network to distribute the clock signal to all flops, achieving zero/minimum skew.

### Example
- **Inputs**: Placed standard cells.
- **Outputs**: Clock tree network, buffers, and inverters.

## Routing

Once standard cells are placed and the clock network is synthesized, all the connecting data nets are laid out on the metal layers. **Routing** involves connecting these nets, followed by optimizations based on design timing requirements.

### Example
- **Inputs**: Placed standard cells, clock tree network.
- **Outputs**: Routed nets, optimized design.

## Timing Analysis & Signoff

After routing, **Static Timing Analysis (STA)** is performed. This step breaks down the design into timing paths, calculates signal propagation delays, and checks for timing violations.

### Example
- **Inputs**: Routed design.
- **Outputs**: Timing reports, identified violations, optimized design for timing.

## Physical Verification & Signoff

After routing, the layout must be verified to ensure correct electrical and logical functionality. **Physical Verification** includes various checks like DRC, LVS, ERC, and more.

### Example
- **Inputs**: Final routed design.
- **Outputs**: Verified design, GDSII/OASIS file for fabrication.


## Power Planning

**Power planning** creates a power distribution network (PDN) to provide power to all chip components. It ensures stable voltage, avoids electromigration and self-heating, and meets IR drop targets.


### Power Distribution Hierarchy
1. **Power Pads**: Bring power from outside the chip.
2. **Power Rings**: Around the core area.
3. **Power Stripes**: Distribute power across the core.
4. **Power Rails**: Connect power stripes to standard cells.

### Key Steps in Power Planning
1. **Calculating power pins required**.
2. **Determining power rings, stripes, and rails**.
3. **Sizing power rings and stripes**.
4. **Analyzing and mitigating IR drop**.


![image](https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/cd8656e1-97c4-44cb-81ed-375cf3c9a2fa)

### Lab - Floorplanning of VSDBabySoC

#### Downloading Physical Design Collaterals

Clone the repositories to get the necessary technology and setup files:

```bash
git clone https://github.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/tree/master
git clone https://github.com/bharath19-gs/synopsys_ICC2flow_130nm
git clone https://github.com/kunalg123/icc2_workshop_collaterals
```

The Interconnect Technology Format (ITF) file is crucial for physical design. It contains information about conductor and dielectric layers, including:
- Thickness, minimum width, and minimum spacing of each conductor layer.
- Sheet resistance (RPSQ) of each conductor layer.
- Thickness and dielectric constant (ER) of each dielectric layer.
- Resistivity (RHO) and area (AREA) of each via layer.

To convert the `.itf` file to `.tluplus` format, use the following commands:
```bash
cd /home/venkatesh/VSDBabySoC/synopsys_ICC2flow_130nm/synopsys_skywater_flow_nominal/itf_files
grdgenxo -itf2TLUPlus -i skywater130.nominal.itf -o skywater130.nominal.tluplus
```
The `grdgenxo` command is part of the Synopsys tool suite and is specifically used for generating grid data files, such as TLU+ (Table Lookup) files, from Interconnect Technology Format (ITF) files. The command's name likely stands for **Grid Generator Extended Output** or something similar, reflecting its function of generating extended output files that are essential for accurate physical design and analysis in electronic design automation (EDA).

Here’s a detailed explanation of its components and typical usage:

### Components of `grdgenxo`

1. **Grid Generation:**
   The primary function of `grdgenxo` is to generate grid data that describes the electrical characteristics of the interconnect layers (metal layers and dielectrics) in a semiconductor process.

2. **Input Format - ITF:**
   The input to `grdgenxo` is usually an ITF (Interconnect Technology Format) file. This file contains detailed descriptions of the metal layers, including thickness, width, spacing, resistance per square (RPSQ), and dielectric properties.

3. **Output Format - TLU+:**
   The output is typically a TLU+ file, which provides a detailed table lookup for resistance and capacitance values across different conditions. These files are used by place-and-route tools and other EDA software to model the interconnects accurately.

### Typical Usage

- **Conversion Command:**
  The command to convert an ITF file to a TLU+ file typically looks like this:

  ```bash
  grdgenxo -itf2TLUPlus -i <input.itf> -o <output.tluplus>
  ```

  Here, `-itf2TLUPlus` specifies the type of conversion, `-i` specifies the input ITF file, and `-o` specifies the output TLU+ file.

### Example Workflow

1. **Prepare ITF File:**
   Ensure that the ITF file, which contains the technology-specific parameters for the metal and dielectric layers, is ready. This file should include details such as thickness, minimum width, minimum spacing, resistance per square, and dielectric constants.

2. **Run the Command:**
   Execute the `grdgenxo` command to generate the TLU+ file.

   ```bash
   grdgenxo -itf2TLUPlus -i skywater130.nominal.itf -o skywater130.nominal.tluplus
   ```

3. **Verify Output:**
   Check the output directory for the generated TLU+ file and verify its contents to ensure it includes the expected resistance and capacitance data.

4. **Integration with EDA Tools:**
   Use the generated TLU+ file in your physical design flow. Tools like Synopsys ICC2, Cadence Innovus, or others will use this file for accurate modeling of interconnects during placement, routing, and timing analysis.

### Summary

- **grdgenxo**: A command-line utility used for generating grid data files, particularly TLU+ files, from ITF files.
- **ITF File**: Input file containing detailed descriptions of metal layers and dielectric properties.
- **TLU+ File**: Output file providing a table lookup for resistance and capacitance values, used by EDA tools for accurate interconnect modeling.

By using `grdgenxo`, designers can ensure that their physical design tools have precise interconnect models, leading to improved performance, reliability, and manufacturability of the integrated circuits.


### Synthesis Flow for VSDBabySoC

The synthesis flow converts the high-level RTL design into a gate-level netlist. Below are the steps and commands used for synthesizing the VSDBabySoC design.

1. **Set the Target Library**: Specify the target library that contains the standard cell definitions.
    ```tcl
    set target_library /home/venkatesh/VSDBabySoC/src/db_files/sky130_fd_sc_hd__tt_025C_1v80.db
    ```

2. **Set the Link Library**: Define the libraries needed for linking, including the target library and additional libraries.
    ```tcl
    set link_library {* /home/venkatesh/VSDBabySoC/src/db_files/sky130_fd_sc_hd__tt_025C_1v80.db  /home/venkatesh/VSDBabySoC/src/lib/avsdpll.db /home/venkatesh/VSDBabySoC/src/lib/avsddac.db}
    ```

3. **Set the Search Path**: Define the paths where the synthesis tool should search for include files and modules.
    ```tcl
    set search_path {/home/venkatesh/VSDBabySoC/src/include /home/venkatesh/VSDBabySoC/src/module}
    ```

4. **Read the Design Files**: Read the Verilog files that make up the design.
    ```tcl
    read_file {sandpiper_gen.vh sandpiper.vh sp_default.vh sp_verilog.vh clk_gate.v rvmyth.v rvmyth_gen.v vsdbabysoc.v} -autoread -top vsdbabysoc
    ```

5. **Link the Design**: Link the design to ensure all references are resolved.
    ```tcl
    link
    ```

6. **Read the SDC File**: Read the Synopsys Design Constraints (SDC) file to apply timing constraints.
    ```tcl
    read_sdc /home/venkatesh/VSDBabySoC/src/sdc/vsdbabysoc_synthesis.sdc
    ```

7. **Compile the Design**: Perform the synthesis to generate the gate-level netlist.
    ```tcl
    compile_ultra
    ```

8. **Write the Netlist**: Write the synthesized netlist to a Verilog file.
    ```tcl
    write_file -format verilog -hierarchy -output /home/venkatesh/VSDBabySoC/output/vsdbabysoc_net_tt_025C_1v80.v
    ```

9. **Generate Reports**: Create reports for Quality of Results (QoR), power, and area.
    ```tcl
    report_qor > ./report/timing_post_synth.txt
    report_power > ./report/power_post_synth.rpt
    report_area > ./report/area_post_synth.rpt
    ```

10. **Write the Design**: Save the design in DDC format.
    ```tcl
    write -f ddc -out /home/venkatesh/VSDBabySoC/output/vsdbabysoc.ddc
    ```

---

By following these steps, the VSDBabySoC design is synthesized, and various reports are generated to evaluate the quality, power, and area of the synthesized design. This synthesis flow ensures that the design is optimized and ready for further stages in the physical design process.


### Schematics VSDBabySoC


<img width="1512" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/104fba22-e5d2-462e-a609-88314ab362f5">

---

<img width="1512" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/637e8b9b-f3e7-4b79-836c-68ab8e4c83e7">

---

<img width="1512" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/7a39b122-b320-4292-b7db-506876cee75b">


---

<img width="1512" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/23cf7a01-34fc-4f6e-9a42-d36e6e07b9db">

---

<img width="1512" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/927bfa59-c45f-4c4c-b8d8-af2d4159882a">

---

<img width="1512" alt="image" src="https://github.com/vpamidi9/sfal-vsd-venkatesh/assets/122497575/275be4c9-bedb-4ad9-96b7-d308a921e57b">

---

# VSDBabySoC: Physical Design Collaterals and Floorplan Options


## Collaterals Setup

The collaterals can be set up in the following files located at `/home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/`:

- `compile_pg_example.tcl`
- `init_design.mcmm_example.auto_expanded.tcl`
- `init_design.read_parasitic_tech_example.tcl`
- `init_design.tech_setup.tcl`
- `pns_example.tcl`
- `top.tcl`
- `write_block_data.tcl`





### Modifications in `top.tcl`


  ![top.tcl - Graphic 1](https://github.com/user-attachments/assets/85ed3e0d-20d7-446b-94f0-a72a5c8175e6)


  ![top.tcl - Graphic 2](https://github.com/user-attachments/assets/a3694b0f-1f33-453f-8c19-62125f4b8317)


  ![top.tcl - Graphic 3](https://github.com/user-attachments/assets/aebbf70f-3232-4d05-8610-85bd6491af85)


  ![top.tcl - Graphic 4](https://github.com/user-attachments/assets/3bc90fa4-63e5-4ca6-bc26-0f376d6022db)


  ![top.tcl - Graphic 5](https://github.com/user-attachments/assets/dbf57242-3175-4821-88a6-fe9eb8d23360)

### Modifications in `icc2_common_setup.tcl`


  ![icc2_common_setup.tcl - Graphic 1](https://github.com/user-attachments/assets/d3118ca6-3cae-4361-be4c-db841ff51da1)


  ![icc2_common_setup.tcl - Graphic 2](https://github.com/user-attachments/assets/6ca6ebb4-9d96-4b15-8cd5-ed734e9b7ec4)


  ![icc2_common_setup.tcl - Graphic 3](https://github.com/user-attachments/assets/cb2d6350-d751-4abc-a0e6-0017b5fd521a)
  ![icc2_common_setup.tcl - Graphic 4](https://github.com/user-attachments/assets/eda22bf1-a1fe-418f-96bc-5ae0e42481a2)
  ![icc2_common_setup.tcl - Graphic 5](https://github.com/user-attachments/assets/7bf1df9e-f4ce-4ffc-baf7-3624f2ac589a)
  ![icc2_common_setup.tcl - Graphic 6](https://github.com/user-attachments/assets/ff4fef1d-0c1b-4ad3-9591-ae09a6319cff)

### Changes in `init_design.read_parasitic_tech_example.tcl`


  ![init_design.read_parasitic_tech_example.tcl](https://github.com/user-attachments/assets/49f2d88f-29ce-4785-a8fe-d38640a73b16)

### Changes in `init_design.mcmm_example.auto_expanded.tcl`

  ![init_design.mcmm_example.auto_expanded.tcl](https://github.com/user-attachments/assets/e38879d8-5e5a-4e61-9b6b-819b347fc5df)

### Changes to `pns_example.tcl`

  ![pns_example.tcl](https://github.com/user-attachments/assets/73f518e4-a5cd-4d92-a7ac-6f766bdd126e)

### Modifications in `icc2_dp_setup.tcl`

  ![icc2_dp_setup.tcl - Graphic 1](https://github.com/user-attachments/assets/880c597b-1dba-4eea-b564-081c98380330)
  ![icc2_dp_setup.tcl - Graphic 2](https://github.com/user-attachments/assets/dc902ab1-b86e-4c18-8a47-eddb73c83f13)
  ![icc2_dp_setup.tcl - Graphic 3](https://github.com/user-attachments/assets/3556831d-add1-4101-be70-287f3a81b283)

---

# Executing every stage in top.tcl to undertand the flow

<img width="1508" alt="Pasted Graphic" src="https://github.com/user-attachments/assets/7f921565-d224-4f0c-af49-2973bd56682a">


<img width="1508" alt="image" src="https://github.com/user-attachments/assets/0d659339-b093-4583-931e-4f70393f18bd">

**Additoionally we can see changes in gui in every stage**

<img width="1508" alt="image" src="https://github.com/user-attachments/assets/7f2b942f-fc0c-4dcd-b847-7b07ab7b4d3d">


## Explanation of Commands

### Sourcing Setup Scripts

1. **Source `icc2_common_setup.tcl` with Echo**
   ```tcl
   source -echo /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/icc2_common_setup.tcl
   ```
   - This command sources the `icc2_common_setup.tcl` script with the `-echo` option, which prints each command executed by the script to the terminal. This is useful for debugging and verifying the commands being run.

2. **Source `icc2_dp_setup.tcl` with Echo**
   ```tcl
   source -echo /home/venkatesh/VSDBabySoC/icc2_workshop_collaterals/standaloneFlow/icc2_dp_setup.tcl
   ```
   - Similar to the previous command, this sources the `icc2_dp_setup.tcl` script with the `-echo` option for transparency and debugging purposes.

### Conditional Deletion of Design Library

3. **Check if the Design Library Exists and Delete It**
   ```tcl
   if {[file exists ${WORK_DIR}/$DESIGN_LIBRARY]} {
      file delete -force ${WORK_DIR}/${DESIGN_LIBRARY}
   }
   ```
   - This conditional block checks if the design library directory exists in the `WORK_DIR`.
   - If it exists, it forcefully deletes the directory using `file delete -force`. This ensures that any previous design library is removed before creating a new one.

### NDM Library Creation

4. **Create Library Command Setup**
   ```tcl
   set create_lib_cmd "create_lib ${WORK_DIR}/$DESIGN_LIBRARY"
   ```
   - This sets up the base command for creating the design library. The command `create_lib` is used to create a new library in the specified `WORK_DIR` with the name `DESIGN_LIBRARY`.

5. **Append Technology File or Technology Library**
   ```tcl
   if {[file exists [which $TECH_FILE]]} {
      lappend create_lib_cmd -tech $TECH_FILE ;# recommended
   } elseif {$TECH_LIB != ""} {
      lappend create_lib_cmd -use_technology_lib $TECH_LIB ;# optional
   }
   ```
   - This block conditionally appends options to the `create_lib_cmd` based on the existence of the technology file or technology library.
   - If the `TECH_FILE` exists, it appends `-tech $TECH_FILE` to use the technology file.
   - If the `TECH_LIB` variable is not empty, it appends `-use_technology_lib $TECH_LIB` to use the specified technology library.

6. **Append Reference Libraries**
   ```tcl
   lappend create_lib_cmd -ref_libs $REFERENCE_LIBRARY
   ```
   - This appends the `-ref_libs` option with the `REFERENCE_LIBRARY` to the `create_lib_cmd`. The reference library is used as a basis for creating the new library.

7. **Print the Create Library Command**
   ```tcl
   puts "RM-info : $create_lib_cmd"
   ```
   - This prints the complete `create_lib_cmd` to the terminal for informational and debugging purposes.

8. **Evaluate and Execute the Create Library Command**
   ```tcl
   eval ${create_lib_cmd}
   ```
   - This executes the `create_lib_cmd` using the `eval` command, which parses and runs the complete command string that has been constructed.

These commands collectively initialize and configure the design environment by sourcing necessary setup scripts, conditionally removing any existing design libraries, and creating a new library with specified technology and reference libraries.

---

<img width="1508" alt="image" src="https://github.com/user-attachments/assets/d215b260-6744-46a1-880c-47e799de4b23">


<img width="1508" alt="image" src="https://github.com/user-attachments/assets/6eb8cf0e-56f6-4e18-a931-12dfce6fda77">


<img width="1508" alt="image" src="https://github.com/user-attachments/assets/c02d5060-5884-40d8-a7fa-820b73afd0dd">


## Explanation of Commands

### Reading Synthesized Verilog

1. **Conditionally Read Verilog Outline or Full Chip Verilog**
   ```tcl
   if {$DP_FLOW == "hier" && $BOTTOM_BLOCK_VIEW == "abstract"} {
      # Read in the DESIGN_NAME outline.  This will create the outline
      puts "RM-info : Reading verilog outline (${VERILOG_NETLIST_FILES})"
      read_verilog_outline -design ${DESIGN_NAME}/${INIT_DP_LABEL_NAME} -top ${DESIGN_NAME} ${VERILOG_NETLIST_FILES}
   } else {
      # Read in the full DESIGN_NAME.  This will create the DESIGN_NAME view in the database
      puts "RM-info : Reading full chip verilog (${VERILOG_NETLIST_FILES})"
      read_verilog -design ${DESIGN_NAME}/${INIT_DP_LABEL_NAME} -top ${DESIGN_NAME} ${VERILOG_NETLIST_FILES}
   }
   ```
   - This block checks if the design planning flow (`DP_FLOW`) is hierarchical (`hier`) and if the bottom block view is abstract (`abstract`).
   - If true, it reads the Verilog outline using the `read_verilog_outline` command, creating the design outline.
   - Otherwise, it reads the full chip Verilog using the `read_verilog` command, creating the complete design view in the database.
   - The `${VERILOG_NETLIST_FILES}` contains the file paths to the synthesized Verilog files, and `${DESIGN_NAME}` and `${INIT_DP_LABEL_NAME}` specify the design and initialization labels.

### Technology Setup for Routing Layer Direction, Offset, Site Default, and Site Symmetry

2. **Set Up Technology Information**
   ```tcl
   if {$TECH_FILE != "" || ($TECH_LIB != "" && !$TECH_LIB_INCLUDES_TECH_SETUP_INFO)} {
      if {[file exists [which $TCL_TECH_SETUP_FILE]]} {
         puts "RM-info : Sourcing [which $TCL_TECH_SETUP_FILE]"
         source -echo $TCL_TECH_SETUP_FILE
      } elseif {$TCL_TECH_SETUP_FILE != ""} {
         puts "RM-error : TCL_TECH_SETUP_FILE($TCL_TECH_SETUP_FILE) is invalid. Please correct it."
      }
   }
   ```
   - This block sets up technology information if a technology file (`TECH_FILE`) is specified or if a technology library (`TECH_LIB`) is used but does not include the necessary setup information.
   - If the `TCL_TECH_SETUP_FILE` exists, it is sourced using the `source -echo` command, and the file's path is printed to the terminal.
   - If the `TCL_TECH_SETUP_FILE` does not exist or is invalid, an error message is printed.

### Specifying a Tcl Script to Read TLU+ Files

3. **Read Parasitic Technology Setup**
   ```tcl
   if {[file exists [which $TCL_PARASITIC_SETUP_FILE]]} {
      puts "RM-info : Sourcing [which $TCL_PARASITIC_SETUP_FILE]"
      source -echo $TCL_PARASITIC_SETUP_FILE
   } elseif ($TCL_PARASITIC_SETUP_FILE != "") {
      puts "RM-error : TCL_PARASITIC_SETUP_FILE($TCL_PARASITIC_SETUP_FILE) is invalid. Please correct it."
   } else {
      puts "RM-info : No TLU plus files sourced, Parasitic library containing TLU+ must be included in library reference list"
   }
   ```
   - This block specifies a Tcl script to read in TLU+ files, which are used for parasitic extraction.
   - If the `TCL_PARASITIC_SETUP_FILE` exists, it is sourced using the `source -echo` command, and the file's path is printed to the terminal.
   - If the `TCL_PARASITIC_SETUP_FILE` does not exist or is invalid, an error message is printed.
   - If no TLU+ files are sourced, a message is printed indicating that a parasitic library containing TLU+ must be included in the library reference list.

These commands collectively handle the reading of synthesized Verilog files, setting up technology information, and sourcing parasitic technology setup scripts, ensuring a complete and accurate design setup for further processing.

---

<img width="1508" alt="image" src="https://github.com/user-attachments/assets/15b0b9f3-1d0f-4241-9788-78fbbfb33f13">

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/9301922e-a9cb-4a43-8e64-d7b563a80247">


<img width="1512" alt="image" src="https://github.com/user-attachments/assets/6e7ae189-3472-4813-a4a5-5da4e28edbb4">


## Explanation of Commands

### Routing Settings

1. **Set Maximum Routing Layer**
   ```tcl
   if {$MAX_ROUTING_LAYER != ""} {
      set_ignored_layers -max_routing_layer $MAX_ROUTING_LAYER
   }
   ```
   - This command checks if the `MAX_ROUTING_LAYER` variable is set (not an empty string).
   - If it is set, it specifies the maximum routing layer to be used in the design by calling the `set_ignored_layers` command with the `-max_routing_layer` option.

2. **Set Minimum Routing Layer**
   ```tcl
   if {$MIN_ROUTING_LAYER != ""} {
      set_ignored_layers -min_routing_layer $MIN_ROUTING_LAYER
   }
   ```
   - This command checks if the `MIN_ROUTING_LAYER` variable is set (not an empty string).
   - If it is set, it specifies the minimum routing layer to be used in the design by calling the `set_ignored_layers` command with the `-min_routing_layer` option.

### Check Design: Pre-Floorplanning

3. **Check Design**
   ```tcl
   if {$CHECK_DESIGN} {
      redirect -file ${REPORTS_DIR_INIT_DP}/check_design.pre_floorplan {
         check_design -ems_database check_design.pre_floorplan.ems -checks dp_pre_floorplan
      }
   }
   ```
   - This command block checks if the `CHECK_DESIGN` variable is set to true.
   - If it is true, it runs the `check_design` command to perform a design check before floorplanning.
   - The `redirect` command is used to save the output of the `check_design` command to a file located at `${REPORTS_DIR_INIT_DP}/check_design.pre_floorplan`.
   - The `check_design` command is run with the `-ems_database` option to specify an EMS database file (`check_design.pre_floorplan.ems`) and the `-checks dp_pre_floorplan` option to specify the pre-floorplanning checks.

These commands ensure that the routing layer constraints are set and that the design is checked for issues before proceeding with the floorplanning stage. The checks are redirected to a specified file for review and documentation.

---

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/5b0d5333-dbd9-4f81-a79d-18dc4cde2a6c">


<img width="1512" alt="image" src="https://github.com/user-attachments/assets/8b8be95c-7ad8-48d0-aec3-dbc5c89def48">


<img width="1403" alt="Pasted Graphic" src="https://github.com/user-attachments/assets/7f0b0c14-e4f1-46ad-a9e3-ac231ff064c0">


## Explanation of Commands

### Floorplanning

1. **Initialize Floorplan with Core Utilization and Offset**
   ```tcl
   initialize_floorplan -core_utilization 0.3 -core_offset {5}
   ```
   - This command initializes the floorplan with a specified core utilization and offset.
   - `-core_utilization 0.3` sets the core area utilization to 30%.
   - `-core_offset {5}` sets the offset from the core boundary to 5 units on all sides.

2. **Create Keepout Margin**
   ```tcl
   create_keepout_margin -type hard -outer {2 2 2 2} [get_cells -physical_context -filter design_type==macro]
   ```
   - This command creates a hard keepout margin around specified cells.
   - `-type hard` specifies the keepout margin type as hard, meaning no cells can be placed within this margin.
   - `-outer {2 2 2 2}` sets the outer margins to 2 units on all sides.
   - `[get_cells -physical_context -filter design_type==macro]` selects cells that are macros to apply the keepout margin around them.

3. **Save Library**
   ```tcl
   save_lib -all
   ```
   - This command saves all the libraries currently in use.
   - `-all` specifies that all libraries should be saved.

### Summary

These commands are used to set up and initialize the floorplan for the design. The `initialize_floorplan` command sets the core utilization and offset, ensuring that the core area is properly defined. The `create_keepout_margin` command creates a hard keepout margin around macro cells to prevent other cells from being placed too close to them, ensuring proper spacing and avoiding placement issues. Finally, the `save_lib -all` command saves all libraries to preserve the current state of the design and its configurations.

----

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/0320325a-6580-4570-b0fa-70cc17ee7a6e">

<img width="1243" alt="image" src="https://github.com/user-attachments/assets/d08b6218-055f-4e99-805b-a9255af7d06c">

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/c939f257-a855-4f6e-bc86-8b15cd348b0d">



## Explanation of Commands

### Power and Ground (PG) Pin Connections

1. **Print Information Message**
   ```tcl
   puts "RM-info : Running connect_pg_net -automatic on all blocks"
   ```
   - This command prints an information message to the terminal, indicating that the `connect_pg_net -automatic` command is being run on all blocks.
   - `puts` is used to display messages in Tcl.

2. **Connect PG Nets Automatically**
   ```tcl
   connect_pg_net -automatic -all_blocks
   ```
   - This command connects power and ground (PG) nets automatically for all blocks in the design.
   - `-automatic` specifies that the connections should be made automatically based on predefined rules.
   - `-all_blocks` indicates that the command should apply to all blocks in the design.

3. **Save Block with Specific Label**
   ```tcl
   save_block -force -label ${PRE_SHAPING_LABEL_NAME}
   ```
   - This command saves the current state of the block with a specific label.
   - `-force` ensures that the block is saved even if there are warnings or if the block already exists.
   - `-label ${PRE_SHAPING_LABEL_NAME}` assigns the specified label to the saved block, allowing it to be easily identified later.

4. **Save All Libraries**
   ```tcl
   save_lib -all
   ```
   - This command saves all the libraries currently in use.
   - `-all` specifies that all libraries should be saved.

### Summary

These commands are used to connect the power and ground (PG) nets for all blocks in the design and to save the current state of the blocks and libraries. The `connect_pg_net -automatic -all_blocks` command ensures that all blocks have their PG nets connected automatically based on predefined rules. The `save_block -force -label ${PRE_SHAPING_LABEL_NAME}` command saves the current state of the block with a specific label, allowing it to be easily referenced later. Finally, the `save_lib -all` command saves all libraries to preserve the current state of the design and its configurations.

---

## Explanation of Verilog Code of SKY130_FD_SC_HD__CONB_1_FUNCTIONAL_V


<img width="1419" alt="image" src="https://github.com/user-attachments/assets/bb390e04-2771-4116-bf51-85be85b77e90">



<img width="861" alt="image" src="https://github.com/user-attachments/assets/df03ca09-2198-421b-9e8e-00b87a96b2b7">




### Header and Guard Clauses

1. **Header and Guard Clauses**
   ```verilog
   `ifndef SKY130_FD_SC_HD__CONB_1_FUNCTIONAL_V
   `define SKY130_FD_SC_HD__CONB_1_FUNCTIONAL_V
   ```
   - The `ifndef` directive checks if `SKY130_FD_SC_HD__CONB_1_FUNCTIONAL_V` is not defined.
   - If it is not defined, the `define` directive defines `SKY130_FD_SC_HD__CONB_1_FUNCTIONAL_V`.
   - This is used to prevent multiple inclusions of the same file, known as an include guard.

### Documentation and Timescale

2. **Documentation and Timescale**
   ```verilog
   /**
    * conb: Constant value, low, high outputs.
    *
    * Verilog simulation functional model.
    */

   `timescale 1ns / 1ps
   `default_nettype none
   ```
   - The comment block describes the module `conb` as having constant value low and high outputs.
   - `timescale 1ns / 1ps` specifies the time unit and precision for the simulation (1 nanosecond unit and 1 picosecond precision).
   - `default_nettype none` enforces that all nets must be explicitly declared, helping to avoid implicit net declarations which can lead to errors.

### Module Definition

3. **Module Definition**
   ```verilog
   `celldefine
   module sky130_fd_sc_hd__conb_1 (
       HI,
       LO
   );
   ```
   - `celldefine` marks the beginning of a cell definition, useful for CAD tools.
   - The module `sky130_fd_sc_hd__conb_1` is defined with two output ports: `HI` and `LO`.

### Module Ports and Internal Logic

4. **Module Ports and Internal Logic**
   ```verilog
       // Module ports
       output HI;
       output LO;

       //       Name       Output
       pullup   pullup0   (HI    );
       pulldown pulldown0 (LO    );
   ```
   - `output HI;` and `output LO;` declare `HI` and `LO` as output ports.
   - `pullup pullup0 (HI);` connects a pullup resistor to the `HI` output, ensuring it is constantly driven high.
   - `pulldown pulldown0 (LO);` connects a pulldown resistor to the `LO` output, ensuring it is constantly driven low.

### End of Module and Guard Clause

5. **End of Module and Guard Clause**
   ```verilog
   endmodule
   `endcelldefine

   `default_nettype wire
   ```
   - `endmodule` marks the end of the module definition.
   - `endcelldefine` marks the end of the cell definition.
   - `default_nettype wire` resets the default net type to `wire`.

### Include Guard End

6. **Include Guard End**
   ```verilog
   `endif // SKY130_FD_SC_HD__CONB_1_FUNCTIONAL_V
   ```
   - `endif` marks the end of the include guard, ensuring that the code between `ifndef` and `endif` is only included once.

### Summary

This Verilog code defines a module `sky130_fd_sc_hd__conb_1` with two outputs: `HI` and `LO`. The `HI` output is connected to a pullup resistor, making it constantly high, while the `LO` output is connected to a pulldown resistor, making it constantly low. The code includes guard clauses to prevent multiple inclusions and uses the `celldefine` directive for CAD tool compatibility. The timescale and default nettype are specified to ensure precise simulation behavior and explicit net declarations.


Making an output always low or high can be useful in various scenarios within digital design and circuit applications. Here are some reasons why you might want to have an output always low:

### Reasons for an Output Always Low

1. **Default State or Initialization:**
   - In some designs, certain signals need to be initialized to a known state. Setting an output to always low ensures that the connected components start in a known state, preventing unpredictable behavior at power-up or reset.

2. **Control Signals:**
   - Constant low signals can be used as control signals to disable or enable certain parts of a circuit. For example, a constant low might be used to keep a specific part of the circuit inactive until another condition is met.

3. **Logic Level Reference:**
   - Providing a constant low (or ground) signal can be useful as a reference for other components or circuits that need a steady low voltage reference.

4. **Testing and Debugging:**
   - During the testing phase, it might be necessary to force certain signals to a known state to isolate and debug parts of a circuit. A constant low output can be used to test how the circuit behaves under specific conditions.

5. **Unused Pins:**
   - Unused inputs on certain chips might need to be tied to a logic low to prevent them from floating, which can cause unpredictable behavior due to noise.

6. **Power Management:**
   - In power management scenarios, a constant low signal can be used to disable parts of a circuit, reducing power consumption when those parts are not needed.

### Example Use Case

Let's consider a scenario where you might need an output to always be low:

- **Reset Signal:** In many digital circuits, there is a reset signal that needs to be held low to reset the state of various flip-flops and other sequential elements. By providing a constant low signal, you can ensure that all parts of the circuit are properly reset before starting normal operation.


### Summary

Having an output always set to low is useful for initializing states, providing control signals, referencing logic levels, and various other scenarios in digital design. It ensures predictable and stable behavior in circuits where a known low state is required.

Choosing whether to keep an output always low or high depends on the specific requirements and context of your design. Both have their uses and advantages. Here's a comparison to help determine which might be best in different scenarios:

### Keeping Output Always Low

#### Advantages:
1. **Default State Initialization:**
   - Ensures certain components or signals start in a known low state, which might be required for proper initialization.

2. **Disabling Components:**
   - Can be used to disable certain parts of the circuit, ensuring they remain inactive until needed.

3. **Power Management:**
   - Often, keeping certain signals low can reduce power consumption, as some components consume less power in a low state.

4. **Ground Reference:**
   - Provides a stable ground reference for other parts of the circuit.

#### Use Cases:
- Reset signals that need to initialize the state of components.
- Control signals to keep parts of the circuit inactive.
- Unused inputs that need to be tied low to prevent floating.

### Keeping Output Always High

#### Advantages:
1. **Enabling Components:**
   - Can be used to enable certain parts of the circuit, ensuring they remain active or ready to operate.

2. **Default State Initialization:**
   - Ensures certain components or signals start in a known high state, which might be required for proper initialization.

3. **Voltage Reference:**
   - Provides a stable high voltage reference for other parts of the circuit.

#### Use Cases:
- Signals that need to be high to enable certain functionalities.
- Control signals to keep parts of the circuit active.
- Initialization signals that need to start in a high state.

### Comparison and Decision Criteria

1. **Component Requirements:**
   - Some components may require a high or low signal to be in a specific state (enabled, disabled, initialized).

2. **Power Consumption:**
   - Consider the power implications. Some designs might be more power-efficient with signals set to low, while others might not have a significant difference.

3. **Design Logic:**
   - Based on the logic design, determine whether the circuit operates correctly and efficiently with a constant high or low signal.

4. **Safety and Stability:**
   - Evaluate which state (high or low) ensures greater stability and safety for the overall circuit operation.

### Example Scenarios

1. **Reset Signal:**
   - Typically, reset signals are active low, meaning a low signal resets the circuit. In this case, having an output always low could be useful during initialization but would need to be controllable.

2. **Enable Signal:**
   - Many enable signals are active high, meaning a high signal enables the circuit. Here, having an output always high might be beneficial for default enable states.

### Conclusion

Neither always low nor always high is inherently better; the choice depends on the specific needs of your circuit and design. Here are some guiding points:
- **Use always low** if you need to initialize or disable components, provide a ground reference, or manage power efficiently.
- **Use always high** if you need to enable components, provide a voltage reference, or ensure certain signals start in an active state.

Ultimately, evaluate your specific design requirements and choose the state that best aligns with your circuit's functionality and performance goals.

---
<img width="1469" alt="image" src="https://github.com/user-attachments/assets/4df029a5-8724-4e32-b066-acd79e1a7c12">


<img width="1469" alt="image" src="https://github.com/user-attachments/assets/abf26f1e-8ddf-4063-b90c-e58cb6f694d3">



<img width="1469" alt="image" src="https://github.com/user-attachments/assets/9415a1dd-1fbf-4abe-94ea-6531e09d04e2">


## Explanation of Commands

### Memory Placement

1. **Check for Hard Macros and Report Constraints**
   ```tcl
   if [sizeof_collection [get_cells -hier -filter is_hard_macro==true -quiet]] {
      set all_macros [get_cells -hier -filter is_hard_macro==true]
      # Check top-level
      report_macro_constraints -allowed_orientations -preferred_location -alignment_grid -align_pins_to_tracks $all_macros > $REPORTS_DIR_PLACEMENT/report_macro_constraints.rpt
   }
   ```
   - This block checks if there are any hard macros in the design.
   - `get_cells -hier -filter is_hard_macro==true -quiet` retrieves all hard macros in the hierarchical design. The `sizeof_collection` function returns the number of hard macros found.
   - If any hard macros are found, the list of all hard macros is stored in the `all_macros` variable.
   - The `report_macro_constraints` command generates a report for macro constraints, including allowed orientations, preferred locations, alignment grid, and pin alignment to tracks. The report is saved to a file specified by `$REPORTS_DIR_PLACEMENT/report_macro_constraints.rpt`.

2. **Place Macros at User-Defined Locations**
   ```tcl
   if {$USE_INCREMENTAL_DATA && [file exists $OUTPUTS_DIR/preferred_macro_locations.tcl]} {
      source $OUTPUTS_DIR/preferred_macro_locations.tcl
   }
   ```
   - This block checks if the `USE_INCREMENTAL_DATA` variable is true and if the file `preferred_macro_locations.tcl` exists in the `$OUTPUTS_DIR` directory.
   - If both conditions are met, the `preferred_macro_locations.tcl` file is sourced, applying user-defined macro placement locations.

### Summary

These commands handle the placement of memory and hard macros in the design. The first block checks for the presence of hard macros and generates a report detailing their constraints and preferred locations. The second block applies user-defined macro placements if incremental data is being used and the corresponding Tcl file exists. This approach ensures that macros are placed according to design constraints and user preferences, facilitating efficient and optimal placement in the floorplan.

---


<img width="1473" alt="image" src="https://github.com/user-attachments/assets/29c508d7-fd69-4b7f-8a5d-cbc5e001023c">


## Explanation of Commands

### Configure Placement

1. **Set HOST_OPTIONS Based on DISTRIBUTED Flag**
   ```tcl
   if {$DISTRIBUTED} {
      set HOST_OPTIONS "-host_options block_script"
   } else {
      set HOST_OPTIONS ""
   }
   ```
   - This block checks if the `DISTRIBUTED` variable is set to true.
   - If `DISTRIBUTED` is true, it sets the `HOST_OPTIONS` variable to `-host_options block_script`.
   - If `DISTRIBUTED` is false, it sets `HOST_OPTIONS` to an empty string.

2. **Set CMD_OPTIONS with HOST_OPTIONS**
   ```tcl
   set CMD_OPTIONS "-floorplan $HOST_OPTIONS"
   ```
   - This command sets the `CMD_OPTIONS` variable.
   - It combines the `-floorplan` option with the value of `HOST_OPTIONS`.
   - If `HOST_OPTIONS` is set (e.g., `-host_options block_script`), `CMD_OPTIONS` will include this setting.
   - If `HOST_OPTIONS` is empty, `CMD_OPTIONS` will only include `-floorplan`.

### Summary

These commands configure the placement settings for the design based on whether the `DISTRIBUTED` flag is set. If the design is distributed, it adds specific host options to the command options. Otherwise, it proceeds with default settings. The `CMD_OPTIONS` variable is then constructed to include the necessary options for floorplanning, ensuring that the appropriate configurations are applied based on the `DISTRIBUTED` flag.

---

<img width="1118" alt="image" src="https://github.com/user-attachments/assets/0c1b3c58-19d2-40ae-bd50-dc8f59e0f1d4">


<img width="1493" alt="image" src="https://github.com/user-attachments/assets/3bd85bb1-3391-4f9d-abd3-d88ba8e3326f">


## Explanation of Commands

### Read Parasitic Files

1. **Check and Source Parasitic Setup File**
   ```tcl
   if {[file exists [which $TCL_PARASITIC_SETUP_FILE]]} {
      puts "RM-info : Sourcing [which $TCL_PARASITIC_SETUP_FILE]"
      source -echo $TCL_PARASITIC_SETUP_FILE
   } elseif {$TCL_PARASITIC_SETUP_FILE != ""} {
      puts "RM-error : TCL_PARASITIC_SETUP_FILE($TCL_PARASITIC_SETUP_FILE) is invalid. Please correct it."
   } else {
      puts "RM-info : No TLU plus files sourced, Parasitic library containing TLU+ must be included in library reference list"
   }
   ```
   - This block handles the sourcing of the parasitic setup file for the design.
   
2. **File Existence Check and Sourcing**
   ```tcl
   if {[file exists [which $TCL_PARASITIC_SETUP_FILE]]} {
      puts "RM-info : Sourcing [which $TCL_PARASITIC_SETUP_FILE]"
      source -echo $TCL_PARASITIC_SETUP_FILE
   }
   ```
   - This command checks if the `TCL_PARASITIC_SETUP_FILE` exists using the `file exists` and `which` commands.
   - If the file exists, it prints an informational message indicating that the file is being sourced.
   - The `source -echo` command sources the `TCL_PARASITIC_SETUP_FILE`, printing each command executed by the script for debugging purposes.

3. **Invalid File Check**
   ```tcl
   elseif {$TCL_PARASITIC_SETUP_FILE != ""} {
      puts "RM-error : TCL_PARASITIC_SETUP_FILE($TCL_PARASITIC_SETUP_FILE) is invalid. Please correct it."
   }
   ```
   - If the `TCL_PARASITIC_SETUP_FILE` variable is not empty but the file does not exist, it prints an error message indicating that the file is invalid and needs correction.

4. **No File Specified**
   ```tcl
   else {
      puts "RM-info : No TLU plus files sourced, Parasitic library containing TLU+ must be included in library reference list"
   }
   ```
   - If the `TCL_PARASITIC_SETUP_FILE` variable is empty, it prints an informational message indicating that no TLU+ files were sourced and that a parasitic library containing TLU+ must be included in the library reference list.

### Summary

These commands manage the process of reading parasitic setup files for the design. They check if the specified parasitic setup file exists and, if so, source it with detailed output for debugging. If the file is specified but does not exist, an error message is printed. If no file is specified, an informational message is printed, indicating the need for a parasitic library containing TLU+ in the library reference list. This ensures that the design has the necessary parasitic information for accurate timing and signal integrity analysis.

---

<img width="1302" alt="image" src="https://github.com/user-attachments/assets/02fe8b49-4c78-4943-ba3b-e3c3bdac538f">


<img width="949" alt="image" src="https://github.com/user-attachments/assets/61c5ca96-a885-4a16-bf3e-d70b36ed1bab">


<img width="796" alt="image" src="https://github.com/user-attachments/assets/8061a509-876b-4774-9011-4ae4f38b85df">


<img width="1129" alt="Pasted Graphic 1" src="https://github.com/user-attachments/assets/b784de9c-274e-427d-bccc-e57e56c019c3">


<img width="1439" alt="Pasted Graphic 2" src="https://github.com/user-attachments/assets/5e4d0805-255e-432d-917d-190dcc049c8f">



<img width="1439" alt="Pasted Graphic 3" src="https://github.com/user-attachments/assets/20522921-374c-4c5e-9c02-d37ab752402e">


<img width="1439" alt="Pasted Graphic 4" src="https://github.com/user-attachments/assets/5d306384-cca2-4b9d-adb5-8bb06e67c430">




## Explanation of Commands

### Read Constraints

1. **Check and Source MCMM Setup File**
   ```tcl
   if {[file exists $TCL_MCMM_SETUP_FILE]} {
      puts "RM-info : Loading TCL_MCMM_SETUP_FILE ($TCL_MCMM_SETUP_FILE)"
      source -echo $TCL_MCMM_SETUP_FILE
   } else {
      puts "RM-error : Cannot find TCL_MCMM_SETUP_FILE ($TCL_MCMM_SETUP_FILE)"
      error
   }
   ```
   - This block handles the loading of the Multi-Corner Multi-Mode (MCMM) setup file for the design.
   - It checks if the `TCL_MCMM_SETUP_FILE` exists using the `file exists` command.
   - If the file exists, it prints an informational message and sources the file with `source -echo`.
   - If the file does not exist, it prints an error message and raises an error to stop script execution.

### Placement Configuration

2. **Set Placement Options**
   ```tcl
   set plan.place.auto_generate_blockages true
   eval create_placement -effort high -timing_driven -congestion -floorplan
   legalize_placement
   ```
   - `set plan.place.auto_generate_blockages true`: Enables automatic generation of blockages during placement.
   - `eval create_placement -effort high -timing_driven -congestion -floorplan`: Creates the placement with high effort, considering timing, congestion, and the existing floorplan.
   - `legalize_placement`: Adjusts the placement to ensure legality, resolving any overlaps or other issues.

3. **Report Placement**
   ```tcl
   report_placement -physical_hierarchy_violations all -wirelength all -hard_macro_overlap -verbose high > $REPORTS_DIR_PLACEMENT/report_placement.rpt
   ```
   - Generates a detailed placement report, including physical hierarchy violations, wirelength, and hard macro overlap.
   - The report is saved to `$REPORTS_DIR_PLACEMENT/report_placement.rpt`.

### Macro Placement

4. **Write Macro Preferred Locations**
   ```tcl
   if [sizeof_collection [get_cells -hier -filter is_hard_macro==true -quiet]] {
      file delete -force $OUTPUTS_DIR/preferred_macro_locations.tcl
      set all_macros [get_cells -hier -filter is_hard_macro==true]
      derive_preferred_macro_locations $all_macros -file $OUTPUTS_DIR/preferred_macro_locations.tcl
   }
   ```
   - Checks if there are any hard macros in the design.
   - If hard macros are found, it deletes any existing `preferred_macro_locations.tcl` file in the `$OUTPUTS_DIR` directory.
   - The list of hard macros is stored in the `all_macros` variable.
   - The `derive_preferred_macro_locations` command generates preferred locations for the hard macros and saves them to `preferred_macro_locations.tcl`.

### Summary

These commands manage the process of reading MCMM constraints, configuring placement, and handling macro placement in the design. They ensure that the necessary constraints for MCMM analysis are loaded, create and legalize the placement with high effort considering timing and congestion, and generate a detailed placement report. Additionally, they manage the preferred locations for hard macros, ensuring optimal placement in subsequent runs. This comprehensive approach ensures accurate and efficient placement in the design flow.

---


<img width="776" alt="image" src="https://github.com/user-attachments/assets/6f2a65a7-359e-4abc-a4a6-8af1666bcaa4">


<img width="975" alt="image" src="https://github.com/user-attachments/assets/78b0da23-bd7e-4516-9368-46ddaf2d9da1">



## Explanation of Commands

### Fix All Shaped Blocks and Macros

1. **Check for Hard Macros**
   ```tcl
   if [sizeof_collection [get_cells -hier -filter is_hard_macro==true -quiet]] {
      set_attribute -quiet [get_cells -hier -filter is_hard_macro==true] status fixed
   }
   ```
   - This block checks if there are any hard macros in the design.
   - `get_cells -hier -filter is_hard_macro==true -quiet` retrieves all hard macros in the hierarchical design.
   - If hard macros are found, the `set_attribute -quiet [get_cells -hier -filter is_hard_macro==true] status fixed` command sets the `status` attribute of all hard macros to `fixed`.
   - Setting the `status` to `fixed` ensures that the placement of these hard macros is locked and they will not be moved in subsequent design stages.

2. **Save Block with Specific Label**
   ```tcl
   save_block -hier -force -label ${PLACEMENT_LABEL_NAME}
   ```
   - This command saves the current state of the hierarchical block with a specific label.
   - `-force` ensures that the block is saved even if there are warnings or if the block already exists.
   - `-label ${PLACEMENT_LABEL_NAME}` assigns the specified label to the saved block, allowing it to be easily identified later.

3. **Save All Libraries**
   ```tcl
   save_lib -all
   ```
   - This command saves all the libraries currently in use.
   - `-all` specifies that all libraries should be saved.

### Summary

These commands manage the process of fixing the placement of all hard macros and saving the current state of the design. The first block checks for the presence of hard macros and sets their status to `fixed` to lock their placement. The second command saves the hierarchical block with a specific label, ensuring that the current placement is preserved. Finally, the third command saves all libraries to maintain the current state of the design and its configurations. This approach ensures that the placement of critical components is maintained and can be reliably referenced in future design stages.


---

<img width="975" alt="image" src="https://github.com/user-attachments/assets/cc3188c2-1b63-45a0-8cc5-30f6f42502de">


<img width="975" alt="image" src="https://github.com/user-attachments/assets/67d7e752-7bcc-415f-835a-1a9f752f9fc6">


<img width="1438" alt="image" src="https://github.com/user-attachments/assets/d1578f53-1112-4b8e-ac60-c4b3bbb6200c">



<img width="616" alt="image" src="https://github.com/user-attachments/assets/18eb5d09-ae9f-414b-b99a-dc7ce8ef8efd">


## Explanation of Commands

### Create Power

1. **Source PNS File**
   ```tcl
   if {[file exists $TCL_PNS_FILE]} {
      puts "RM-info : Sourcing TCL_PNS_FILE ($TCL_PNS_FILE)"
      source -echo $TCL_PNS_FILE
   }
   ```
   - This block checks if the `TCL_PNS_FILE` exists.
   - If it exists, an informational message is printed, and the file is sourced using `source -echo`.

2. **PNS Characterization Flow**
   ```tcl
   if {$PNS_CHARACTERIZE_FLOW == "true" && $TCL_COMPILE_PG_FILE != ""} {
      puts "RM-info : RUNNING PNS CHARACTERIZATION FLOW because \$PNS_CHARACTERIZE_FLOW == true"
      characterize_block_pg -output block_pg -compile_pg_script $TCL_COMPILE_PG_FILE
      set_constraint_mapping_file ./block_pg/pg_mapfile
      if {$DISTRIBUTED} { 
         set HOST_OPTIONS "-host_options block_script"
      } else {
         set HOST_OPTIONS ""
      }
      puts "RM-info : Running run_block_compile_pg $HOST_OPTIONS"
      eval run_block_compile_pg ${HOST_OPTIONS}
   }
   ```
   - If the `PNS_CHARACTERIZE_FLOW` variable is set to "true" and `TCL_COMPILE_PG_FILE` is not empty, the PNS characterization flow is executed.
   - An informational message is printed, and `characterize_block_pg` is run to output to `block_pg` and compile the PG script specified by `TCL_COMPILE_PG_FILE`.
   - The constraint mapping file is set to `./block_pg/pg_mapfile`.
   - The `HOST_OPTIONS` variable is set based on whether the `DISTRIBUTED` variable is true or false.
   - An informational message is printed, and `run_block_compile_pg` is executed with the specified `HOST_OPTIONS`.

3. **Source Compile PG File**
   ```tcl
   } else {
      if {$TCL_COMPILE_PG_FILE != ""} {
         source $TCL_COMPILE_PG_FILE
      } else {
         puts "RM-info : No Power Networks Implemented as TCL_COMPILE_PG_FILE does not exist"
      }
   }
   ```
   - If the PNS characterization flow is not executed and `TCL_COMPILE_PG_FILE` is not empty, the `TCL_COMPILE_PG_FILE` is sourced.
   - If `TCL_COMPILE_PG_FILE` is empty, an informational message is printed, indicating that no power networks are implemented.

4. **Check PG Connectivity and DRC**
   ```tcl
   check_pg_connectivity -check_std_cell_pins none
   check_pg_drc -ignore_std_cells
   redirect -file $REPORTS_DIR_CREATE_POWER/check_mv_design.erc_mode {check_mv_design -erc_mode}
   redirect -file $REPORTS_DIR_CREATE_POWER/check_mv_design.power_connectivity {check_mv_design -power_connectivity}
   ```
   - `check_pg_connectivity -check_std_cell_pins none`: Checks PG connectivity, ignoring standard cell pins.
   - `check_pg_drc -ignore_std_cells`: Checks PG DRC, ignoring standard cells.
   - `redirect -file $REPORTS_DIR_CREATE_POWER/check_mv_design.erc_mode {check_mv_design -erc_mode}`: Redirects the output of `check_mv_design -erc_mode` to the specified report file.
   - `redirect -file $REPORTS_DIR_CREATE_POWER/check_mv_design.power_connectivity {check_mv_design -power_connectivity}`: Redirects the output of `check_mv_design -power_connectivity` to the specified report file.

5. **Save Block and Libraries**
   ```tcl
   save_block -hier -force -label ${CREATE_POWER_LABEL_NAME}
   save_lib -all
   ```
   - `save_block -hier -force -label ${CREATE_POWER_LABEL_NAME}`: Saves the current state of the hierarchical block with a specific label.
   - `save_lib -all`: Saves all the libraries currently in use.

### Summary

These commands handle the process of creating power networks in the design. They source the necessary PNS file, run the PNS characterization flow if required, and source the compile PG file if it exists. They also check PG connectivity and DRC, generating reports for these checks. Finally, the current state of the hierarchical block and all libraries are saved, ensuring that the power network setup is preserved and can be referenced in future design stages.

---



<img width="1478" alt="image" src="https://github.com/user-attachments/assets/8dda1222-9bf8-42fe-8f6f-1ba9440f1c54">



<img width="634" alt="image" src="https://github.com/user-attachments/assets/1aadb405-435b-41be-9122-fb395ce05be6">



<img width="1001" alt="image" src="https://github.com/user-attachments/assets/873890d0-8f54-41bb-834b-2aa6ca4465c7">




<img width="1216" alt="image" src="https://github.com/user-attachments/assets/8de26d4b-b689-474f-a7cb-f09202248031">



## Explanation of Commands

### Pin Placement

1. **Source Pin Constraint File**
   ```tcl
   if {[file exists [which $TCL_PIN_CONSTRAINT_FILE]] && !$PLACEMENT_PIN_CONSTRAINT_AWARE} {
      source -echo $TCL_PIN_CONSTRAINT_FILE
   }
   ```
   - This block checks if the `TCL_PIN_CONSTRAINT_FILE` exists and if the `PLACEMENT_PIN_CONSTRAINT_AWARE` variable is not set.
   - If both conditions are met, the `TCL_PIN_CONSTRAINT_FILE` is sourced using `source -echo`.

2. **Set Application Options**
   ```tcl
   set_app_options -as_user_default -list {route.global.timing_driven true}
   ```
   - This command sets application options to default to timing-driven global routing.

3. **Pre-Pin Placement Design Check**
   ```tcl
   if {$CHECK_DESIGN} {
      redirect -file ${REPORTS_DIR_PLACE_PINS}/check_design.pre_pin_placement {
         check_design -ems_database check_design.pre_pin_placement.ems -checks dp_pre_pin_placement
      }
   }
   ```
   - If `CHECK_DESIGN` is true, this block runs a design check before pin placement.
   - The results are redirected to `${REPORTS_DIR_PLACE_PINS}/check_design.pre_pin_placement`.

4. **Place Pins**
   ```tcl
   if {$PLACE_PINS_SELF} {
      place_pins -self
   }
   ```
   - If `PLACE_PINS_SELF` is true, this command places the pins using the `-self` option.

5. **Write Pin Constraints and Verify Placement**
   ```tcl
   if {$PLACE_PINS_SELF} {
      # Write top-level port constraint file based on actual port locations in the design for reuse during incremental run.
      write_pin_constraints -self -file_name $OUTPUTS_DIR/preferred_port_locations.tcl -physical_pin_constraint {side | offset | layer} -from_existing_pins

      # Verify Top-level Port Placement Results
      check_pin_placement -self -pre_route true -pin_spacing true -sides true -layers true -stacking true

      # Generate Top-level Port Placement Report
      report_pin_placement -self > $REPORTS_DIR_PLACE_PINS/report_port_placement.rpt
   }
   ```
   - If `PLACE_PINS_SELF` is true, this block writes the pin constraints based on the actual port locations to `preferred_port_locations.tcl`.
   - It verifies the top-level port placement results, checking pre-route pin spacing, sides, layers, and stacking.
   - It generates a report for the pin placement results, saved to `${REPORTS_DIR_PLACE_PINS}/report_port_placement.rpt`.

6. **Save Block and Libraries**
   ```tcl
   save_block -hier -force -label ${PLACE_PINS_LABEL_NAME}
   save_lib -all
   ```
   - `save_block -hier -force -label ${PLACE_PINS_LABEL_NAME}`: Saves the current state of the hierarchical block with a specific label.
   - `save_lib -all`: Saves all the libraries currently in use.

### Summary

These commands manage the pin placement process in the design. They source the pin constraint file if it exists, set application options, and run a pre-pin placement design check if required. They place the pins, write the pin constraints for reuse, verify the pin placement, and generate a placement report. Finally, they save the current state of the hierarchical block and all libraries, ensuring that the pin placement configuration is preserved for future design stages.


---


<img width="1219" alt="image" src="https://github.com/user-attachments/assets/2ba05818-a6a7-4d01-b076-ae29698ba4ab">


<img width="1051" alt="image" src="https://github.com/user-attachments/assets/a9d9cdd2-13ab-4de1-8272-d77ee830195d">



<img width="943" alt="image" src="https://github.com/user-attachments/assets/fa1cd299-9473-4d29-bd01-8bd89089762d">



<img width="943" alt="image" src="https://github.com/user-attachments/assets/aa227a78-e62f-4eac-9763-99a9933b77d0">


**Constraints used** 

<img width="943" alt="image" src="https://github.com/user-attachments/assets/3fa8d97e-76f2-4523-b003-b6794f2b6b7d">


**Report_timing**

<img width="943" alt="image" src="https://github.com/user-attachments/assets/c50617ef-75b5-45af-9766-127ed3a9ee84">

**report_constraints -all_violators**


<img width="984" alt="image" src="https://github.com/user-attachments/assets/107584d2-3f00-4577-bb60-93edddd73523">


<img width="928" alt="image" src="https://github.com/user-attachments/assets/646f2409-237a-4267-aca5-c2a231a9161f">


## Explanation of Commands

### Timing Estimation and Reporting

1. **Estimate Timing**
   ```tcl
   estimate_timing
   ```
   - This command performs a timing estimation for the design.

2. **Redirect Timing Report**
   ```tcl
   redirect -file $REPORTS_DIR_TIMING_ESTIMATION/${DESIGN_NAME}.post_estimated_timing.rpt {
      report_timing -corner estimated_corner -mode [all_modes]
   }
   ```
   - This block redirects the output of the `report_timing` command to a file.
   - `report_timing -corner estimated_corner -mode [all_modes]` generates a timing report for the estimated corner and all modes.
   - The report is saved to `$REPORTS_DIR_TIMING_ESTIMATION/${DESIGN_NAME}.post_estimated_timing.rpt`.

3. **Redirect Quality of Results (QoR) Report**
   ```tcl
   redirect -file $REPORTS_DIR_TIMING_ESTIMATION/${DESIGN_NAME}.post_estimated_timing.qor {
      report_qor -corner estimated_corner
   }
   ```
   - This block redirects the output of the `report_qor` command to a file.
   - `report_qor -corner estimated_corner` generates a QoR report for the estimated corner.
   - The report is saved to `$REPORTS_DIR_TIMING_ESTIMATION/${DESIGN_NAME}.post_estimated_timing.qor`.

4. **Redirect QoR Summary Report**
   ```tcl
   redirect -file $REPORTS_DIR_TIMING_ESTIMATION/${DESIGN_NAME}.post_estimated_timing.qor.sum {
      report_qor -summary
   }
   ```
   - This block redirects the output of the `report_qor -summary` command to a file.
   - `report_qor -summary` generates a summary QoR report.
   - The report is saved to `$REPORTS_DIR_TIMING_ESTIMATION/${DESIGN_NAME}.post_estimated_timing.qor.sum`.

### Save Block and Libraries

5. **Save Block with Specific Label**
   ```tcl
   save_block -hier -force -label ${TIMING_ESTIMATION_LABEL_NAME}
   ```
   - This command saves the current state of the hierarchical block with a specific label.
   - `-force` ensures that the block is saved even if there are warnings or if the block already exists.
   - `-label ${TIMING_ESTIMATION_LABEL_NAME}` assigns the specified label to the saved block, allowing it to be easily identified later.

6. **Save All Libraries**
   ```tcl
   save_lib -all
   ```
   - This command saves all the libraries currently in use.
   - `-all` specifies that all libraries should be saved.

### Summary

These commands manage the timing estimation process and generate detailed reports on the design's timing and quality of results (QoR). They estimate the timing, generate and redirect timing and QoR reports to specified files, and save the current state of the hierarchical block and all libraries. This ensures that the timing estimation results and the design configuration are preserved for future reference and analysis.

---

<img width="947" alt="image" src="https://github.com/user-attachments/assets/61e945dd-e7e0-4cbb-b7f7-b0bba0776457">

---


<img width="1512" alt="image" src="https://github.com/user-attachments/assets/6f351092-1d28-485d-8d45-88deea9ce26f">

---

<img width="649" alt="image" src="https://github.com/user-attachments/assets/66a56d28-a793-46b3-a045-efac14e604bc">

---

<img width="1433" alt="image" src="https://github.com/user-attachments/assets/cabfeccd-525c-43ff-a455-7251cea2fdbf">

---

<img width="1433" alt="Pasted Graphic 5" src="https://github.com/user-attachments/assets/c2012bc8-6395-45d6-af71-80fb194647a8">

---

<img width="1433" alt="image" src="https://github.com/user-attachments/assets/d26ebea7-3725-4e4e-92ec-864153fb0042">


## Explanation of Commands

### Place, Clock Tree Synthesis (CTS), and Route

1. **Set Host Options**
   ```tcl
   set_host_options -max_cores 8
   ```
   - This command sets the maximum number of cores to be used by the host to 8. This helps in parallelizing the process to utilize available CPU resources efficiently.

2. **Remove Estimated Corner**
   ```tcl
   remove_corners [get_corners estimated_corner]
   ```
   - This command removes the estimated corner from the design's corners. This is usually done to clean up any temporary or placeholder timing corners used during initial estimations.

3. **Set Application Options**
   ```tcl
   set_app_options -name place.coarse.continue_on_missing_scandef -value true
   ```
   - This command sets an application option to allow the placement process to continue even if a scan definition file is missing. This can be useful in scenarios where scan chains are not fully defined during initial placement.

4. **Perform Placement Optimization**
   ```tcl
   place_opt
   ```
   - This command performs placement optimization on the design. It adjusts the positions of standard cells to improve the design's performance and area utilization.

5. **Perform Clock Tree Synthesis (CTS) Optimization**
   ```tcl
   clock_opt
   ```
   - This command performs clock tree synthesis optimization. It ensures that the clock distribution network is optimized for minimal skew and optimal timing performance.

6. **Perform Routing**
   ```tcl
   route_auto -max_detail_route_iterations 5
   ```
   - This command performs automatic routing with a maximum of 5 iterations for detailed routing. Routing connects the physical wires between cells, adhering to design rules and optimizing for performance and manufacturability.

7. **Create Standard Cell Fillers**
   ```tcl
   set FILLER_CELLS [get_object_name [sort_collection -descending [get_lib_cells sky130_fd_sc_hd__fill*] area]]
   create_stdcell_fillers -lib_cells $FILLER_CELLS
   ```
   - `set FILLER_CELLS [get_object_name [sort_collection -descending [get_lib_cells sky130_fd_sc_hd__fill*] area]]`: This command identifies filler cells from the standard cell library, sorted in descending order by area.
   - `create_stdcell_fillers -lib_cells $FILLER_CELLS`: This command creates standard cell fillers using the identified filler cells. Filler cells are used to fill gaps between standard cells to ensure manufacturability and to maintain the integrity of the power and ground networks.

### Save Block and Libraries

8. **Save Block with Specific Label**
   ```tcl
   save_block -hier -force -label post_route
   ```
   - This command saves the current state of the hierarchical block with the label `post_route`.
   - `-force` ensures that the block is saved even if there are warnings or if the block already exists.
   - `-label post_route` assigns the label `post_route` to the saved block.

9. **Save All Libraries**
   ```tcl
   save_lib -all
   ```
   - This command saves all the libraries currently in use.
   - `-all` specifies that all libraries should be saved.

### Summary

These commands manage the process of placement, clock tree synthesis, and routing for the design. They set the maximum number of cores for parallel processing, remove temporary corners, set application options, and perform placement optimization. They also handle clock tree synthesis and automatic routing, create standard cell fillers, and save the current state of the hierarchical block and all libraries. This ensures that the design is optimized and all steps are properly documented and saved for future reference.

---

### report_timing

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/09153c82-112c-45ab-8e6d-f788d012844e">

---

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/ecfa9dc4-9fde-4a53-ab3f-0eb7544bf908">





</details>



