# Some Quick and Discrete Notes on SPTM

## Glossary
* SPTM: Secure Page Table Monitor
* TXM: Trusted Execution Monitor

## Device List
| Device | Build | Device |
| -------- | ------- | ------- |
| ipad14p | release | iPad_Fall_2021 |
| iphone14 | release | iPhone142 |
| iphone14 | release | iPhone143 |
| iphone14 | release | iPhone144 |
| iphone14 | research | iPhone144 |
| iphone14 | release | iPhone145 |
| iphone14b | release | iPhone147 |
| iphone14b | release | iPhone148 |
| iphone14c | release | iPhone146 |
| iphone15 | release | iPhone152 |
| iphone15 | research | iPhone152 |
| iphone15 | release | iPhone153 |
Seems only A15 & A16 have `SPTM`.

## IMG4
* `sptm-path`: `/usr/standalone/firmware/FUD/Ap,SecurePageTableMonitor.img4`
* `txm-path`: `/usr/standalone/firmware/FUD/Ap,TrustedExecutionMonitor.img4`
* `cl4-path`: `/usr/standalone/firmware/FUD/Ap,cL4.img4`
* `ExclaveOSIntegrityCatalog-path`: `/usr/standalone/firmware/FUD/Ap,ExclaveOSIntegrityCatalog.img4`
* `ExclaveOSTrustCache-path`: `/usr/standalone/firmware/FUD/Ap,ExclaveOSTrustCache.img4`

## `BuildManifest.plist`
Keys:
1. `InstalledSPTM`
2. `RestoreSPTM`
3. `Ap,RestoreSecurePageTableMonitor`
4. `Ap,RestoreTrustedExecutionMonitor`
5. `Ap,SecurePageTableMonitor`: `Firmware/sptm.t8110.release.im4p`
6. `Ap,TrustedExecutionMonitor`: `Firmware/txm.iphoneos.release.im4p`
7. 
Seems `SPTM` is a standalone unit.

## MachO

* `Firmware/sptm.t8110.release.im4p`
* `Firmware/txm.iphoneos.release.im4p`
They are not encrypted, can get machos from them, and **do further analysis**.

## iOS-Kernel
* Segment: `__DATA_SPTM`


## References
1. https://twitter.com/doadam/status/1666353556700405761
2. https://theapplewiki.com/wiki/Keys:DawnSeed_21A5248v_(iPhone14,5)

## Appendices

