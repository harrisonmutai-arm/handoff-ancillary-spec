.. _tf_entries:

Entries related to Trusted Firmware
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following entry types are defined for Trusted Firmware projects,
including TF-A, OP-TEE and Hafnium.

.. _tf_entries_summary:
.. list-table:: Summary of Trusted Firmware Entries
   :header-rows: 1

   * - Tag ID
     - Description

   * - :ref:`0x100 <tab_optee_pageable_part_address>`
     - OP-TEE Pageable Part Address

   * - :ref:`0x101 <tab_dt_spmc_manifest>`
     - DT Formatted SPMC Manifest

   * - :ref:`0x102 <tab_entry_point_info>`
     - AArch64 Entry Point Info

   * - :ref:`0x103 <tab_ffa_sp_binary>`
     - FF-A SP Binary

   * - :ref:`0x104 <tab_rw_mem_layout>`
     - RW Memory Layout (64-bit)

   * - :ref:`0x105 <tab_mbedtls_heap_info>`
     - Mbed-TLS Heap Info

   * - :ref:`0x106 <tab_dt_ffa_manifest>`
     - DT Formatted FF-A Manifest

   * - :ref:`0x107 <tab_rw_mem_layout32>`
     - RW Memory Layout (32-bit)

   * - :ref:`0x108 <tab_entry_point_info32>`
     - AArch32 Entry Point Info

   * - :ref:`0x109 <tab_gpt_info>`
     - GPT Error Info

**OP-TEE pageable part address entry layout (XFERLIST_OPTEE_PAGEABLE_PART_ADDR)**

This entry type holds the address of OP-TEE pageable part which is described in
[OPTEECore]_.
This address (of type 'uint64_t') is used when OPTEED (OP-TEE Dispatcher)
is the Secure Payload Dispatcher, indicating where to load the pageable image of
the OP-TEE OS.

.. _tab_optee_pageable_part_address:
.. list-table:: OP-TEE pageable part address type layout
   :widths: 2 2 2 8
   :header-rows: 1

   * - Field
     - Size (bytes)
     - Offset (bytes)
     - Description

   * - tag_id
     - 0x3
     - 0x0
     - The tag_id field must be set to `0x100`.

   * - hdr_size
     - 0x1
     - 0x3
     - |hdr_size_desc|

   * - data_size
     - 0x4
     - 0x4
     - The size (in bytes) of the address of OP-TEE pageable part which must be set to `8`.

   * - pp_addr
     - 0x8
     - hdr_size
     - Holds the address of OP-TEE pageable part

**DT formatted SPMC manifest entry layout (XFERLIST_DT_SPMC_MANIFEST)**

This entry type holds the SPMC (Secure Partition Manager Core) manifest image
which is in DT format [DT]_ and described in [SPMCATTR]_.
This manifest contains the SPMC attribute node consumed by the SPMD
(Secure Partition Manager Dispatcher) at boot time.
It may also contain some information for the SPMC implementation, to
initialize itself.

.. _tab_dt_spmc_manifest:
.. list-table:: DT formatted SPMC manifest type layout
   :widths: 2 2 2 8
   :header-rows: 1

   * - Field
     - Size (bytes)
     - Offset (bytes)
     - Description

   * - tag_id
     - 0x3
     - 0x0
     - The tag_id field must be set to `0x101`.

   * - hdr_size
     - 0x1
     - 0x3
     - |hdr_size_desc|

   * - data_size
     - 0x4
     - 0x4
     - The size of SPMC manifest in bytes.

   * - spmc_man
     - data_size
     - hdr_size
     - Holds a SPMC manifest image in DT format.

.. _64_bit_ep_info:

**AArch64 executable entry point information (XFERLIST_EXEC_EP_INFO64)**

This entry type holds the AArch64 variant of `entry_point_info`.
`entry_point_info` is a TF-A-specific data structure [TF_BL31]_ used to
represent the execution state of an image; that is, the state of general purpose
registers, PC, and SPSR.

This information is used by clients to setup the execution environment of
subsequent images. A concrete example is the execution of a bootloader such as
U-Boot in non-secure mode. In TF-A, the runtime firmware BL31 uses an
`entry_point_info` structure corresponding to the bootloader, to setup the
general and special purpose registers. Following conventions
outlined in [FWH]_ in the Section AArch64 Receiver, the general purpose registers consumed
by the bootloader contain the base addresses of the device tree, and transfer
list; along with the transfer list signature.

In practice, control might be transferred from BL31 to any combination of
software running in Secure, Non-Secure, or Realm modes.

