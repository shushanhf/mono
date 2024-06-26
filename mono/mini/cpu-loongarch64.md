# Copyright (c) 2021 Loongson Technology, Inc
# Author: Qiao Pengcheng (qiaopengcheng@loongson.cn), Liu An(liuan@loongson.cn)
# Licensed under the MIT license. See LICENSE file in the project root for full license information.
# LoongArch64 cpu description file
# this file is read by genmdesc to pruduce a table with all the relevant
# information about the cpu instructions that may be used by the regsiter allocator,
# the scheduler and other parts of the arch-dependent part of mini.
#
# An opcode name is followed by a colon and optional specifiers.
# A specifier has a name, a colon and a value.
# Specifiers are separated by white space.
# Here is a description of the specifiers valid for this file and their
# possible values.
#
# dest:register       describes the destination register of an instruction
# src1:register       describes the first source register of an instruction
# src2:register       describes the second source register of an instruction
#
# register may have the following values:
#	i  integer register
#	l  integer register pair
#	//v  a0 register (output from calls)
#	//V  a0/a1 register pair (output from calls)
#   a  r21 register
#	b  base register (used in address references)
#	f  floating point register
#	//g  floating point register return pair (f0/f1)
#
# len:number         describe the maximun length in bytes of the instruction
#   number is a positive integer
#
# cost:number        describe how many cycles are needed to complete the instruction (unused)
#
# clob:spec          describe if the instruction clobbers registers or has special needs
#
# spec can be one of the following characters:
#	c  clobbers caller-save registers
#	r  'reserves' the destination register until a later instruction unreserves it
#          used mostly to set output registers in function calls
#
# flags:spec        describe if the instruction uses or sets the flags (unused)
#
# spec can be one of the following chars:
# 	s  sets the flags
#       u  uses the flags
#       m  uses and modifies the flags
#
# res:spec          describe what units are used in the processor (unused)
#
# delay:            describe delay slots (unused)
#
# the required specifiers are: len, clob (if registers are clobbered), the registers
# specifiers if the registers are actually used, flags (when scheduling is implemented).
#
# See the code in mini.c/mini-codegen.c for more details on how the specifiers are used.
#
memory_barrier: len:4
nop: len:4
relaxed_nop: len:4
br: len:16
break: len:16

compare: src1:i src2:i len:4
compare_imm: src1:i len:20
fcompare: src1:f src2:f len:12
rcompare: src1:f src2:f len:12
lcompare: src1:i src2:i len:4
lcompare_imm: src1:i len:20
icompare: src1:i src2:i len:4
icompare_imm: src1:i len:12

ceq: dest:i len:16
cgt: dest:i len:16
cgt_un: dest:i len:16
clt: dest:i len:16
clt_un: dest:i len:16

int_beq: len:12
int_bge: len:12
int_bgt: len:12
int_ble: len:12
int_blt: len:12
int_bne_un: len:12
int_bge_un: len:12
int_bgt_un: len:12
int_ble_un: len:12
int_blt_un: len:12

int_ceq: dest:i len:16
int_cgt: dest:i len:12
int_cgt_un: dest:i len:12
int_clt: dest:i len:12
int_clt_un: dest:i len:12
int_cneq: dest:i len:20
int_cge: dest:i len:16
int_cle: dest:i len:16
int_cge_un: dest:i len:16
int_cle_un: dest:i len:16

long_beq: len:8
long_bge: len:8
long_bgt: len:8
long_ble: len:8
long_blt: len:8
long_bne_un: len:8
long_bge_un: len:8
long_bgt_un: len:8
long_ble_un: len:8
long_blt_un: len:8

long_ceq: dest:i len:12
long_cgt: dest:i len:12
long_cgt_un: dest:i len:12
long_clt: dest:i len:12
long_clt_un: dest:i len:12

float_beq:    len:16
float_bne_un: len:16
float_blt:    len:16
float_blt_un: len:16
float_bgt:    len:16
float_bgt_un: len:16
float_bge:    len:16
float_bge_un: len:16
float_ble:    len:16
float_ble_un: len:16

cond_exc_eq: len:8
cond_exc_ne_un: len:8
cond_exc_lt: len:8
cond_exc_lt_un: len:8
cond_exc_gt: len:8
cond_exc_gt_un: len:8
cond_exc_ge: len:8
cond_exc_ge_un: len:8
cond_exc_le: len:8
cond_exc_le_un: len:8
cond_exc_ov: len:8
cond_exc_no: len:8
cond_exc_c: len:8
cond_exc_nc: len:8