### `strings` of `sptm.t8110.release.bin`
```
[TXM/SK] Unhandled synchronous exception taken from GL0/GL1
[SPTM] Synchronous exception taken while SP0 selected in GL2
[SPTM Dispatch] Synchronous exception taken while SP1 selected in GL2
Assertion failed: %s, function %s, file %s, line %d.
output != NULL
src/libc/stdio/format.c
__liblibc_format_string
%%lc specifier not supported
%%n specifier not supported
__vsnprintf_chk
src/libc/stdio/vsnprintf_chk.c
Security assertion failed: %s, function %s, file %s, line %d.
(len) <= (dstlen)
__memcpy_chk
src/libc/string/memcpy_chk.c
__stack_chk_fail
src/libc/sys/sys_stack_chk_fail.c
VIOLATION_UAT_INVALID_SUBTYPE
VIOLATION_UAT_ILLEGAL_TTBR1_SENTINEL
VIOLATION_UAT_INVALID_TYPE
VIOLATION_UAT_INVALID_ROOT_TABLE
VIOLATION_UAT_ILLEGAL_OBJECT_STATE
VIOLATION_UAT_INVALD_OBJECT_OFFSET
VIOLATION_UAT_INVALID_CONTEXT_ID
VIOLATION_UAT_INVALID_REFCNT
VIOLATION_UAT_INVALID_VADDR
VIOLATION_UAT_INVALID_PADDR
VIOLATION_UAT_INVALID_PT_LEVEL
VIOLATION_UAT_ILLEGAL_FW_TABLE
VIOLATION_UAT_ILLEGAL_TTE
VIOLATION_UAT_ILLEGAL_PTE
VIOLATION_UAT_ILLEGAL_PAGE_TABLE
VIOLATION_UAT_INVALID_NUM_SEGS
VIOLATION_UAT_INVALID_SEGMENT
VIOLATION_UAT_ILLEGAL_TTBAT_MAPPING
VIOLATION_UAT_INVALID_NUM_MAPPINGS
VIOLATION_UAT_ILLEGAL_PREPARE_FW_UNMAP
VIOLATION_UAT_ILLEGAL_CACHED_MAPPING
VIOLATION_UAT_ILLEGAL_CACHE_FLUSH
VIOLATION_UAT_INVALID_CTX_ID
VIOLATION_UAT_ILLEGAL_SET_CTX_ID
VIOLATION_UAT_ILLEGAL_REMOVE_CTX_ID
VIOLATION_UAT_ILLEGAL_TTBAT_LOCK
VIOLATION_UAT_INVALID_INFO_TYPE
VIOLATION_UAT_ILLEGAL_CONTINUE_FW_UNMAP
%s: error %d looking up %s
uat_get_dt_prop
%s: %s is not %zu bytes wide (%d)
%s: Passed in root table paddr is the TTBR1 root table paddr %#llx
sptm_uat_init_state
root_table_paddr
expected_subtype
%s: Non-zero root table paddr in state object %p %#llx
sptm_uat_destroy_state
state_object
state_object->context_id
sptm_uat_map_table
prev_refcnt
sptm_uat_unmap_table
%s: Parent page table has a zero refcnt
%s: Corrupted map_data, current segment greater than number of segments %p %zu %zu
sptm_uat_map_continue
%s Invalid cur_seg_offset %zu %zu
%s: Invalid microPPL magic value found in the handoff region. Expected: 0x%llx, Actual: 0x%llx
sptm_uat_prepare_fw_unmap_begin
total_size
sptm_uat_prepare_fw_unmap_continue
state_object_handle
%s: Corrupted prepare_fw_unmap_data, cur_mapping greater than num_mappings %p %zu %zu
flush->flush_state
sptm_uat_unmap_begin
%s: Context ID not set despite cache flush maybe being needed
sptm_uat_unmap_continue
sptm_uat_set_ctx_id
context_id
(&uat_instance->ttbat_lock)
ttba0_entry
ttba1_entry
sptm_uat_remove_ctx_id
flush_state
%s: Valid context ID but the TTBAT already contains invalid entries %p %d
sptm_uat_get_info
info_type
%s: new type not XNU_IOMMU
uat_retype_in
retype_params.type
%s: retyping from XNU_IOMMU but not to XNU_DEFAULT %u
uat_retype_out
uat_validate_vaddr
uat_walk_tables
table_paddr
uat_get_table_ttep
%s: Found an unmanaged page table in a TTBR0 address space, that shouldn't be possible. Page table corruption likely. %#llx %#llx %u
%s: Invalid page table level specified %#llx %u.
uat_vaddr_tt_index
UAT Global
%s: uat_instance is NULL
uat_bootstrap_parse_dt
/arm-io/sgx
%s: error %d looking up /arm-io/sgx
gfx-shared-region-base
gfx-shared-region-size
%s: The TTBR1 Root Table is smaller than a page in size or not a multiple of the PAGE_SIZE %llu
gpu-region-base
gpu-region-size
%s: The TTBAT is smaller than a page in size or not a multiple of the PAGE_SIZE %llu
gfx-handoff-base
gfx-handoff-size
%s: The Handoff region is smaller than a page in size or not a multiple of the PAGE_SIZE %llu
gfx-data-base
gfx-data-size
%s: The GFX data segment is  not a multiple of the PAGE_SIZE %llu
rtkit-private-vm-region-base
RTKit private region base is NULL
RTKit private region base is not page aligned
rtkit-private-vm-region-size
RTKit private region has zero size
RTKit private region size is not a multiple of the L1 TTE size
RTKit private memory region (base=0x%llx, size=0x%llx) wraps around
%s: error %d looking up /defaults
uat-enforce-gpu-carveout
%s: uat-enforce-gpu-carveout is not %zu bytes wide (%d)
uat-vaddr-size
%s: uat-vaddr-size is %zu bytes wide (%d)
%s: Invalid vaddr size specified in device tree (%d). Valid values are %d to %d inclusive
uat_acquire_and_validate_pt_paddr
phys_to_type(pt_paddr)
*iommu_frame_tsd(pt_paddr)
uat_validate_paddr
map_options
uat_state_object_acquire
uat_instance->state_object_size
phys_to_fte(state_object_handle)->type
*iommu_frame_tsd(state_object_handle)
&state_object->current_state
expected_state
UAT_STATE_LOCKED
%s: Attempted to perform a TLB flush on a TTBR1 address but not using the TTBR1 state object %p %#llx
issue_tlbi_by_addr
uat_copy_segments_locally
segments_pa
num_segments
segments_pa_type
%s: Copying segments into %p will overflow the state object page %zu
uat_validate_map_segment
segment->start_paddr
segment->num_mappings
seg_size
last_paddr
uat_map_segment
cur_vaddr
cur_paddr
%s: Corrupted unmap_data, current segment greater than number of segments %p %zu %zu
uuc_segment_walker
uat_validate_unmap_segment
segment->start_vaddr
uuc_unmap_pte_update
%s: Attempted to perform a TLBI-by-ASID on the TTBR1 address space %p
issue_tlbi_by_asid
%s: invalid context ID %p %u
BootKC-ro
BootKC-rs
%s: Error looking up /cpus
sptm_register_cpu
%s: Error initializing DT iterator
%s: Error obtaining CPU node
%s: CPU not found in the device tree %p
cpu-impl-reg
%s: Error looking up image region
%s: DT property has illegal size %zu
acc-impl-reg
cpm-impl-reg
sptm_slide_region
sptm_init.c
target_papt_start
%s: Number of supported PAPT ranges has been exhausted
sptm_cpu_id
physical_id
DeviceTree
BootKC-rx
BootKC-bx
BootKC-rw
BootKC-le
TrustCache
RTBuddySeg
SEPPatches
preoslog
BootArgs
ExclaveOSIntegrityCatalog
ExclaveOSTrustCache
AVAILABLE
PHYS_SLIDE
CL4-entry
CL4-virt
TXM-entry
TXM-virt
BootKC-entry
BootKC-virt
TXM not yet fixed up
%s: Frame has been left untyped during bootstrap %p
sptm_init_txm_bootstrap_complete
DT memory-map is NULL
%s: error %d looking up image region '%s'
init_get_image_region
chosen/memory-map
error %d looking up chosen/memory-map
unsupported entry, bind: %d
unsupported auth_rebase key %d
auth_rebase addrDiv is not 0: %llx
fixup ptr format is unsupported %d
%s: Error looking up /chosen
copy_and_clear_random_seed
random-seed
%s: Error looking up /chosen/random-seed
%s: random-seed (%p) size mismatch (%d) or NULL
%s: BootKC rs region does not fit xnu_ro_data (needed: %zu, have: %zu)
init_xnu_ro_data
%s: xnu ro pagetables don't begin on page boundary
%s: xnu ro pagetables don't end on page boundary
%s: %s not fully within BootKC ro region
xnu_el2_exception_vector
xnu_ctrr_dispatch_table
xnu_ro_pagetables_begin
xnu_ro_pagetables_end-1
%s: region '%s' beginning [%p] or end [%p] is invalid
validate_region_order
%s: region '%s' [%p-%p] not immediately after region '%s' [ending at %p]
AuxKC-ro
AuxKC-rx
%s: CTRR-A already enabled! %#llx
ctrr_lock_boot
VIOLATION_NVME_INVALID_QID
VIOLATION_NVME_INVALID_CID
VIOLATION_NVME_INVALID_PAGE_COUNT
VIOLATION_NVME_INVALID_NVME_PAGE
VIOLATION_NVME_ILLEGAL_CALL
VIOLATION_NVME_ILLEGAL_CID_STATE_TRANSITION
%s: Zero TCB entries per queue is not supported
nvme_bootstrap
%s: Too many TCB entries per queue:%u
%s: iommu_bootstrap_alloc_frames() returns 0
sptm_nvme_map_pages
&nvme_instance->cid_mode[c_id]
CID_EMPTY
CID_BUSY
%s: TCB entry %d is not invalidated in DRAM
%s: TCB entry: %d NLB is not zero
%s: NLB (0x%x) doesn't match seg count (0x%x)
sptm_nvme_unmap_pages
(nvme_cid_mode_t)(((uint8_t) (q_id) << 4) | CID_FILLED_q)
sptm_nvme_set_sq_entry
(nvme_cid_mode_t)(((uint8_t) (q_id) << 4) | CID_SHADOW_q)
%s: NVMe Queue-entries mismatch %d %d
sptm_nvme_init
%s: NVMe Queueing protocol version mismatch %llu
validate_nvme_call_allowed
nvme_instance->allowed_functions
%s: Could not find /arm-io
read_edt_properties
%s: Could not find /arm-io/ans
nvme-linear-sq
nvme-queue-entries
%s: Couldn't find nvme-queue-entries
%s: Could not find /arm-io/ans.reg
%s: /arm-io/ans.reg bad size
Unexpected ANS register size
chosen/carveout-memory-map
region-id-%d
%s: Unexpected size of region id 55
validate_qid
validate_cid
validate_nvme_page
%s: ANS invalidation failed with error: %u
invalidate_tcb_entry
/chosen/lock-regs
%s: /chosen/lock-regs not found
ctrr_amcc_find_lock_group_data
assert_ctrr_amcc_region_unlocked
%s: AMCC %s region has been locked unexpectedly
assert_amcc_cache_disabled
%s: AMCC cache has been enabled unexpectedly
%s: Malformed %s memory region %p %p
ctrr_amcc_map_lock_group
%s: Unable to find 'chosen' DT node
dram-base
%s: Unable to find 'dram-base' entry in the 'chosen' DT node
%s: /chosen/lock-regs/%s not found
ctrr_dt_get_lock_group
aperture-count
%s: %s %s %u exceeds maximum %u
aperture-size
%s: %s have %u apertures, but 0 size
plane-count
plane-stride
%s: %s plane-count (%u) > 1, but stride is 0/missing
%s: %s aperture-size (%#x) is insufficent to cover plane-count (%#x) of plane-stride (%#x) bytes
aperture-phys-addr
%s: %s missing required %s
%s: %s aperture-phys-addr size (%#x) != (aperture-count (%#x) * PA size (%#zx) = %#lx)
cache-status
amcc-ctrr-a
%s: Could not find required property '%s'
ctrr_dt_get_uint32
%s: Found unexpected size for property %s %u
%s-reg-offset
%s-reg-mask
%s-reg-value
%s: /chosen/lock-regs/%s/%s not found
ctrr_dt_get_lock_type
page-size-shift
lower-limit
upper-limit
write-disable
[SPTM] %s: somehow a violation was triggered in early boot. That shouldn't be possible. %u
env_violation
[SPTM] %s: %s
VIOLATION_SART_INVALID_PT
VIOLATION_SART_INVALID_PADDR
VIOLATION_SART_INVALID_N_OPS
VIOLATION_SART_INVALID_SIZE
VIOLATION_SART_INVALID_PERM
VIOLATION_SART_ILLEGAL_STATE
VIOLATION_SART_NO_SPACE
VIOLATION_SART_ILLEGAL_MAP
VIOLATION_SART_ILLEGAL_UNMAP
VIOLATION_SART_CPU_RACE
VIOLATION_SART_INVALID_CONFIG
%s: Invalid SART version %d from EDT
sart_bootstrap
sptm_sart_map_region
sart_instance->iommu_instance.iommu_id
sptm_sart_unmap_region
sptm_sart_set_state
%s: error %d looking up '%s
sart_bootstrap_parse_edt
arm-io/sart-ans
%s: error %d looking up '%s' - SPTM/EDT misconfiguration
sart-version
%s: error %d looking up '%s'
sart-ans/sart-version
%s: DT property '%s' has illegal size %zu
sart-ans/regs
%s: Illegal SART Register Size %d
exclusive-bounds
sart_validate_address_range
enforce_paddr_managed
sptm_internal.h
sart_instance_acquire
(&sart_instance->mtx_guard)
sart_add_region
region_id
%s: n_region overflow
sart_remove_region
%s: n_region underflow
%s: Invalid SART offset %x
sart_write_reg
%s: Invalid bootloader mapping covers managed memory %llx
sart_bootstrap_register_bootloader_mappings
%s: Invalid SART size region %d
sart_get_registers
sart_read_reg
sart_set_registers
t8110dart
VIOLATION_T8110_DART_RACE
VIOLATION_T8110_DART_SID_RACE
VIOLATION_T8110_DART_INVALID_RETYPE_NEW_TYPE
VIOLATION_T8110_DART_INVALID_LEVEL
VIOLATION_T8110_DART_INVALID_RETYPE_LEVEL
VIOLATION_T8110_DART_INVALID_TSD_LEVEL
VIOLATION_T8110_DART_INVALID_DART_ID
VIOLATION_T8110_DART_INVALID_RETYPE_DART_ID
VIOLATION_T8110_DART_INVALID_TSD_DART_ID
VIOLATION_T8110_DART_INVALID_SID
VIOLATION_T8110_DART_INVALID_RETYPE_SID
VIOLATION_T8110_DART_INVALID_TSD_SID
VIOLATION_T8110_DART_INVALID_NBYTES
VIOLATION_T8110_DART_INVALID_DVA
VIOLATION_T8110_DART_INVALID_DVA_END
VIOLATION_T8110_DART_INVALID_FTE_TYPE
VIOLATION_T8110_DART_INVALID_TARGET_LEVEL_TTE
VIOLATION_T8110_DART_INVALID_INTERMEDIATE_LEVEL_TTE
VIOLATION_T8110_DART_INVALID_PTE_REMAP
VIOLATION_T8110_DART_PROT_MISMATCH
VIOLATION_T8110_DART_INVALID_DART_INSTANCE
VIOLATION_T8110_DART_INVALID_POWER_STATE
VIOLATION_T8110_DART_INVALID_ERR_MASK
VIOLATION_T8110_DART_INVALID_HARDWARE_VERSION
VIOLATION_T8110_DART_INVALID_SMMU_WINDOW
VIOLATION_T8110_DART_INVALID_STT_DEPTH
VIOLATION_T8110_DART_INVALID_STT_INDEX
VIOLATION_T8110_DART_FREE_TABLE_IN_USE
VIOLATION_T8110_DART_UNPERMITTED_TLIMIT_CLAMP
VIOLATION_T8110_DART_MULTIPLE_MAP_TABLE
VIOLATION_T8110_DART_SID_ACCESS_DENIED
/Library/Caches/com.apple.xbs/Sources/SPTM/sptm/iommu/dart/t8110dart.c
t8110dart_retype_in
t8110dart.c
t8110dart %s:%s:%d: %s == NULL
outargsp
t8110dart %s:%s:%d: alloc size must be greater than 0
t8110dart_bootstrap_allocate
t8110dart %s:%s:%d: allocation requests must not have a size greater than one page (%#llx)
t8110dart_bootstrap_tz
t8110dart %p (%s:%u): TZ select greater than max
t8110dart %p (%s:%u): TZ config greater than max
t8110dart %p (%s:%u): Disabled TZT region %u invalid %#x %#x %#x
t8110dart %p (%s:%u): Enabled TZT region %u invalid %#x %#x %#x
t8110dart %p (%s:%u): SID %u is greater than the maximum
t8110dart %p (%s:%u): SID 0 is invalid with Dynamic SID Assignment
apf-bypass
t8110dart %p (%s:%u): APF bypass and Dynamic SIDs are mutually exclusive
t8110dart %p (%s:%u): overflow
t8110dart %p (%s:%u): vm-base-%u must be of size %zu
t8110dart %p (%s:%u): vm-size-%u must be of size %zu
t8110dart %p (%s:%u): %hu: Per-SID vm-base/vm-size override not supported for this class of DART
t8110dart %p (%s:%u): SID %u: per-sid vm-base/vm-size override unsupported for Dynamic SID Assignment
t8110dart %p (%s:%u): Per-SID (%u) vm-base/vm-size override may increase VM limits
t8110dart %p (%s:%u): Per-SID (%u) vm-size must be within global range
t8110dart %p (%s:%u): SID %u end page %u is greater than maximum
t8110dart %p (%s:%u): Invalid SID (%u) configured as bypass
t8110dart %p (%s:%u): SID %u: remap and bypass are mutually exclusive
t8110dart %p (%s:%u): SID %u: Per-SID VM limits incompatible with bypass
t8110dart %p (%s:%u): SID %u: remap and translation are mutually exclusive
pt-region
t8110dart %p (%s:%u): pt-region-%d incorrectly sized
t8110dart %p (%s:%u): SID %u: page table carveout not page-aligned
t8110dart %p (%s:%u): SID %u: page table carveout size not page-aligned
t8110dart %p (%s:%u): carveout pages %zu must be < %u
real-time
t8110dart %p (%s:%u): Cannot specify Per-SID (%u) realtime with global realtime
sid-count
t8110dart %p (%s:%u): sid-count %u is greater than max permitted sids %u
t8110dart %p (%s:%u): SID Remap incompatible with Dynamic SID Assignment
t8110dart %p (%s:%u): Malformed 'remap' property
t8110dart %p (%s:%u): remap %u -> %u is greater than max SID %u
t8110dart %p (%s:%u): Remap src SID %u already remapped to %u
t8110dart %p (%s:%u): Remap dst SID %u already remapped to %u
t8110dart %p (%s:%u): SID %u: SID remapped to itself: %#x
t8110dart %p (%s:%u): missing %s property
t8110dart %p (%s:%u): sid count %u greater than maximum
t8110dart %p (%s:%u): SID count (%u) must be greater than 0
t8110dart %p (%s:%u): malformed %s property
t8110dart %p (%s:%u): mismatch between 'sid-count' (%u) and 'sid' (%u) property length
exclave-sid
t8110dart %p (%s:%u): exclave sid count %u greater than maximum
t8110dart %p (%s:%u): SID %u: Not initialized
t8110dart %p (%s:%u): SID remap %u->%u invalid target
t8110dart %s:%s:%d: error getting SecureDT
t8110dart_bootstrap
t8110dart %s:%s:%d: error %d looking up arm-io
compatible
dart,t8110
sptm_t8110dart_map_table
pt_paddr
(&state->sids[sid]->lock)
_mga_lock
tte->raw
state_guard_release - %#llx, %#llx
sptm_t8110dart_unmap_table
ttes[level]->raw
nested_level
sptm_t8110dart_map
sptm_t8110dart_unmap
pte->wrprot
sptm_t8110dart_init
(&state->lock)
state->power_state
sptm_t8110dart_powerup
sptm_t8110dart_powerdown
sptm_t8110dart_query_tlb
instance
sptm_t8110dart_set_smmu_window
sptm_t8110dart_read_smmu_stt_index
sptm_t8110dart_clamp_tlimits
sptm_t8110dart_clear_err
err_mask
t8110dart %p (%s:%u): DART instance %u: Unrecoverable secondary error 0x%08x
sptm_t8110dart_clear_exception
sptm_t8110dart_clear_perf_interrupts
sptm_t8110dart_clear_all_interrupts
sptm_t8110dart_disable_translation
sptm_t8110dart_enable_translation
t8110dart %p (%s:%u): Invalid DART instance %u
t8110dart %p (%s:%u): Invalid SMMU instance %u
t8110dart %p (%s:%u): Invalid DAPF instance %u
t8110dart_write_dart_reg
t8110dart_write_dapf_reg
t8110dart_verify_dapf_reg
t8110dart %p (%s:%u): DART instance %u: Attempt to modify locked reg %p 0x%08x->0x%08x
t8110dart_verify_dart_reg
t8110dart_sid_is_valid
t8110dart %s:%s:%d: Invalid physical address %llx
t8110dart_addr_to_te_phy
t8110dart_addr_to_page
sptm_t8110dart_sk_tlbi_request
start_page
end_page
sptm_t8110dart_sk_tlbi_barier
t8110dart_assert_prop_size
t8110dart %s:%s:%d: expected size %u for '%s', but it is %u
t8110dart %p (%s:%u): invalid SID %u
t8110dart %p (%s:%u): SID %u already allocated
t8110dart_get_sid_property
t8110dart %s:%s:%d: invalid %s size %u
t8110dart %p (%s:%u): Invalid level %d
t8110dart %p (%s:%u): SID %u level %hhu table PA %#llx falls outside carveout region
t8110dart %p (%s:%u): SID %u TZ Dead Page only permitted in 3 level translation STEs
t8110dart %p (%s:%u): SID %u dead page must be zero
t8110dart %p (%s:%u): SID %u TZ Dead Page not in TZ REGION at STE %zu
t8110dart %p (%s:%u): SID %u leaf page table flags present in level %hhu page table entry %#zx: 0x%016llx
t8110dart_bootstrap_instance
t8110dart %s:%s:%d: error %d getting dart-id
t8110dart %s:%s:%d: error %d invalid DART ID: %u
t8110dart %s:%s:%d: DART ID %u used twice
t8110dart %s:%s:%d: error %d getting name
dart-options
debug-enabled
t8110dart %p (%s:%u): unexpected size for 'debug-enabled'
retention
enable-perf-counters
ioa-parent
allow-mixed-bypass-mode
flush-by-dva
noncompliant-dead-mappings
clamp-tlimits
t8110dart %p (%s:%u): vm-size must be non-zero, but it is %#llx
t8110dart %p (%s:%u): invalid vm-size %#llx
t8110dart %p (%s:%u): vm-base %#llx is greater than the maximum DVA %#llx
t8110dart %p (%s:%u): vm-size %#llx is less than the maximum size %#llx
t8110dart %p (%s:%u): end of vm range %#llx is greater than the maximum DVA %#llx
vm-alignment
t8110dart %p (%s:%u): invalid size %u for vm-alignment
t8110dart %p (%s:%u): invalid vm_alignment
ignore-secondary
t8110dart %p (%s:%u): malformed 'instances' property
t8110dart %p (%s:%u): invalid 'instance' size %u
t8110dart %p (%s:%u): max %s instances exceeded
t8110dart %p (%s:%u): invalid instance type: 0x%08x
t8110dart %p (%s:%u): missing 'reg' property
t8110dart %p (%s:%u): incorrect size for 'reg'
t8110dart %p (%s:%u): mis-aligned DART register size %#llx
t8110dart %p (%s:%u): mis-aligned DART register base %#llx
dart-tunables-instance-%d
t8110dart %p (%s:%u): Malformed dart-tunables-instance-%u
t8110dart %p (%s:%u): Invalid DART instance (%u) for SMMU instance (%u)
t8110dart %p (%s:%u): smmu instance %u 'reg' not page sized
t8110dart %p (%s:%u): mis-aligned SMMU register base %#llx
allow-pte-remap
t8110dart %p (%s:%u): PTE remap not permitted
t8110dart %p (%s:%u): unexpected dram base
t8110dart %p (%s:%u): APF is incompatible with Dynamic SID Assignment
dual-vc-llt-workaround-%d
t8110dart %p (%s:%u): Multiple instances using dual vc limits
t8110dart %p (%s:%u): invalid dual-vc-llt-workaround size
t8110dart %p (%s:%u): num_dual_vc_limits %u exceed maximum %u
t8110dart %p (%s:%u): dual VC LLT limit %d is invalid: [%#llx,%#llx]
dapf-instance-%d
t8110dart %p (%s:%u): malformed dapf-instance-%u
t8110dart %p (%s:%u): dapf-instance-%d must have at least one slice
t8110dart %p (%s:%u): number of apf slices %u greater than max %u
t8110dart %p (%s:%u): Invalid DART instance (%u) for APF instance (%u)
t8110dart %p (%s:%u): apf instance %u 'reg' not page sized
t8110dart %p (%s:%u): mis-aligned APF register base %#llx
t8110dart %p (%s:%u): Invalid limits for APF slice %u instance %u: [%#llx,%#llx]
t8110dart %p (%s:%u): end(%#llx) < start(%#llx) for slice %d instance %u
t8110dart_validate_tunable
t8110dart %p (%s:%u): DART instance %u: invalid tunable offset %#x
t8110dart_update_ttbr
t8110dart %p (%s:%u): (DART instance %u): Locked register update: 0x%08x -> 0x%08x
t8110dart %p (%s:%u): (DART instance %u): Inconsistent write disable set and lock 0x%08x
t8110dart_invalidate_tlb_by_dva
t8110dart %p (%s:%u): Invalid flush-by-DVA range [%#x,%#x)
t8110dart_invalidate_tlb_by_sid
t8110dart_walk_tables
table_pa
t8110dart %p (%s:%u): table_pa==0
t8110dart %p (%s:%u): table==0
t8110dart %p (%s:%u): DART instance %u: Unsupported hardware version %u.%u
t8110dart %p (%s:%u): DART instance %u: hardware version mismatch %u.%u
t8110dart %p (%s:%u): flush-by-dva not supported
t8110dart %p (%s:%u): Non-compliant dead page mapping not supported
t8110dart %p (%s:%u): DART instance %u: Inconsistent hardware definition: min_page %#x hw %#x
t8110dart %p (%s:%u): DART instance %u: Max SID is %u
t8110dart %p (%s:%u): DART instance %u: Invalid VM page limits [%#x,%#x)
t8110dart %p (%s:%u): DART instance %u: invalid SID %u
t8110dart %p (%s:%u): DART instance %u: invalid SID %u full bypass addr %#llx
t8110dart %p (%s:%u): DART instance %d: invalid SID %u legacy bypass addr 0x%llx
t8110dart %p (%s:%u): DART instance %u: SID %u translation disabled
t8110dart %p (%s:%u): DART instance %u: SID %u page limits not set [%#x..%#x)
t8110dart %p (%s:%u): DART instance %u: SID %u page limits [%#x..%#x) outside required limits [%#x..%#x)
t8110dart %p (%s:%u): DART instance %u: 4-level translation not supported for SID %u
t8110dart %p (%s:%u): DART instance %u uses dual-vc-llt-workaround, but has no APF
t8110dart %p (%s:%u): DART instance %u: Too many APF slices, max %u configured %u
t8110dart %p (%s:%u): DART instance %u: Invalid APF slice %u range [%#llx,%#llx]
t8110dart %p (%s:%u): DART instance %u: APF slice %u uses invalid SID %u
t8110dart %p (%s:%u): DART instance %u: DART not locked
t8110dart %p (%s:%u): DART instance %u: Unconfigured SID %u differs from reset value: 0x%08x
t8110dart %p (%s:%u): DART instance %u: SID %u inconsistent remap_enable configuration: actual %#x expected %#x
t8110dart %p (%s:%u): DART instance %u: SID %u inconsistent remap configuration: actual %#x expected %#x
t8110dart %p (%s:%u): DART instance %u: SID %u inconsistent remap ttbr configuration: actual %#llx expected %#x
t8110dart %p (%s:%u): DART instance %u: SID %u inconsistent translation_enable configuration: actual %#x expected %#x
t8110dart %p (%s:%u): DART instance %u: SID %u inconsistent ttbr configuration: actual %#llx expected %#llx
t8110dart %p (%s:%u): DART instance %u: SID %u unexpected ttbr valid with bypass: actual ttbr %#llx expected %#llx
t8110dart %p (%s:%u): DART instance %u: SID %u unexpected non-zero TTBR for full bypass %#llx
t8110dart %p (%s:%u): DART instance %u: SID %u unexpected zero TTBR for legacy bypass %#llx
t8110dart %p (%s:%u): DART instance %u: SID %u inconsistent APF bypass configuration: actual %#x expected %#x
t8110dart_verify_bypass
t8110dart %p (%s:%u): DART instance %u SID %u invalid translation setting
t8110dart %p (%s:%u): DART instance %u: APF not locked
t8110dart %p (%s:%u): Client partitions not supported
t8110dart %p (%s:%u): DART instance %u: Locked register update SID_CONFIG[%u] 0x%08x->0x%08x
t8110dart %p (%s:%u): DART instance %u: TTBR must be invalid for remapped SID(%u)
t8110dart %p (%s:%u): DART instance %u: Locked register update TTBR[%u] 0x%08x->0x%08x
t8110dart %p (%s:%u): instance %u WRITE_DISABLE_LOCK unlocked expected 0x%08x actual 0x%08x
t8110dart %p (%s:%u): instance %u WRITE_DISABLE enabled expected 0x%08x actual 0x%08x
t8110dart %p (%s:%u): instance %u %s changed expected 0x%08x actual 0x%08x
(TZ_SELECT)
(TZ_CONFIG)
(TZ_REGION0_START)
(TZ_REGION0_END)
(TZ_REGION0_OFFSET)
(TZ_REGION1_START)
(TZ_REGION1_END)
(TZ_REGION1_OFFSET)
(TZ_REGION2_START)
(TZ_REGION2_END)
(TZ_REGION2_OFFSET)
t8110dart %p (%s:%u): TZ Dead Page is not supported for hw version: %#hx
t8110dart %p (%s:%u): TZ Dead Page must be zero with build_params2.trustzone_tagger_supported
t8110dart_poll
t8110dart %p (%s:%u): Invalid poller type %d
t8110dart %p (%s:%u): Invalid poller type: %u
t8110dart %p (%s:%u): DART instance %u: %s still busy after %zu polls, OP=%#x
t8110dart %p (%s:%u): Flush Unlock not supported
t8110dart_tlb_flush_unlock
t8110dart %p (%s:%u): Not serialized request from %s
t8110dart %p (%s:%u): Unsupported TLB operation %#x
%s: Invalid element_size/num_elements %zd %d
copy_array_to_scratch
sptm_misc.c
array_fte->type
array_pa
element_size
num_elements
%s: Offset into scratch page crosses page boundary %zu %zu
VIOLATION_INVALID_PT
VIOLATION_INVALID_LEVEL
VIOLATION_INVALID_GEOMETRY_ID
VIOLATION_INVALID_IOMMU_ID
VIOLATION_INVALID_ASID
VIOLATION_INVALID_PADDR
VIOLATION_INVALID_VADDR
VIOLATION_INVALID_VADDR_RANGE
VIOLATION_INVALID_TYPE
VIOLATION_INVALID_N_OPS
VIOLATION_INVALID_ARRAY
VIOLATION_INVALID_SHARED_RT
VIOLATION_INVALID_USER_RT
VIOLATION_INVALID_OPTIONS
VIOLATION_INVALID_INSTANCE_ID
VIOLATION_INVALID_FLAG
VIOLATION_INVALID_CPU_PHYSICAL_ID
VIOLATION_ILLEGAL_CPU_REGISTRATION
VIOLATION_ILLEGAL_RETYPE
VIOLATION_ILLEGAL_MAP
VIOLATION_ILLEGAL_NEST
VIOLATION_ILLEGAL_UNNEST
VIOLATION_ILLEGAL_UPDATE
VIOLATION_ILLEGAL_UNMAP
VIOLATION_ILLEGAL_UPGRADE
VIOLATION_ILLEGAL_DOWNGRADE
VIOLATION_ILLEGAL_PTE
VIOLATION_ILLEGAL_TTE
VIOLATION_ILLEGAL_SPRR_INDEX
VIOLATION_ILLEGAL_NG_FIELD
VIOLATION_ILLEGAL_PTE_WRITABLE
VIOLATION_ILLEGAL_SPANS_TABLES
VIOLATION_ILLEGAL_MAPPING_TYPE
VIOLATION_ILLEGAL_DISPATCH_DOMAIN
VIOLATION_ILLEGAL_DISPATCH_TABLE
VIOLATION_ILLEGAL_DISPATCH_PERMISSION
VIOLATION_ILLEGAL_DISPATCH_ENTRY_POINT
VIOLATION_ILLEGAL_DISPATCH_TXM_STACK
VIOLATION_ILLEGAL_SPTM_ENDPOINT
VIOLATION_ILLEGAL_SPTM_DISPATCH_TABLE
VIOLATION_ILLEGAL_SPTM_EXC_RETURN
VIOLATION_SIGN_ILLEGAL_KEY
VIOLATION_AUTH_ILLEGAL_KEY
VIOLATION_ILLEGAL_SLIDE
VIOLATION_DOUBLE_NEST
VIOLATION_MISMATCHED_PT_GEOMETRIES
VIOLATION_FRAME_HELD_EXCLUSIVE
VIOLATION_FRAME_HELD_SHARED
VIOLATION_FRAME_OWNER
VIOLATION_FRAME_PROTECTED
VIOLATION_FRAME_TYPE
VIOLATION_OVERFLOW_RO_REFCNT
VIOLATION_OVERFLOW_WX_REFCNT
VIOLATION_OVERFLOW_NESTED_REFCNT
VIOLATION_OVERFLOW_USER_REFCNT
VIOLATION_SHARED_REGIONS_EXHAUSTED
VIOLATION_SHARED_REGIONS_ALREADY_CONFIGURED
VIOLATION_SHARED_REGIONS_MISMATCH
VIOLATION_SHARED_REGIONS_NOT_CONFIGURED
VIOLATION_ASID_IN_USE
VIOLATION_POSSIBLE_PENDING_TLBI
VIOLATION_TABLE_NOT_EMPTY
VIOLATION_ILLEGAL_COMMPAGE_VADDR
VIOLATION_REG_ID_INVALID
VIOLATION_REG_DENIED_BITS
VIOLATION_POSSIBLE_PENDING_CACHE_FLUSH
%s: failed to get DT
sptm_parse_io_ranges
/defaults node doesn't exist in DT
pmap-io-ranges
%s: %u addr 0x%llx is not page-aligned
sptm_compute_io_ranges
%s: %u length 0x%llx is not page-aligned
%s: %u addr 0x%llx length 0x%llx wraps around
%s: %u addr 0x%llx length 0x%llx overlaps managed physical memory
%s: Failed to get defaults node from EDT
sptm_load_io_ranges
%s: Failed to get pmap-io-ranges property from the defaults node
%s: %p length is greater than 64K pages (%llu)
sptm_register_io_frame
%s: Attempted to register managed page as an IO range %p %u
%s: Attempted to register an empty IO range %p %u
%s: Attempted to register unaligned IO range %p %u
%s: sptm_pmap_io_ranges %u and %u overlap.
sptm_validate_io_ranges
get_ptep
sptm_page_tables.c
cur_level
Found non-page table frame during a page table walk
%s: paddr isn't managed %#llx
papt_update_mapping
%s: invalid flag found while updating PAPT mapping %u
%s: Attempted to update PAPT permissions outside of a retype() operation, this is unsafe.
%s: invalid cache attribute index found while updating PAPT mapping %p %hhuu %hhu
%s: invalid root table type %hhu for PA 0x%llx
sptm_broadcast_tlbi
%s: VA 0x%llx not aligned to root table page size
%s: num_mappings %u exceeds RTLBI threshold
%s: unsupported RTLBI flavor 0x%x
%s: unsupported TLBI flavor 0x%x
%s: Attempted to announce bootstrap stage that has already been reached %p %p
bootstrap_stage_announce
%s: Expected bootstrap stages not reached %p
bootstrap_stage_enforce_after
%s: Unexpected bootstrap stages reached %p
bootstrap_stage_enforce_before
%s: Exceeded available number of bootstrap SPTM CTRR-protected frames
bootstrap_alloc_frames
%s: Exceeded available number of xnu CTRR-protected frames
%s: Attempted to allocate bootstrap frame with invalid CTRR permissions
%s: Called with [num_frames] of zero
bootstrap_retype_frames
%s: Attempted to retype a non-managed frame %p %u
%s: Range overflow while attempting to retype frames %p %u
%s: Invalid new type %u
wrong frame type for SPTM CTRR protected page tables: %d
wrong frame type for page tables: %d
%s: PAPT PTE could not be reached %p
bootstrap_map_region
%s Valid PTE found while attempting to create mapping %p
bootstrap_unmap_region
%s Invalid PTE found while attempting to unmap page %p
request for %u mappings exceeds %u late I/O limit (current: %u), max needed %u
%s: Attempted to unmap non-existent IO region %p %u
bootstrap_unmap_io_region
Required PAPT range could not be found.
%s: Name is NULL
bootstrap_register_papt_range
%s: Invalid bootstrap type %u
%s: Attempted to go over the maximum number of PAPT ranges
bootstrap_retype_papt_range
%s Unable to find requested range %s
%s() No PAPT ranges have been registered
papt_ranges_update
%s() invalid virt_base: %llx
sptm_bootstrap_early
%s() invalid phys_base: %llx
%s() invalid first_avail_phys: %llx
%s() invalid mem_size: %zx
%s: PAPT range is in multiple SPTM domains
%s: [current_contiguous] set on the first PAPT range
%s: Two or more PAPT ranges overlap in VA space %s
xnu ro pagetable page #%d is not XNU owned, but %x
sptm_bootstrap_late
%s: IO Ranges %u and %u overlap
%s: NULL device tree
/defaults
%s: Error looking up /defaults
pmap-max-asids
pmap-max-asids DT property is not a 32-bit integer
pmap-max-asids DT property is greater than 1 << 16
pmap-max-asids DT property is zero
%s: No valid PAPT mapping found while traversing the page tables
papt_to_phys
%s: Trying to run on more CPUs than MAX_CPUS %d >= %d
sptm_cpu_init
rc16_inc
SPRR index is invalid
frame_acquire
sptm_types.c
NULL ptep pointer
table_acquire
%s: Non-PT frame found in page tables %p
pt_fte->type
%s: XNU_PAGE_TABLE frame found while walking Shared Region page tables %p
Incorrect level set in page table FTE
FTE type is not of a CPU page table
NULL FTE with at least one valid PTE (%#llx or %#llx)
%s: Attempted to update refcnts on a non-cpu-page: %d
refcounts_update_page_op
%s(%s:%d)
shared_region_alloc
Shared region ID does not exist %d
shared_region_configure
Unexpected failure while attempting to configure a shared region
rc16_dec
cpu_root_table_retype_in
%s: Unexpected [new_type] while retyping page %d
validate_attribute_index
validate_asid
validate_root_flags
root_flags
cpu_root_table_retype_out
mapping_refcnt
ASID 0x%hx was already clear in sptm_asid_bitmap
user_refcnt
%s: Unexpected [fte->type] while retyping page %d
validate_iommu_id
xnu_iommu_retype_out
iommu_refcnt
validate_pt_level
cpu_page_table_retype_out
nested_refcnt
xnu_default_retype_out
read_refcnt
write_exec_refcnt
%s called on unsupported configuration
sptm_flush_noncoherent_caches_for_page
%s: Unexpected write/exec refcount found while retyping an XNU_ROZONE frame (fte = %p)
xnu_rozone_retype_out
xnu_exec_retype_out
sk_types_retype_out
%s: IO PADDR could not be found %p
io_range_get_papt
%s: IO PADDR belongs to Secure Kernel %p
State objects exhausted for IOMMU %s with ID %d
%s: Attempted to register managed page as an IO range %p %zu
iommu_bootstrap_register_io_range
%s: Attempted to register an empty IO range %p %zu
%s: Attempted to register unaligned IO range %p %zu
%s: %#llx length is greater than 64K pages (%zu)
%s: IO frame is not XNU_PROTECTED_IO typed for IO range %p %u
%s: IO frame starting index %llu does not match IO range starting index %llu
%s: IO frame number of pages %llu does not match IO range number of pages %u
%s: pmap-io-ranges does not exist for IO range %p %u
%s: cache mode (%u) does not match with pmap-io-ranges (%u) for IO range %p %u
%s: Number of IO ranges exhausted
iommu_frame_acquire
sptm_iommus.c
iommu_id
%s: IOMMU %s with id %d attempted to release unexpected frame %p
iommu_frame_release
%s: Tried releasing a frame that should not have been acquired to begin with %d
%s: IOMMU %d attempted to increment refcnt of unexpected frame %p
iommu_page_refcnt_update
%s: IOMMU %s with id %d attempted to register illegal mapping
iommu_register_mapping
iommu_unregister_mapping
%s: Error getting SecureDT
sptm_iommu_bootstrap
%s: error %d looking up /chosen
gpu-iouat
%s: gpu-iouat is not %zu bytes wide (%d)
%s: Unable to get root device tree node.
sptm_iommu_reg_base
%s: Could not find /arm-io device tree node!
%s: Overflow while calculating register end address (0x%llx + 0x%llx)
#address-cells
%s: Unable to get expected value for #address-cells property from the root device tree node: prop_value = 0x%llx *prop_value = 0x%x.
#size-cells
%s: Unable to get expected value for #size-cells property from the root device tree node.
%s: Invalid ranges property found.
%s: Overflow while calculating parent range  end address (0x%llx + 0x%llx)
%s: Overflow while calculating translated address (0x%llx + 0x%llx)
%s: Unable to locate matching range for register range [0x%llx, 0x%llx]
IOMMU with id %d not supported
%s: Unexpected Dispatch ID %llu
current_iommu
iommu_validate_instance
instance_id
%s: Frame expected to have been acquired has not been acquired %p %u
iommu_assert_frame_in_flight
iommu_refcnt_add
iommu_refcnt_sub
iommu_update_page_refcount
current_iommu()
IOMMU "%s" does not provide an XNU-facing dispatch table
IOMMU "%s" bootstrap failure
dispatch_table_lookup
dispatch.c
caller_domain
domain_id
dispatch_target
table_id
table_permissions
[SPTM Dispatch] %s: Found illegal dispatch entry point. caller_domain: %d, entry_point: %#llx, dispatch_target: %#llx
invalid state: %d
invalid event type: %d
invalid next state: %d
%s: invalid state transition - no action set for the transition. current_state: %#x, event_type: %d, event_metadata: %#llx
dispatch_state_machine
%s: Invalid hop detected when transitioning XNU->TXM %llu
txm_stack
phys_to_type(txm_stack)
sptm_register_dispatch_table
entry->dispatch_entry_point
genter_dispatch_entry
sptm_register_xnu_exc_return
return_addr
xnu_exc_return_handler
sptm_dispatch
calling_domain(cpu_data)
endpoint_id
vector_type not valid
xnu_el2_exception_vector not set
xnu_exc_return_handler not registered
%s at pc 0x%016llx, lr 0x%016llx
  x0:  0x%016llx x1:  0x%016llx  x2:  0x%016llx  x3:  0x%016llx
  x4:  0x%016llx x5:  0x%016llx  x6:  0x%016llx  x7:  0x%016llx
  x8:  0x%016llx x9:  0x%016llx  x10: 0x%016llx  x11: 0x%016llx
  x12: 0x%016llx x13: 0x%016llx  x14: 0x%016llx  x15: 0x%016llx
  x16: 0x%016llx x17: 0x%016llx  x18: 0x%016llx  x19: 0x%016llx
  x20: 0x%016llx x21: 0x%016llx  x22: 0x%016llx  x23: 0x%016llx
  x24: 0x%016llx x25: 0x%016llx  x26: 0x%016llx  x27: 0x%016llx
  x28: 0x%016llx fp:  0x%016llx  lr:  0x%016llx  sp:  0x%016llx
  pc:  0x%016llx cpsr: 0x%08x         esr: 0x%08x          far: 0x%016llx
[SPTM Dispatch] Invalid GENTER immediate value %u in genter_system_is_panicking (current CPU: %u, panicking CPU: %u).
%s(%s:%d) - %s(%#llx)
sptm_retype
%s(%s:%d) - %s(%#llx), %s(%#llx), %s(%#llx)
calling_domain(sptm_cpu_data_ptr_fast())
current_type_params->owner_domain
%s(%s:%d) - %s(%#llx), %s(%#llx)
fte->type
current_type
new_type
sptm_map_page
page_fte
%s(%s:%d) - %s(%#llx), %s(%#llx), %s(%#llx), %s(%#llx), %s(%#llx)
page_fte->type
new_sprr_index
%s(%s:%d) - %s(%#llx), %s(%#llx), %s(%#llx), %s(%#llx), %s(%#llx), %s(%#llx)
root_pt_fte
root_pt_fte->type
leaf_pt_fte->type
is_user_mapping
prev_pte
page_paddr
prev_sprr_index
sptm_map_table
child_pt_fte
commpage_region_start
commpage_region_size
root_table_flags
%s(%s:%d) - %s(%#llx), %s(%#llx), %s(%#llx), %s(%#llx)
child_pt_paddr
child_pt_fte->type
parent_pt_fte->type
child_pt_fte->cpu_page_table.level
target_level
expected_tte
rw_guard_release_shared: %p
sptm_unmap_table
parent_tte
%s: incompatible page table type 0x%hhx found in page table %p
parent_ttep
%s: preflight or finalize mapping operation function pointers are NULL
sptm_region_op
start_vaddr
cur_mapping
*prev_pte
Page FTE does not match the validity of the previous PTE %#llx %p
%s: cur_mapping_data/new_pte are NULL
unmap_preflight_op
cur_mapping_data->leaf_pt_fte
%s: cur_mapping_data is NULL
unmap_finalize_op
PTEP is somehow NULL
prev_pte is somehow empty
Leaf page table FTE is somehow NULL
%s: cur_mapping_data/new_pte is NULL
update_preflight_op
%s: page_fte is NULL
cur_mapping_data
pte_template
new_perms
current_perms
update_finalize_op
prev_pte is somehow empty in the final pass
sptm_update_region
(root_pt_U).UNSAFE
(start_vaddr_U).UNSAFE
sptm_disjoint_op
sptm_update_disjoint
(paddr_U).UNSAFE
(disjoint_ops_U).UNSAFE
sptm_update_disjoint_multipage
num_entries
cur_entry_idx
num_mappings
multipage_ops_pa
cur_entry
Unexpected shared region state
sptm_nest_region
user_root_fte->cpu_root_table.attr_idx
shared_root_fte->cpu_root_table.attr_idx
shared_region_id
shared_region_start_vaddr
shared_region_end_vaddr
expected_shared_region
(user_root_pt_U).UNSAFE
(shared_root_pt_U).UNSAFE
user_tt_fte->type
sptm_unnest_region
shared_region_size
prev_shared_region_id
user_tte
user root table ASID aliases kernel ASID
sptm_sign_user_pointer
discriminator
sptm_auth_user_pointer
validate_managed_page
sptm_validation.h
validate_frame_type
frame_type
acquire_root_pt
validate_aligned_vaddr
aligned_vaddr
attr_idx
validate_pte
helper_enforce_paddr_mappable
validate_pt_level_intermediate
validate_tte
rc16_try_inc_from
sptm_concurrency.h
%s: refcnt overflow: rc %p old_value %lld value %lld
crt_mapcnt_inc
cpt_mapcnt_inc
rc16_try_dec_from
expected
%s: refcnt underflow: rc %p old_value %lld value %lld
crt_mapcnt_dec
cpt_mapcnt_dec
validate_vaddr_range_leaf
helper_validate_aligned_vaddr_range
page_count
end_vaddr
validate_managed_addr
validate_page_count
sptm_update_papt
acquire_shared_root_pt
shared_root_pt
shared_root_pt_fte
shared_root_pt_fte->type
validate_vaddr_range_twig
Unexpected shared region ID
acquire_user_root_pt
user_root_pt
user_root_pt_fte
user_root_pt_fte->type
rc16_try_inc
old_value
rc->_value
validate_root_config
root_config
sptm.t8110.release
0123456789abcdef0123456789ABCDEF@
randseed
(%Em*-Fm,5Gm.=Hm*Y@
(%Em*-Fm,5Gm.=Hm*Y@
(%Em*-Fm,5Gm.=Hm*Y@
(%Em*-Fm,5Gm.=Hm*Y@
(%Em*-Fm,5Gm.=Hm*Q@

```