.. _tab_entry_point_info:
.. list-table:: Entry point info type layout
   :widths: 2 5 2 6
   :header-rows: 1

   * - Field
     - Size (bytes)
     - Offset (bytes)
     - Description

   * - tag_id
     - 0x3
     - 0x0
     - The tag_id field must be set to `0x102`.

   * - hdr_size
     - 0x1
     - 0x3
     - |hdr_size_desc|

   * - data_size
     - 0x4
     - 0x4
     - Size of the `entry_point_info` structure in bytes.

   * - ep_info_hdr
     - 0x8
     - hdr_size
     - Header of type :ref:`param_header<tab_param_header>` containing
       information about this structure. The type must be `0x1`, version `0x2`,
       and size `0x58`.

   * - pc
     - 0x8
     - hdr_size + 0x8
     - Program counter (entry point into image).

   * - spsr
     - 0x4
     - hdr_size + 0x10
     - Saved Program Status Register.

   * - x0
     - 0x8
     - hdr_size + 0x18
     - Register X0.

   * - x1
     - 0x8
     - hdr_size + 0x20
     - Register X1.

   * - x2
     - 0x8
     - hdr_size + 0x28
     - Register X2.

   * - x3
     - 0x8
     - hdr_size + 0x30
     - Register X3.

   * - x4
     - 0x8
     - hdr_size + 0x38
     - Register X4.

   * - x5
     - 0x8
     - hdr_size + 0x40
     - Register X5.

   * - x6
     - 0x8
     - hdr_size + 0x48
     - Register X6.

   * - x7
     - 0x8
     - hdr_size + 0x50
     - Register X7.

The structures header contains an attributes field which is used to encode the image's
execution state (i.e., Secure, Non-Secure, or Realm).

.. _tab_param_header:
.. list-table::  Layout of ``param_header``.

   * - Field
     - Size (bytes)
     - Offset (bytes)
     - Description

   * - type
     - 0x1
     - 0x0
     - Type of the structure.

   * - version
     - 0x1
     - 0x1
     - Version of the structure.

   * - size
     - 0x2
     - 0x2
     - Size of the structure in bytes.

   * - attr
     - 0x4
     - 0x4
     - Structure attributes.

**FF-A SP binary (XFERLIST_FFA_SP_BINARY)**

This entry holds a reference to an FF-A Secure Partition (SP) binary.

This TE type is for an SPMC implementation to identify which entry
relates to the SP's binary, such that it can install the binary and
hand-over execution.

.. _tab_ffa_sp_binary:
.. list-table:: An FF-A SP binary type layout
   :widths: 2 2 2 8
   :header-rows: 1

   * - Field
     - Size (bytes)
     - Offset (bytes)
     - Description

   * - tag_id
     - 0x3
     - 0x0
     - The tag_id field must be set to `0x103`.

   * - hdr_size
     - 0x1
     - 0x3
     - |hdr_size_desc|

   * - data_size
     - 0x4
     - 0x4
     - The size of the SP binary in bytes.

   * - ffa_sp_binary
     - data_size
     - hdr_size
     - Holds the FF-A SP binary.

.. _64_bit_mem_layout:

**Read-Write Memory Layout Entry Layout (XFERLIST_RW_MEM_LAYOUT64)**

This entry type holds a structure that describes the layout of a read-write
memory region.

For example, TF-A uses it to convey to BL2 the extent of memory it has available
to perform read-write operations on. BL2 maps the memory described by the layout
into its memory map during platform setup. If other memory types are required
(i.e. read-only memory) separate TEs should be defined.

.. _tab_rw_mem_layout:
.. list-table:: Layout for a RW memory layout entry
   :widths: 2 5 5 6
   :header-rows: 1

   * - Field
     - Size (bytes)
     - Offset (bytes)
     - Description

   * - tag_id
     - 0x3
     - 0x0
     - The tag_id field must be set to `0x104`.

   * - hdr_size
     - 0x1
     - 0x3
     - |hdr_size_desc|

   * - data_size
     - 0x4
     - 0x4
     - The size of the layout in bytes.

   * - addr
     - 0x8
     - hdr_size
     - The base address of the memory region.

   * - size
     - 0x8
     - hdr_size + 0x8
     - The size of the memory region.

**DT formatted FF-A manifest entry layout (XFERLIST_DT_FFA_MANIFEST)**

This entry type holds the FF-A manifest image which is in DT format [DT]_,
as described in [TFAFFAMB]_.
This manifest contains the SP (Secure Partition) configuration, consumed
by the SPMC at boot time.

It may also contain some information to the SP itself.

.. _tab_dt_ffa_manifest:
.. list-table:: DT formatted FF-A manifest type layout
   :widths: 2 2 2 8
   :header-rows: 1

   * - Field
     - Size (bytes)
     - Offset (bytes)
     - Description

   * - tag_id
     - 0x3
     - 0x0
     - The tag_id field must be set to `0x106`.

   * - hdr_size
     - 0x1
     - 0x3
     - |hdr_size_desc|

   * - data_size
     - 0x4
     - 0x4
     - The size of FF-A manifest in bytes.

   * - ffa_manifest
     - data_size
     - hdr_size
     - Holds a FF-A manifest image in DT format.

**Mbed-TLS heap information (XFERLIST_MBEDTLS_HEAP_INFO)**

Specifies the location and size of a memory region, carved out for
stack-based memory allocation in Mbed-TLS. The buffer address and size are
passed to later stages for initialisation of Mbed-TLS.