cond_exc_ieq: len:16
cond_exc_ine_un: len:16
cond_exc_ilt: len:16
cond_exc_ilt_un: len:16
cond_exc_igt: len:16
cond_exc_igt_un: len:16
cond_exc_ige: len:16
cond_exc_ige_un: len:16
cond_exc_ile: len:16
cond_exc_ile_un: len:16
cond_exc_iov: len:16
cond_exc_ino: len:16
cond_exc_ic: len:16
cond_exc_inc: len:16

localloc: dest:i src1:i len:60
localloc_imm: dest:i len:64
check_this: src1:b len:4
seq_point: len:24 clob:c
il_seq_point: len:0

voidcall: len:28 clob:c
voidcall_reg: src1:i len:24 clob:c
voidcall_membase: src1:b len:32 clob:c

call: dest:v clob:c len:28
call_reg: dest:v src1:i len:24 clob:c
call_membase: dest:v src1:b len:32 clob:c

fcall: dest:f len:28 clob:c
fcall_reg: dest:f src1:i len:24 clob:c
fcall_membase: dest:f src1:b len:32 clob:c

rcall: dest:f len:28 clob:c
rcall_reg: dest:f src1:i len:24 clob:c
rcall_membase: dest:f src1:b len:32 clob:c

lcall: dest:V len:28 clob:c
lcall_reg: dest:V src1:i len:24 clob:c
lcall_membase: dest:V src1:b len:32 clob:c

vcall: len:16 clob:c
vcall_reg: src1:i len:20 clob:c
vcall_membase: src1:b len:20 clob:c

vcall2: len:28 clob:c
vcall2_reg: src1:i len:24 clob:c
vcall2_membase: src1:b len:32 clob:c
dyn_call: src1:i src2:i len:216 clob:c

tailcall: len:255 clob:c # FIXME len
tailcall_membase: src1:b len:255 clob:c # FIXME len
tailcall_reg: src1:b len:255 clob:c # FIXME len
# tailcall_parameter models the size of moving one parameter,
# so that the required size of a branch around a tailcall can
# be accurately estimated; something like:
# void f1(volatile long *a)
# {
# a[large] = a[another large]
# }
#
# This is two instructions typically, but can be 6 for frames larger than 32K.
# FIXME A fixed size sequence to move parameters would moot this.
tailcall_parameter: len:24

arglist: src1:i len:12

setfret: src1:f len:12
setlret: src1:i src2:i len:12

iconst: dest:i len:12
i8const: dest:l len:24
r4const: dest:f len:20
r8const: dest:f len:28

# Linear IR opcodes
dummy_use: src1:i len:0
dummy_iconst: dest:i len:0
dummy_i8const: dest:i len:0
dummy_r8const: dest:f len:0
dummy_r4const: dest:f len:0
not_reached: len:0
not_null: src1:i len:0

label: len:0
switch: src1:i len:12

store_membase_reg: dest:b src1:i len:20
store_membase_imm: dest:b len:20
storei1_membase_imm: dest:b len:20
storei1_membase_reg: dest:b src1:i len:20
storei2_membase_imm: dest:b len:20
storei2_membase_reg: dest:b src1:i len:20
storei4_membase_imm: dest:b len:20
storei4_membase_reg: dest:b src1:i len:20
storei8_membase_imm: dest:b len:20
storei8_membase_reg: dest:b src1:i len:20
storer4_membase_reg: dest:b src1:f len:20
storer8_membase_reg: dest:b src1:f len:20
load_membase: dest:i src1:b len:20
loadi1_membase: dest:i src1:b len:20
loadu1_membase: dest:i src1:b len:20
loadi2_membase: dest:i src1:b len:20
loadu2_membase: dest:i src1:b len:20
loadi4_membase: dest:i src1:b len:20
loadu4_membase: dest:i src1:b len:20
loadi8_membase: dest:i src1:b len:20
loadr4_membase: dest:f src1:b len:20
loadr8_membase: dest:f src1:b len:20
load_memindex: dest:i src1:b src2:i len:4
loadi1_memindex: dest:i src1:b src2:i len:12
loadu1_memindex: dest:i src1:b src2:i len:12
loadi2_memindex: dest:i src1:b src2:i len:12
loadu2_memindex: dest:i src1:b src2:i len:12
loadi4_memindex: dest:i src1:b src2:i len:12
loadu4_memindex: dest:i src1:b src2:i len:12
loadr4_memindex: dest:f src1:b src2:i len:12
loadr8_memindex: dest:f src1:b src2:i len:12
store_memindex: dest:b src1:i src2:i len:12
storei1_memindex: dest:b src1:i src2:i len:12
storei2_memindex: dest:b src1:i src2:i len:12
storei4_memindex: dest:b src1:i src2:i len:12
storer4_memindex: dest:b src1:f src2:i len:12
storer8_memindex: dest:b src1:f src2:i len:12
loadu4_mem: dest:i len:8