### `strings` of `txm.iphoneos.release.bin`
```
attempted to initialize boot-args again
page enforcement failed (%u | %u): (%p | %u) --> %u | 0x%016llX
com.apple.private.cs.debugger
attempted to initialize ASID table again
shared region base range: %p --> %p
com.apple.oah.runtime_arm_internal
com.apple.runtime_arm_internal
%s: association spans outside of code limit
dynamic-codesigning
research.com.apple.license-to-operate
get-task-allow
build variant: %s
Exemption (allowModifiedCode): %u
Exemption (allowUnrestrictedDebugging): %u
Exemption (developerModeEnabled): %u
Exemption (skipTrustEvaluation): %u
Exemption (allowAnySignature): %u
Exemption (allowUnrestrictedLocalSigning): %u
Exemption (allowCTDebuggingRoots): %u
Exemption (enforceCoreTrust): %u
%p: unable to free entitlements index: %s
%s: unable to resolve entitlements index size: %s
%s | %p: unable to build entitlements index: %s
%s | %p: entitlements not accelerated after building index
com.apple.private.enable-swift-playgrounds-validation
com.apple.private.amfi.can-load-cdhash
txm_device_type
unsupported device type computed: %u
resolved device type identity: %u
txm_platform_code_only
platform code only runtime policy: %u
amfi_get_out_of_my_way
cs_enforcement_disable
txm_skip_trust_evaluation
amfi_allow_any_signature
txm_allow_any_signature
txm_cs_enforcement_disable
amfi_unrestrict_task_for_pid
txm_unrestrict_task_for_pid
amfi_unrestricted_local_signing
txm_unrestricted_local_signing
txm_enforce_coretrust
txm_developer_mode
txm_cs_disable
srd_fusing
TXM [Log]:
attempted to initialize logging again
setup logging: %u bytes (%u | %u)
TXM [Log]: error adding log for this slot: %d
com.apple.private.pmap.load-trust-cache
missing trust cache range from device tree
trust cache range is 0 length
failed to load external trust cache module: %u
loaded external trust cache modules: %u
personalized.engineering-root
personalized.trust-cache
personalized.pdi
personalized.ddi
personalized.cryptex-research
personalized.ephemeral-cryptex
global.ota-update-brain
global.install-assistant
global.bootability-brain
cryptex1.boot.os
cryptex1.boot.app
cryptex1.preboot.app
global.pdi
personalized.mobile-asset-brain
cryptex1.safari-downlevel
cryptex1.preboot.os
personalized.supplemental-persistent
personalized.supplemental-ephemeral
cryptex1.generic
cryptex1.generic.supplemental
attempted to initialize boot memory again
system supports DIT feature
txm.iphoneos.release.TrustedExecutionMonitor_Guarded-78
%s: unsupported function called [%p | %p| %d]
_Block_object_assign
%s: unsupported function called [%p | %d]
_Block_object_dispose
fatal abort condition issued from liblibc
trapped due to a sanitizer runtime violation: %d | %p
[code: 0x%08X | %u]
TXM [Panic]:
setup device tree range
unable to find model property in /
RealityDevice
device type from /model: %u
falling back to SDK type for device type
amfi-only-platform-code
unable to find amfi-only-platform-code property in /chosen
invalid length for amfi-only-platform-code property: %u
research-enabled
unable to find research-enabled property in /chosen
invalid length for research-enabled property: %u
debug-enabled
unable to find debug-enabled property in /chosen
invalid length for debug-enabled property: %u
/chosen/memory-map
unable to find /chosen/memory-map in device tree
TrustCache
unable to find TrustCache property in /chosen/memory-map
invalid length for TrustCache property: %u
/product
udid-version
unable to find udid-version property in /product
invalid length for udid-version property: %u
unsupported UDID version: %u
unable to find chip-id property in /chosen
invalid length for chip-id property: %u
unique-chip-id
unable to find unique-chip-id property in /chosen
invalid length for unique-chip-id property: %u
%08X-%016llX
successfully queried the UDID from the device tree
invalid return from snprintf for UDID: %d
TXM [Panic]: fatal error early within bootstrap
Assertion failed: %s, function %s, file %s, line %d.
output != NULL
src/libc/stdio/format.c
__liblibc_format_string
%%lc specifier not supported
%%n specifier not supported
Security assertion failed: %s, function %s, file %s, line %d.
(len) <= (dstlen)
__vsnprintf_chk
src/libc/stdio/vsnprintf_chk.c
__memcpy_chk
src/libc/string/memcpy_chk.c
(smax) <= (obj_size)
__memset_s_chk
src/libc/string/memset_s_chk.c
__strlcpy_chk
src/libc/string/strlcpy_chk.c
(!len && !dstlen) || (dstlen == ~((size_t)0)) || !((uintptr_t)src <= (uintptr_t)dst + dstlen && (uintptr_t)dst <= (uintptr_t)src + len)
__stack_chk_fail
src/libc/sys/sys_stack_chk_fail.c
beta-reports-active
/private/var/containers/Bundle/
LOCALSPKEY
com.apple.private.oop-jit.loader
task_for_pid-allow
com.apple.system-task-ports
com.apple.system-task-ports.control
com.apple.system-task-ports.token.control
com.apple.private.amfi.can-execute-cdhash
com.apple.developer.swift-playgrounds-app.development-build
com.apple.private.oop-jit.runner
com.apple.security.get-task-allow
previews
ml-compiler
not implemented
alloc ws
Bad tailq head %p first->prev != head @%u
preboot splat manifest hash
payload digest
allow mix-n-match
unique chip identifier
research mode
maximum restore version
cstring [version]
trusted execution monitor
trusted execution monitor dtless
panic: failed to get chosen node
using shadowed ap nonce for domain: %s
nonce is inaccessible: %s: %d
release type set, rejecting: %s
release type too long: %s
release type set to darwinOS
release_type_p == cf4->cf4_osreleasetype
/Library/Caches/com.apple.xbs/Sources/AppleImage4_txm/src/runtime/txm.c
img4_txm_set_release_type
panic: ap shadow already set: existing = %s, incoming = %s
panic: failed to get SecureDT root
panic: failed to instantiate ap: %d
panic: failed to get mix-n-match policy: %d
previous stage manifest hash is missing; continuing because mix-n-match is permitted
panic: failed to get previous stage manifest hash: %d
panic: failed to get long os version: %d
panic: failed to retrieve research enabled bit: %d
completed runtime initialization for TXM
/chosen/manifest-properties
panic: failed to get manifest properties
panic: failed to read boot nonce hash: secure boot level = %#llx: %d
panic: error not set to valid posix code: %d
panic: unreachable
certificate-production-status
certificate-security-mode
effective-production-status-ap
effective-security-mode-ap
internal-use-only-unit
mix-n-match-prevention-status
engineering-use-only-unit
factory-prerelease-global-trust
chip-epoch
board-id
security-domain
esdm-fuses
boot-manifest-hash
panic: zero-length string read for identifier
set nonce request: %lu
panic: nonce value too large: actual = %u, expected <= %lu
panic: nonce already set: %lu
roll nonce request: %lu
panic: attempted to query supplemental root without CSM data
use-ddi-secure-boot
%s: 0x%x
panic: failed to get device tree node: %s: %d
using ddi secure boot
allow-ecid-mismatch
allow-ecid-mismatch: %#x
crypto-hash-method
panic: failed to get crypto algorithm: %d
crypto method: %s
sha2-384
panic: non-sensical crypto hash method: %s
/chosen/asmb
lp-smb0 = %#x
lp-smb0 not present
panic: failed to read lp-smb0: %d
lp-smb1 = %#x
lp-smb1 not present
panic: failed to read lp-smb1: %d
lp state = %#x
full security
reduced security
permissive security
panic: invalid lp state: %#x
returning secure boot chip: %s
panic: invalid nonce domain index: %llu
panic: attempted to query nonce value without CSM data
board identifier
security domain
cryptex subtype
panic: invalid min/max: min = %u, max = %u
blessed local policy generation
certificate production status
volume group uuid
ap-hosted cryptex1 asset coprocessor
chip identifier
reduced-security arm ap
boot splat manifest hash
ap-hosted cryptex1 boot coprocessor
ap-hosted cryptex1 boot proposal
ap-hosted cryptex1 pre-reboot coprocessor
Img4DecodeInitAsPayload: [%d %s]
panic: unsupported signature oid: len = %lu
using pre-canned decode implementation root
firmware
panic: buffer cannot hold object: object = %s, length = %lu, required = %lu
permissive arm ap
factory pre-release global trust
software ap ff01
software instance omits identifier: %s
software instance cannot contain identifier: id = %s, version = %u, expected >= %u
querying software instance: %s
panic: invalid offset: %lld
querying runtime: %s
runtime query: %d
%s = 0x%x
%s = 0x%llx
failed to get digest: %s: %d
panic: illegal identifier definition
runtime does not have cstring support: name = %s, version = 0x%x, required >= 2: %d
failed to get cstring: %s: %d
failed to get cstr: %s: %d
instantiating chip from family: %s
panic: invalid identifier [bad id]: id = %llu, name = %s
using custom identifier: chip = %s, id = %s
panic: illegal identifier: id = %llu, name = %s, type = %s
chip identifier unsupported: %s
failed to instantiate chip identifier: %s: %d
panic: illegal chip definition: ap has no cryptex1 equivalent
certificate/chip epoch
universal device identifier
panic: invalid iterator: %lu
chip has no associated software instance: chip = %s, id = %s
panic: illegal chip definition: %s
software instance omits identifier: chip = %s, id = %s
evaluating runtime limit: chip = %s, id = %s, type = 0x%llx, software query >= 0x%llx, instance = %d
panic: invalid identifier type for transform: actual = 0x%llx, expected = 0x%llx, xf type = 0x%llx
panic: cannot transform manifest digest
panic: cannot transform manifest cstring
%s[manifest %s chip]: enforcing: 0x%x %s 0x%x
panic: illegal identifier constraint: %llu
%s[manifest %s chip]: enforcing: 0x%llx %s 0x%llx
%s[manifest %s chip]: enforcing: %s %s %s
panic: illegal identifier definition: %s
%s[manifest %s chip]: enforcing: %lu %s %lu: %s %s %s
%s[manifest %s chip]: enforcing: %u %s %u: %s %s %s
extended security domain
cryptex1 generic extension
cryptex type
engineering use-only unit
software ap ff06
anti-replay token
manifest
ap extension
vma2 ap clone
_Img4DecodeInitAsManifest: [%d %s]
unwired firmware; passing NULL image
calling out to execution context: %d
initializing chip state
unwrapped
authenticating %sfirmware on chip: 4cc = %s, chip = %s
evaluating manifest on chip: %s
chip allows mix-n-match; ignoring previous stage manifest hash
chip disallows mix-n-match
new chain of trust; will enforce manifest directly
wrapped Image4 payload
firmware authenticated
computeDigest: [%d %s]
panic: unsupported digest length: %lu
Img4DecodeCopyPayloadDigest: [%d %s]
Img4DecodePerformTrustEvaluationWithCallbacks: 4cc = %s, dr = [%d %s]
manifest property: %s
Img4DecodeGetPropertyBoolean: [%d %s]
manifest has mix-n-match entitlement
using caller provided nonce
Img4DecodeGetPropertyInteger: [%d %s]
invalid nonce domain: 0x%x
failed to copy nonce: %s: %d
Img4DecodeGetPropertyData: [%d %s]
manifest has version constraint
chip does not use vnum anti-replay policy
unknown manifest tag: %s
panic: illegal property definition: %s
returning DERReturn: %d
panic: cannot initialize nonce from buffer: len = %lu, max = %lu
os version
failed to get love: %d
manifest is valid for os version
%s constraint violated: %s: %d
%s constraint satisfied: %s
panic: cannot initialize digest from buffer: len = %lu, max = %lu
chained manifest
error querying previous stage manifest hash: %d
manifest is consistent with original boot chain
manifest transform failed: id = %s: %d
failed to get identifier: chip = %s, id = %s, type = %s: %d
Img4DecodeGetPropertyInteger64: [%d %s]
invalid version string: %s: %d
will fetch composite boolean: %s
failed to get boolean: %s: %d
composite boolean: identifier = %s, val = %u
eieiuou = %u
object property: %s
unknown object tag: %s
trust evaluation results: dr = %d, ct = 0x%x, error = %d
returning property callback error: %d
trust evaluation succeeded
object not present in manifest or certificate constraints violated
trust evaluation failed: dr = %d, ct = 0x%x
Img4DecodePerformManifestTrustEvaluationWithCallbacks: dr = [%d %s]
attached Image4 manifest
detached Image4 manifest
delivering raw payload for execution
Img4DecodeGetPayload: [%d %s]
delivering unwrapped payload for execution
chip: CPRO = 0x%x, CSEC = 0x%x
chip: EPRO = 0x%x, ESEC = 0x%x
chip: AMNM = 0x%x
chip: iuou = 0x%x
chip: fusing: physical = 0x%llx, effective = 0x%llx
setting nonce
failed to init chip state: %s: %d
subsequent stage
querying previous stage manifest hash
Img4DecodeCopyManifestDigest: [%d %s]
manifest consistent with previous stage
first stage
caller specified no nonce; failing
failed to initialize decode: %d
manifest-only evaluation; allowing mix-n-match
manifest has no nonce; allowing mix-n-match
first-stage verification; forcing AMNM recognition
nonce shadows AP; using AP mix-n-match policy
subsequent-stage verification; mix-n-match allowed
chip allows mix-n-match; not enforcing os version
allowing cross-group os version: fusing = 0x%llx
allowing cross-group os version: effective fusing = 0x%llx
allowing cross-group os version: internal use-only unit
overriding mix-n-match policy: old = %s, new = %s
manifest and chip allow mix-n-match; relaxing CHMH
manifest authenticated
os version x-group
manifest and chip version groups not comparable; overriding enforcement decision
enforcing manifest chain policy: %s
enforcing mix-n-match policy: %s
vma2-hosted cryptex1 boot coprocessor
vma2 clone-hosted cryptex1 boot coprocessor
vma2-hosted cryptex1 boot proposal
vma2 clone-hosted cryptex1 boot proposal
vma2-hosted cryptex1 pre-reboot coprocessor
reduced-security cryptex1 boot coprocessor
reduced-security cryptex1 boot proposal
reduced-security cryptex1 pre-reboot coprocessor
software ap ff00
sha1 arm ap
sha2-384 arm ap
effective production status
internal use-only unit
product class
com.apple.private.img4.nonce.test
trust cache
com.apple.private.img4.nonce.trust-cache
personalized disk image
com.apple.private.img4.nonce.pdi
com.apple.private.img4.nonce.cryptex
developer disk image
com.apple.private.img4.nonce.ddi
ephemeral cryptex
com.apple.private.img4.nonce.ephemeral-cryptex
cryptex1 coprocessor snuf stub
com.apple.private.img4.nonce.cryptex1.snuf-stub
cryptex1 coprocessor boot
com.apple.private.img4.nonce.cryptex1.boot
cryptex1 coprocessor asset
com.apple.private.img4.nonce.cryptex1.asset
cryptex1 coprocessor supplemental content
com.apple.private.img4.nonce.cryptex1.supplemental
cryptex1 coprocessor simulator
com.apple.private.img4.nonce.cryptex1.simulator
panic: invalid nonce domain: %llu
previous stage manifest hash
certificate security mode
cryptex chip identifier
effective security mode
chained manifest hash
0123456789abcdef
panic: bogus digest length: %lu
end of sequence or set
unexpected tag found while decoding
misc. decoding error (badly formatted DER)
function not implemented in this configuration
incomplete sequence
incoming parameter error
buffer overflow
DeviceTree overflow: %p, size %#zx
Device tree pointer outside of device tree region: pointer %p, region [0x%lx, 0x%lx]
Device tree property overflow: prop %p, length 0x%x
der_vm_execute_nocopy
unhandled opcode
der_vm_iterate_b
iterable decoding failure
iteration over a non-iterable type
encountered invalid element in an iterable
der_vm_integer_from_context
Attempting to select an integer value from a non-integer DER object
der_vm_string_from_context
Attempting to select a string value from a non-string DER object
der_vm_bool_from_context
Attempting to select a boolean value from a non-boolean DER object
der_vm_buffer_from_context
Failed during buffer decoding
der_vm_execute_select_index
der_vm_execute_select_key
key length is invalid
invalid dictionary element
der_vm_execute_match_string
string decode failure
der_vm_execute_match_bool
bool decode failure
der_vm_execute_match_integer
der_vm_execute_match_string_prefix
der_vm_execute_string_value_allowed
Unexpected type to match against
der_vm_execute_string_prefix_value_allowed
der_vm_execute_select_longest_matching_key
der_vm_execute_integer_value_allowed
string_value_allowed_iterate
Unexpected type to match against during iteration
string_prefix_allowed_iterate
integer_allowed_iterate
der_decode_next
could not decode tag for next DER sub-sequence
could not decode size for next DER sub-sequence
sub-sequence size is larger than sequence size
der_decode_number
unknown number encoding
number too large
der_decode_boolean
Unknown boolean encoding
boolean should be exactly 1 byte
der_decode_string
Unknown string encoding
der_decode_data
Unknown data encoding
der_decode_key_value
key / value decoding failure
dictionary key is not a valid string
unable to decode value tag for key-value pair
unable to decode value size for key-value pair
key-value pair contains extra elements
recursivelyValidateEntitlements
recursion limit reached
invalid tag or length
String contains an embedded NUL
data element not allowed
encountered invalid tag
extra data at the end of element
der_validate_dictionary
dictionary decoding failure
dictionary key / value decoding failure
key contains an embedded NUL
dictionary keys are not sorted or not unique
dictionary value verification failure
der_validate_array
array decoding failure
encountered invalid element in an array
Not a CoreEntitlements error!
API Misuse
Invalid Arguments
A memory allocation has failed
Verification failed
Query cannot be satisfied
Not eligible for acceleration
CEValidateWithOptions
invalid arguments passed in
xml-looking blob was passed in
validate_VNext
entitlements blob does not have CCDER_ENTITLEMENTS coding
[%s]: entitlements blob has unexpected version %lld
entitlements blob does not have CCDER_ENTITLEMENTS_DICT as the second element
validate_V0
entitlements blob does not have CCDER_CONSTRUCTED_SET coding
[%s]: subset validation failed, key not found in the superset '%.*s'
valuesAreAllowed_block_invoke
[%s]: subset validation failed, key %.*s with value '%.*s' not allowed
[%s]: subset validation failed, bool value of key '%.*s' not allowed
[%s]: subset validation failed, integer value of key '%.*s' not allowed
[%s]: subset validation failed, string array of key '%.*s' not allowed
[%s]: subset validation failed, type mismatch for key '%.*s' %lu != %lu
B16@?0^{?={der_vm_context=^{CERuntime}{CEAccelerationContext=^{CEAccelerationElement}Q}QBB(?={ccder_read_blob=**}{?=**})}{der_vm_context=^{CERuntime}{CEAccelerationContext=^{CEAccelerationElement}Q}QBB(?={ccder_read_blob=**}{?=**})}II^v}8
[%s]: %s
CEIndexSizeForContext
index size overflow
CEBuildIndexForContext
runtime doesn't support acceleration
copy_keys_to_accelerator_block_invoke
fatal error when decoding key/value pair
application-identifier

LOCALSPKEY.swift-playgrounds-*0?
                                )com.apple.developer.app-management-domain

swift-playgrounds*0.
                    )com.apple.developer.swift-playgrounds-app
0@
  ;com.apple.developer.swift-playgrounds-app.development-build

*01
   #com.apple.developer.team-identifier

LOCALSPKEY0123456789abcdef0123456789ABCDEF
AppleInternalProfile
ProvisionsAllDevices
Entitlements
ProvisionedDevices
TeamIdentifier
DeveloperCertificates









Apple Inc.1&0$
Apple Certification Authority1
Apple Root CA0
060425214036Z
350209214036Z0b1
Apple Inc.1&0$
Apple Certification Authority1
Apple Root CA0
https://www.apple.com/appleca/0
Reliance on this certificate by any party assumes acceptance of the then applicable standard terms and conditions of use, certificate policy and certification practice statements.0
)Apple Secure Boot Certification Authority

'Apple Extra Content Global Root CA - G11
Apple Inc.1
211117190012Z
461114000000Z0T100.

'Apple Extra Content Global Root CA - G11
Apple Inc.1
myX0_Ny="
Apple Secure Boot Root CA - G21
Apple Inc.1
141219201310Z
341214201310Z0K1'0%
Apple Secure Boot Root CA - G21
Apple Inc.1
Basic Attestation User Root CA1
Apple Inc.1
California0
170419214156Z
320322000000Z0S1'0%
Basic Attestation User Root CA1
Apple Inc.1
California0v0

"Apple DDI Secure Boot Root CA - G11
Apple Inc.1
201118193306Z
451115000000Z0O1+0)

"Apple DDI Secure Boot Root CA - G11
Apple Inc.1

"Apple X86 Secure Boot Root CA - G11
Apple Inc.1
170322214243Z
170323214243Z0O1+0)

"Apple X86 Secure Boot Root CA - G11
Apple Inc.1
Apple iPhone Certification Authority
Apple Code Signing Certification Authority
Apple Accessories Certification Authority -
Apple Accessory Host Attestation Authority -
Apple Accessories Provisioning Authority -
0.0.0.0.0,1000
txm.iphoneos.release

```