.. _tab_mbedtls_heap_info:
.. list-table:: Mbed-TLS heap info type layout
   :widths: 4 2 4 8
   :header-rows: 1

   * - Field
     - Size (bytes)
     - Offset (bytes)
     - Description

   * - tag_id
     - 0x3
     - 0x0
     - The tag_id field must be set to `0x105`.

   * - hdr_size
     - 0x1
     - 0x3
     - |hdr_size_desc|

   * - data_size
     - 0x4
     - 0x4
     - This value should be set to `0x10`` i.e. `sizeof(heap_address) + sizeof(heap_size)`.

   * - heap_address
     - 0x8
     - hdr_size
     - The address of memory to be used as the heap.

   * - heap_size
     - 0x8
     - hdr_size + 0x8
     - Size of memory region.

**Read-Write Memory Layout Entry Layout (XFERLIST_RW_MEM_LAYOUT32)**

This entry type holds the 32-bit variant of
:ref:`XFERLIST_RW_MEM_LAYOUT64<64_bit_mem_layout>`. It is a structure used to
describe the layout of a read-write memory region. TF-A utilizes this entry type
to notify BL2 of the available memory for read-write operations. Note, for other
memory types, such as read-only memory, distinct entries should be created.

.. _tab_rw_mem_layout32:
.. list-table:: Layout for a RW memory layout entry (32-bit variant)
   :widths: 2 5 5 6

   * - Field
     - Size (bytes)
     - Offset (bytes)
     - Description

   * - tag_id
     - 0x3
     - 0x0
     - The tag_id field must be set to `0x107`.

   * - hdr_size
     - 0x1
     - 0x3
     - |hdr_size_desc|

   * - data_size
     - 0x4
     - 0x4
     - The size of the layout in bytes.

   * - addr
     - 0x4
     - hdr_size
     - The 32-bit base address of the memory region.

   * - size
     - 0x4
     - hdr_size + 0x4
     - The size of the memory region.

**AArch32 executable entry point information (XFERLIST_EXEC_EP_INFO32)**

This entry type holds the 32-bit variant of the `entry_point_info`
structure.  `entry_point_info` is a TF-A-specific data structure [TF_BL31]_ used
to represent the execution state of an image; that is, the state of general
purpose registers, PC, and SPSR.

This information is used by clients to setup the execution environment of
subsequent images. It's usage is identical to the 64-bit form represented by
:ref:`XFERLIST_EXEC_EP_INFO64<64_bit_ep_info>`.

.. _tab_entry_point_info32:
.. list-table:: Entry point info type layout (32-bit variant)
   :widths: 2 3 3 6

   * - Field
     - Size (bytes)
     - Offset (bytes)
     - Description

   * - tag_id
     - 0x3
     - 0x0
     - The tag_id field must be set to `0x108`.

   * - hdr_size
     - 0x1
     - 0x3
     - |hdr_size_desc|

   * - data_size
     - 0x4
     - 0x4
     - Size of the `entry_point_info` structure in bytes.

   * - ep_info_hdr
     - 0x8
     - hdr_size
     - Header of type :ref:`param_header<tab_param_header>` containing
       information about this structure. The type must be `0x1`, version `0x2`,
       and size `0x24`.

   * - pc
     - 0x4
     - hdr_size + 0x8
     - Program counter (entry point into image).

   * - spsr
     - 0x4
     - hdr_size + 0xc
     - Saved Program Status Register.

   * - lr_svc
     - 0x4
     - hdr_size + 0x10
     - Link register.

   * - r0
     - 0x4
     - hdr_size + 0x14
     - Register R0.

   * - r1
     - 0x4
     - hdr_size + 0x18
     - Register R1.

   * - r2
     - 0x4
     - hdr_size + 0x1c
     - Register R2.

   * - r3
     - 0x4
     - hdr_size + 0x20
     - Register R3.

**GUID Partition Table (GPT) error information (XFERLIST_GPT_ERROR_INFO)**

This entry holds an indication of the type of GPT error.
`gpt_error_info` contains all possible GPT error information.
It is an 8-bit value. Bit 0 indicates that the secondary GPT is being used,
due to the corruption of the primary GPT header.
Bits [7:1] are reserved for future use.

.. _tab_gpt_info:
.. list-table:: GPT info type layout
   :widths: 2 2 2 8
   :header-rows: 1

   * - Field
     - Size (bytes)
     - Offset (bytes)
     - Description

   * - tag_id
     - 0x3
     - 0x0
     - The tag_id field must be set to `0x109`.

   * - hdr_size
     - 0x1
     - 0x3
     - |hdr_size_desc|

   * - data_size
     - 0x1
     - 0x4
     - The size of the GPT error information in bytes, it is always 1 byte.

   * - gpt_error_info
     - data_size
     - hdr_size
     - The `gpt_error_info` field contains GPT error information.

.. |hdr_size_desc| replace:: The size of this entry header in bytes must be set to `8`.
.. |current_version| replace:: `0x1`