move: dest:i src1:i len:4
fmove: dest:f src1:f len:4
rmove: dest:f src1:f len:4

move_f_to_i4: dest:i src1:f len:8
move_i4_to_f: dest:f src1:i len:8
move_f_to_i8: dest:i src1:f len:4
move_i8_to_f: dest:f src1:i len:4

add_imm: dest:i src1:i len:20
sub_imm: dest:i src1:i len:20
mul_imm: dest:i src1:i len:20
and_imm: dest:i src1:i len:12
or_imm: dest:i src1:i len:12
xor_imm: dest:i src1:i len:12
shl_imm: dest:i src1:i len:8
shr_imm: dest:i src1:i len:8
shr_un_imm: dest:i src1:i len:8

# 64 bit opcodes: binops_op
long_add: dest:i src1:i src2:i len:4
long_sub: dest:i src1:i src2:i len:4
long_mul: dest:i src1:i src2:i len:32
long_div: dest:i src1:i src2:i len:40
long_div_un: dest:i src1:i src2:i len:16
long_rem: dest:i src1:i src2:i len:48
long_rem_un: dest:i src1:i src2:i len:24
long_and: dest:i src1:i src2:i len:4
long_or: dest:i src1:i src2:i len:4
long_xor: dest:i src1:i src2:i len:4
long_shl: dest:i src1:i src2:i len:4
long_shr: dest:i src1:i src2:i len:4
long_shr_un: dest:i src1:i src2:i len:4

long_add_imm: dest:i src1:i clob:1 len:20
long_sub_imm: dest:i src1:i clob:1 len:20
long_and_imm: dest:i src1:i clob:1 len:12
long_or_imm: dest:i src1:i clob:1 len:12
long_xor_imm: dest:i src1:i clob:1 len:12
long_mul_imm: dest:i src1:i len:20
long_shl_imm: dest:i src1:i len:4
long_shr_imm: dest:i src1:i len:4
long_shr_un_imm: dest:i src1:i len:4

long_add_ovf: dest:i src1:i src2:i len:16
long_add_ovf_un: dest:i src1:i src2:i len:16
long_mul_ovf: dest:i src1:i src2:i len:16
long_mul_ovf_un: dest:i src1:i src2:i len:16
long_sub_ovf: dest:i src1:i src2:i len:16
long_sub_ovf_un: dest:i src1:i src2:i len:16

# 64 bit opcodes: unops_op
long_neg: dest:i src1:i len:4
long_not: dest:i src1:i len:4
long_conv_to_i1: dest:i src1:l len:32
long_conv_to_i2: dest:i src1:l len:32
long_conv_to_i4: dest:i src1:l len:32
long_conv_to_r4: dest:f src1:l len:32
long_conv_to_r8: dest:f src1:l len:32
long_conv_to_u1: dest:i src1:l len:32
long_conv_to_u2: dest:i src1:l len:32
long_conv_to_u4: dest:i src1:l len:32
long_conv_to_u8: dest:l src1:l len:32
long_conv_to_i:  dest:i src1:l len:32

#long_conv_to_ovf_i: dest:i src1:i src2:i len:32
long_conv_to_ovf_i4_2: dest:i src1:i src2:i len:32
long_conv_to_r_un: dest:f src1:i len:44

zext_i4: dest:i src1:i len:16
sext_i4: dest:i src1:i len:16

ckfinite: dest:f src1:f len:52
jump_table: dest:i len:16

