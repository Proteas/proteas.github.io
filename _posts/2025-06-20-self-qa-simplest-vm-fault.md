---
layout: post
title:  Self QA：最简单的 Mach VM Fault
date:   2025-06-20
categories: vm
---

## 源码
```c
void vm_fault(int argc, const char * argv[])
{
    mach_vm_address_t addr = 0;
    mach_vm_size_t size = 0x4000;
    mach_vm_allocate(mach_task_self(), &addr, size, VM_FLAGS_ANYWHERE);
    
    *(uint64_t *)addr = 0;                                     // [1]
    
    mach_vm_deallocate(mach_task_self(), addr, size);
}
```

## 问题：当执行 `[1]` 处的代码时，内核中发生了什么？

## 回答

### 调用流程及描述
```
fleh_synchronous
    sleh_synchronous(arm_context_t *context, uint64_t esr, vm_offset_t far, __unused bool did_initiate_panic_lockdown)
        handle_abort
            inspect_data_abort
            handle_user_abort
                is_vm_fault
                vm_fault(map, vm_fault_addr, fault_type, FALSE, VM_KERN_MEMORY_NONE, THREAD_ABORTSAFE, NULL,  0)
                    vm_fault_internal
                        vm_map_lookup_and_lock_object   ; 查找地址对应的 vm_object_t
                        vm_page_lookup                  ; 在 vm_object_t 中查找地址对应的 vm_page_t
                        vm_page_alloc                   ; 由于是第一次访问这个地址，没有页面，
                                                        ; 从页面队列中，获取一个页面，插入到 vm_object_t 页面队列中。
                            vm_page_grab_options
                            vm_page_insert
                        vm_fault_enter_prepare          ; 必要时验证代码签名
                        vm_page_zero_fill               ; 页面数据被清零
                            pmap_zero_page
                        vm_fault_pmap_enter_with_object_lock ; 在物理层面映射页面：
                                                             ; 填充页表项，
                                                             ; 此时用户空间访问对应地址才不会触发异常
                            vm_fault_attempt_pmap_enter
                              pmap_enter_options_check
                                  pmap_enter_options_addr    ; osfmk/arm/pmap/pmap.c
                                      pmap_enter_options_ppl
                                          pmap_enter_options_internal
                        vm_fault_enqueue_page           ; 将页面加入相应的全局页面队列
                        vm_fault_complete               ; 主要是解锁
```

### 概括
1. 查找地址对应的 `vm_object_t。`
2. 从页面队列中，获取一个页面 `vm_page_t`，插入到 `vm_object_t` 页面队列中。
3. 填充虚拟地址对应的页表项，页表项的值是 `vm_page_t` 对应的物理页面的物理地址。
