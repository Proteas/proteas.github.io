---
layout: post
title:  A Case of Data PAC
date:   2020-06-27
categories: ios
---

## A Case of Data PAC: get_bsdtask_info

### xnu-6153.11.26
```c
void  *
get_bsdtask_info(task_t t)
{
	return t->bsd_info;
}
```

### iOS-v13.5.1-17F80-iPhone11,6
* function address: `0xFFFFFFF007BE04D8`
```assembly
__TEXT_EXEC:__text:FFFFFFF007BE04D8 _get_bsdtask_info
__TEXT_EXEC:__text:FFFFFFF007BE04D8    LDR  X0, [X0,#0x388] ; X0 = bsd_info
__TEXT_EXEC:__text:FFFFFFF007BE04DC    RET
__TEXT_EXEC:__text:FFFFFFF007BE04DC ; End of function _get_bsdtask_info
```

### iOS-v14.0-18A5301v-iPhone11,6
* function address: `0xFFFFFFF007A5385C`
```assembly
__TEXT_EXEC:__text:FFFFFFF007A5385C _get_bsdtask_info
__TEXT_EXEC:__text:FFFFFFF007A5385C    PACIBSP
__TEXT_EXEC:__text:FFFFFFF007A53860    STP    X29, X30, [SP,#-0x10+var_s0]!
__TEXT_EXEC:__text:FFFFFFF007A53864    MOV    X29, SP
__TEXT_EXEC:__text:FFFFFFF007A53868    LDR    X16, [X0,#0x3A0] ; X16 = signed bsd_info
__TEXT_EXEC:__text:FFFFFFF007A5386C    CBZ    X16, loc_FFFFFFF007A538E0
__TEXT_EXEC:__text:FFFFFFF007A53870    ADD    X8, X0, #0x3A0 ; pointer to bsd_info
__TEXT_EXEC:__text:FFFFFFF007A53874    MOVK   X8, #0xCE5A,LSL#48
__TEXT_EXEC:__text:FFFFFFF007A53878    MOV    X17, X16 ; X17 = X16 = signed bsd_info
__TEXT_EXEC:__text:FFFFFFF007A5387C    AUTDA  X16, X8 ; X16 = unsigned bsd_info
__TEXT_EXEC:__text:FFFFFFF007A53880    XPACD  X17 ; X17 = raw bsd_info
__TEXT_EXEC:__text:FFFFFFF007A53884    CMP    X16, X17 ; if not equals, break.
__TEXT_EXEC:__text:FFFFFFF007A53888    B.EQ   loc_FFFFFFF007A53890
__TEXT_EXEC:__text:FFFFFFF007A5388C    BRK    #0xC472
......
__TEXT_EXEC:__text:FFFFFFF007A538F4 ; End of function _get_bsdtask_info
```

## Misc: Some Symbol Address
* Only for reference, can't guantine the correctness.
* iOS-v14.0-18A5301v-iPhone11,6

01. _pid_for_task: `0xFFFFFFF007EC1638`
02. _port_name_to_task_name: `0xFFFFFFF0079F9CB0`
03. _get_bsdtask_info: `0xFFFFFFF007A5385C`
04. _mach_msg_send: `0xFFFFFFF0079DCBEC`
05. _mach_msg_receive: `0xFFFFFFF0079DD410`
06. _mach_msg_overwrite_trap: `0xFFFFFFF0079DD544`
07. _mach_msg_trap: `0xFFFFFFF0079DD9B4`
08. _mach_msg_overwrite: `0xFFFFFFF0079F2390`
09. _ipc_kmsg_copyin_header: `0xFFFFFFF0079C261C`
10. _ipc_kmsg_copyin_body: `0xFFFFFFF0079C373C`
11. __Xmach_vm_read_overwrite: `0xFFFFFFF007A62DFC`
12. _convert_port_to_map: `0xFFFFFFF0079F8FCC`
13. _mach_vm_read_overwrite: `0xFFFFFFF007ADD2EC`
14. __Xmach_vm_write: `0xFFFFFFF007A63010`
15. _vm_map_copy_overwrite: `0xFFFFFFF007AA1F04`

## Reference
1. [https://twitter.com/WangTielei/status/1275253821963407363](https://twitter.com/WangTielei/status/1275253821963407363)