# 32 bit opcodes: binops_op
int_add: dest:i src1:i src2:i len:4
int_sub: dest:i src1:i src2:i len:4
int_mul: dest:i src1:i src2:i len:16
int_div: dest:i src1:i src2:i len:84
int_div_un: dest:i src1:i src2:i len:40
int_rem: dest:i src1:i src2:i len:84
int_rem_un: dest:i src1:i src2:i len:40
int_and: dest:i src1:i src2:i len:8
int_or: dest:i src1:i src2:i len:8
int_xor: dest:i src1:i src2:i len:8
int_shl: dest:i src1:i src2:i len:4
int_shr: dest:i src1:i src2:i len:4
int_shr_un: dest:i src1:i src2:i len:4

int_add_ovf: dest:i src1:i src2:i len:16
int_add_ovf_un: dest:i src1:i src2:i len:16
int_mul_ovf: dest:i src1:i src2:i len:56
int_mul_ovf_un: dest:i src1:i src2:i len:56
int_sub_ovf: dest:i src1:i src2:i len:16
int_sub_ovf_un: dest:i src1:i src2:i len:16

# 32 bit opcodes: unops_op
int_neg: dest:i src1:i len:4
int_not: dest:i src1:i len:4
int_conv_to_i1: dest:i src1:i len:8
int_conv_to_i2: dest:i src1:i len:8
int_conv_to_i4: dest:i src1:i len:4
int_conv_to_r4: dest:f src1:i len:36
int_conv_to_r8: dest:f src1:i len:36
int_conv_to_u1: dest:i src1:i len:4
int_conv_to_u2: dest:i src1:i len:8
int_conv_to_u4: dest:i src1:i
int_conv_to_r_un: dest:f src1:i len:32


int_adc: dest:i src1:i src2:i len:4
int_addcc: dest:i src1:i src2:i len:4
int_subcc: dest:i src1:i src2:i len:4
int_sbb: dest:i src1:i src2:i len:4
int_adc_imm: dest:i src1:i len:12
int_sbb_imm: dest:i src1:i len:12

int_add_imm: dest:i src1:i len:12
int_sub_imm: dest:i src1:i len:12
int_mul_imm: dest:i src1:i len:12
int_div_imm: dest:i src1:i len:20
int_div_un_imm: dest:i src1:i len:12
int_rem_imm: dest:i src1:i len:28
int_rem_un_imm: dest:i src1:i len:16
int_and_imm: dest:i src1:i len:16
int_or_imm: dest:i src1:i len:16
int_xor_imm: dest:i src1:i len:16
int_shl_imm: dest:i src1:i len:8
int_shr_imm: dest:i src1:i len:8
int_shr_un_imm: dest:i src1:i len:8

# float opcodes: binops_op
float_add: dest:f src1:f src2:f len:4
float_sub: dest:f src1:f src2:f len:4
float_mul: dest:f src1:f src2:f len:4
float_div: dest:f src1:f src2:f len:4
float_div_un: dest:f src1:f src2:f len:4
float_rem: dest:f src1:f src2:f len:16
float_rem_un: dest:f src1:f src2:f len:16
r4_add: dest:f src1:f src2:f len:4
r4_sub: dest:f src1:f src2:f len:4
r4_mul: dest:f src1:f src2:f len:4
r4_div: dest:f src1:f src2:f len:4
r4_rem: dest:f src1:f src2:f len:16

float_neg: dest:f src1:f len:4
float_not: dest:f src1:f len:4
float_conv_to_i1: dest:i src1:f len:12
float_conv_to_i2: dest:i src1:f len:40
float_conv_to_i4: dest:i src1:f len:40
float_conv_to_i8: dest:l src1:f len:40
float_conv_to_r4: dest:f src1:f len:8
float_conv_to_u4: dest:i src1:f len:52
float_conv_to_u8: dest:l src1:f len:48
float_conv_to_i: dest:i src1:f len:40
float_conv_to_u: dest:i src1:f len:36
float_conv_to_u1: dest:i src1:f len:52
float_conv_to_u2: dest:i src1:f len:52

float_ceq: dest:i src1:f src2:f len:8
float_cgt: dest:i src1:f src2:f len:8
float_cgt_un: dest:i src1:f src2:f len:8
float_clt: dest:i src1:f src2:f len:8
float_clt_un: dest:i src1:f src2:f len:8
float_cneq: dest:i src1:f src2:f len:8
float_cge: dest:i src1:f src2:f len:8
float_cle: dest:i src1:f src2:f len:8

