# OS Bootloaders
Since I'm tied to an Apple Silicon host (because I love those machines), knowing how to develop simple OS bootloaders for various architectures was not a too straight forward task for me. Maybe someone else wants to do the same and finds the following documentation helpful.

## General overview
*  Boot flow of BIOS (mostly exclusively x86[\_64]) and UEFI (platform agnostic: x86[\_64], ARM[64], RISC-V): https://medium.com/@tunacici7/first-stage-loaders-bios-u-efi-iboot1-u-boot-spl-5c0bee7feb15
    * Role of first-stage bootloaders, its specification and implementations
    * Role of second-stage bootloaders
* Boot flow of Apple Silicon
    * SecureROM, iBoot1, iBoot2 (ending at loading a (custom) OS kernel): https://github.com/AsahiLinux/docs/wiki/Introduction-to-Apple-Silicon#boot-flow
* Boot overview custom OS on Apple Silicon (starting by loading custom m1n1 kernel): https://github.com/AsahiLinux/docs/wiki/Open-OS-Ecosystem-on-Apple-Silicon-Macs#boot-overview

## This playground
Navigate into corresponding subdirectory for more informations.
