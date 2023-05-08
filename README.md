# Formal-Verification-in-VIS
Learn More At: https://ptolemy.berkeley.edu/projects/embedded/research/vis/doc/VisUser/vis_user/node5.html

## VL2MV
### Changes in the RTL code done to convert verilog file into blif-mv file - These are required as
the vl2mv convertor (i.e. from verilog to blif-mv file) supports a restricted set of verilog code, so in that
direction, following changes was made.
1. All the modules that are being used for the functionality of the code should be under 1 verilog file. If the
modules were called from some other files and were defined else were, than those have to be brought
under one common file.
2. ` Operator which is used as a directive for including or defining some files or values is no supported as it
is a reserved key word for yacc parser used by vl2mv.
3. Non-blocking assignments are not supported as VIS allows only one way of introducing the non-
determinism i.e. through usage of #ND operator. So the code should be made free from non-blocking
statements. However, making such a change is sure to affect the logical functionality of the RTL code so
additional changes have to be made to compensate for the same.
4. Delay in the assignment operation is not supported by the VIS so the delays have to be removed from
the code.
5. All the registers should be initialized to their reset value in an initial block. VIS doesn’t allow garbage
values to be assigned to the registers.
6. VIS supports only one universal clock for the entire implementation of the RTL code, so the appropriate
changes have to be made.
7. ‘Else’ condition is not supported in the ‘if’ loop, it should be ‘else if’ all the time.

### Changes made in the RTL code to write the CTL properties -
1. The output variables used in the assign block should be ‘wired’ and those appearing the always block
should be ‘registered’.
2. Input variable can’t be directly used in the CTL formulas. Thus they have to be registered or the other
way is to set a registered flag at the input condition, which you intend to use in the CTL formula. Than
that flag can be directly used to specify certain condition the corresponding input variable.

### Steps for running the CTL properties in VIS 2.4 –
1. Convert the verilog file into .mv format. The command used is
vl2mv/home/amruta/Desktop/verification/8051_verilog_code/8051/trunk/rtl//1_12_2013_new/oc805
1_tc.v
2. Read the mv format into VIS using
read_blif_mv/home/amruta/Desktop/verification/8051_verilog_code/8051/trunk/rtl//1_12_2013_new
/oc8051_tc.mv
3. Initialize verification
init_verify
4. Create a flattened network
flatten_hierarchy
5. Order the MDD variables of the flattened network
static_order
6. Build a partition of MDDs for the flattened network
build_partition_mdds
7. Incrementally verify the CTL properties present in the CTL file
incremental_ctl_verification/home/amruta/Desktop/verification/8051_verilog_code/8051/trunk/rtl/1_
12_2013_new/oc8051_tc.ctl



### Problems faced in the verification process -
1. There was a specific instance where the RTL code line was scon[wr_addr(2:0)] = bit_in, and it was not
being supported. The line said that at index specified by the integer value formed by last 3 bits of
wr_addr of SCON, the bit_in bit should be filled. So, we have to write a case statement for all 8 possible
combinations of the wr_addr(2:0) value and depending on which one was true bit_in was filled at that
index.
2. Most of specifications were not present, so the RTL code itself served as the spec list. This is not the
proper way of writing the properties to verify the implementation.

![Screenshot from 2023-04-25 05-37-44](https://user-images.githubusercontent.com/110079648/236733058-8435fdaf-ecdd-435b-a945-ea2cdbb1b075.png)

## VIS

![Screenshot from 2023-04-25 05-15-27](https://user-images.githubusercontent.com/110079648/236733170-66001546-e4ac-414f-8de4-1520c4ac446f.png)