r4_neg: dest:f src1:f len:4
r4_ceq: dest:i src1:f src2:f len:8
r4_cgt: dest:i src1:f src2:f len:8
r4_cgt_un: dest:i src1:f src2:f len:8
r4_clt: dest:i src1:f src2:f len:8
r4_clt_un: dest:i src1:f src2:f len:8
r4_cneq: dest:i src1:f src2:f len:8
r4_cge: dest:i src1:f src2:f len:8
r4_cle: dest:i src1:f src2:f len:8

# R4 opcodes
r4_conv_to_i1: dest:i src1:f len:12
r4_conv_to_u1: dest:i src1:f len:52
r4_conv_to_i2: dest:i src1:f len:12
r4_conv_to_u2: dest:i src1:f len:52
r4_conv_to_i4: dest:i src1:f len:12
r4_conv_to_u4: dest:i src1:f len:52
r4_conv_to_i8: dest:l src1:f len:8
r4_conv_to_i: dest:l src1:f len:12
r4_conv_to_u8: dest:l src1:f len:48
r4_conv_to_r4: dest:f src1:f len:4
r4_conv_to_r8: dest:f src1:f len:4

# aot compiler.
aotconst: dest:i len:16

# exception related opcodes
call_handler: len:20 clob:c
start_handler: len:16
endfilter: src1:i len:20
endfinally: len:20
get_ex_obj: dest:i len:16
throw: src1:i len:24
rethrow: src1:i len:24

bigmul: len:52 dest:l src1:i src2:i
bigmul_un: len:52 dest:l src1:i src2:i

sqrt: dest:f src1:f len:4
adc: dest:i src1:i src2:i len:4
addcc: dest:i src1:i src2:i len:4
subcc: dest:i src1:i src2:i len:4
adc_imm: dest:i src1:i len:12
addcc_imm: dest:i src1:i len:12
subcc_imm: dest:i src1:i len:12
sbb: dest:i src1:i src2:i len:4
sbb_imm: dest:i src1:i len:12
br_reg: src1:i len:8

###atomic ops
atomic_load_i1: dest:i src1:b len:24
atomic_load_u1: dest:i src1:b len:24
atomic_load_i2: dest:i src1:b len:24
atomic_load_u2: dest:i src1:b len:24
atomic_load_i4: dest:i src1:b len:24
atomic_load_u4: dest:i src1:b len:24
atomic_load_i8: dest:i src1:b len:20
atomic_load_u8: dest:i src1:b len:20
atomic_load_r4: dest:f src1:b len:28
atomic_load_r8: dest:f src1:b len:24
atomic_store_i1: dest:b src1:i len:20
atomic_store_u1: dest:b src1:i len:20
atomic_store_i2: dest:b src1:i len:20
atomic_store_u2: dest:b src1:i len:20
atomic_store_i4: dest:b src1:i len:20
atomic_store_u4: dest:b src1:i len:20
atomic_store_i8: dest:b src1:i len:20
atomic_store_u8: dest:b src1:i len:20
atomic_store_r4: dest:b src1:f len:28
atomic_store_r8: dest:b src1:f len:24

atomic_add_i4: dest:i src1:i src2:i len:32
atomic_add_i8: dest:i src1:i src2:i len:32
atomic_exchange_i4: dest:i src1:i src2:i len:32
atomic_exchange_i8: dest:i src1:i src2:i len:32
atomic_and_i4: dest:i src1:i src2:i len:32
atomic_and_i8: dest:i src1:i src2:i len:32
atomic_or_i4: dest:i src1:i src2:i len:32
atomic_or_i8: dest:i src1:i src2:i len:32

###debuging support.
liverange_start: len:0
liverange_end: len:0

###gc
gc_liveness_def: len:0
gc_liveness_use: len:0
gc_spill_slot_liveness_def: len:0
gc_param_slot_liveness_def: len:0
gc_safe_point: len:0

##LoongArch64 specific
la_ceq: dest:i src1:i src2:i len:16
la_cgt: dest:i src1:i src2:i len:12
la_cgt_un: dest:i src1:i src2:i len:12
la_clt: dest:i src1:i src2:i len:12
la_clt_un: dest:i src1:i src2:i len:12

