# Linux kernel DANGER

This project aims to hack the Linux kernel that make usermode code run in Ring0.

# Omoshiroi Code Files

Headers

[arch/x86/include/uapi/asm/processor-flags.h](arch/x86/include/uapi/asm/processor-flags.h) - CPU Flags, like CR0, CR4

[arch/x86/include/asm/segment.h](arch/x86/include/asm/segment.h) - Segment Descriptors Definitions

[arch/x86/include/asm/pgtable_types.h](arch/x86/include/asm/pgtable_types.h) - Page Table Templates

[arch/x86/include/uapi/asm/setup.h](arch/x86/include/uapi/asm/setup.h) - My Hack Functions

[arch/x86/include/asm/ptrace.h](arch/x86/include/asm/ptrace.h) - Usermode/Kernelmode Partterns

Sources

[arch/x86/kernel/cpu/common.c](arch/x86/kernel/cpu/common.c) - Init some CPU Features

[arch/x86/kernel/setup.c](arch/x86/kernel/setup.c) - Early Boot Kernel Setup

[arch/x86/kernel/head_64.S](arch/x86/kernel/head_64.S) - Early CPU Setup

[arch/x86/kernel/head64.c](arch/x86/kernel/head64.c) - Early CPU Setup

[arch/x86/kernel/process_64.c](arch/x86/kernel/process_64.c) - Start Usermode Threads

[arch/x86/entry/entry_64.S](arch/x86/entry/entry_64.S) - idt/syscall/sysret

[arch/x86/mm/fault.c](arch/x86/mm/fault.c) - Page Fault Handler

[fs/exec.c](fs/exec.c) - Start ELF Binaries from Kernel

[kernel/sched/core.c](kernel/sched/core.c) - Scheduler

# Build & Run on Ubuntu 24.04

```
apt update
apt install -y build-essential libncurses-dev bison flex libssl-dev libelf-dev bc dwarves git
cp /boot/config-$(uname -r) .config
make menuconfig
```

Then, disable ```CONFIG_SYSTEM_TRUSTED_KEYS``` and ```BTF```

```
-> Cryptographic API (CRYPTO [=y])
    -> Certificates for signature checking
        -> Provide system-wide ring of trusted keys (SYSTEM_TRUSTED_KEYRING)
            -> Additional X.509 keys for default system keyring (SYSTEM_TRUSTED_KEYS [=])

-> Kernel hacking
    -> Compile-time checks and compiler options
        -> Generate BTF typeinfo (DEBUG_INFO_BTF [=n])
```

Then you can 

```
make localmodconfig
make -j24
make modules_install
make install
update-grub
```

# x64 Hacking Status

- [] CR0 Write Protection Disable
- [x] Hack the user GDT to Ring 0
- [x] Disabled PTI
- [x] Disabled SMEP/SMAP
- [x] Hacked User Segment Descriptors to Ring 0
- [x] Hacked User Page Table Templates to Ring 0
- [x] Disabled Alternatives
- [x] Adjust IST to make sure stack always available