la_long_ceq: dest:i src1:i src2:i len:8
la_long_cgt: dest:i src1:i src2:i len:4
la_long_cgt_un: dest:i src1:i src2:i len:4
la_long_clt: dest:i src1:i src2:i len:4
la_long_clt_un: dest:i src1:i src2:i len:4

la_int_ceq: dest:i src1:i src2:i len:16
la_int_cgt: dest:i src1:i src2:i len:12
la_int_cgt_un: dest:i src1:i src2:i len:12
la_int_clt: dest:i src1:i src2:i len:12
la_int_clt_un: dest:i src1:i src2:i len:12
la_int_cneq: dest:i src1:i src2:i len:20
la_int_cge: dest:i src1:i src2:i len:16
la_int_cle: dest:i src1:i src2:i len:16
la_int_cge_un: dest:i src1:i src2:i len:16
la_int_cle_un: dest:i src1:i src2:i len:16

la_slt: dest:i src1:i src2:i len:4
la_sltu: dest:i src1:i src2:i len:4
la_slti: dest:i src1:i len:4
la_sltui: dest:i src1:i len:4
la_setfreg_r4: dest:f src1:f len:4

la_cond_exc_eq: src1:i src2:i len:8
la_cond_exc_ne_un: src1:i src2:i len:8
la_cond_exc_lt: src1:i src2:i len:8
la_cond_exc_lt_un: src1:i src2:i len:8
la_cond_exc_gt: src1:i src2:i len:8
la_cond_exc_gt_un: src1:i src2:i len:8
la_cond_exc_ge: src1:i src2:i len:8
la_cond_exc_ge_un: src1:i src2:i len:8
la_cond_exc_le: src1:i src2:i len:8
la_cond_exc_le_un: src1:i src2:i len:8
la_cond_exc_ov: src1:i src2:i len:8
la_cond_exc_no: src1:i src2:i len:8
la_cond_exc_c: src1:i src2:i len:8
la_cond_exc_nc: src1:i src2:i len:8

la_cond_exc_ieq: src1:i src2:i len:16
la_cond_exc_ine_un: src1:i src2:i len:16
la_cond_exc_ilt: src1:i src2:i len:16
la_cond_exc_ilt_un: src1:i src2:i len:16
la_cond_exc_igt: src1:i src2:i len:16
la_cond_exc_igt_un: src1:i src2:i len:16
la_cond_exc_ige: src1:i src2:i len:16
la_cond_exc_ige_un: src1:i src2:i len:16
la_cond_exc_ile: src1:i src2:i len:16
la_cond_exc_ile_un: src1:i src2:i len:16
la_cond_exc_iov: src1:i src2:i len:16
la_cond_exc_ino: src1:i src2:i len:16
la_cond_exc_ic: src1:i src2:i len:16
la_cond_exc_inc: src1:i src2:i len:16

la_long_beq: src1:i src2:i len:4
la_long_bge: src1:i src2:i len:4
la_long_bgt: src1:i src2:i len:4
la_long_ble: src1:i src2:i len:4
la_long_blt: src1:i src2:i len:4
la_long_bne_un: src1:i src2:i len:4
la_long_bge_un: src1:i src2:i len:4
la_long_bgt_un: src1:i src2:i len:4
la_long_ble_un: src1:i src2:i len:4
la_long_blt_un: src1:i src2:i len:4

la_int_beq: src1:i src2:i len:12
la_int_bge: src1:i src2:i len:12
la_int_bgt: src1:i src2:i len:12
la_int_ble: src1:i src2:i len:12
la_int_blt: src1:i src2:i len:12
la_int_bne_un: src1:i src2:i len:12
la_int_bge_un: src1:i src2:i len:12
la_int_bgt_un: src1:i src2:i len:12
la_int_ble_un: src1:i src2:i len:12
la_int_blt_un: src1:i src2:i len:12

la_float_cneq_un: dest:i src1:f src2:f len:8
la_float_cge_un: dest:i src1:f src2:f len:8
la_float_cle_un: dest:i src1:f src2:f len:8
la_r4_cneq_un: dest:i src1:f src2:f len:8
la_r4_cge_un: dest:i src1:f src2:f len:8
la_r4_cle_un: dest:i src1:f src2:f len:8

la_long_beqz: src1:i len:8
la_long_bnez: src1:i len:8
