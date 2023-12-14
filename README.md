# OS Bootloaders
## General overview
*  Boot flow of BIOS (mostly exclusively x86[\_64]) and UEFI (platform agnostic: x86[\_64], ARM[64], RISC-V): https://medium.com/@tunacici7/first-stage-loaders-bios-u-efi-iboot1-u-boot-spl-5c0bee7feb15
    * Role of first-stage bootloaders, its specification and implementations
    * Role of second-stage bootloaders
* Boot flow of Apple Silicon
    * SecureROM, iBoot1, iBoot2 (ending at loading a (custom) OS kernel): https://github.com/AsahiLinux/docs/wiki/Introduction-to-Apple-Silicon#boot-flow
* Boot overview custom OS on Apple Silicon (starting by loading custom m1n1 kernel): https://github.com/AsahiLinux/docs/wiki/Open-OS-Ecosystem-on-Apple-Silicon-Macs#boot-overview

## Examples:
* x86 BIOS boot section:
    * https://steve-mu.medium.com/getting-started-writing-assembly-code-f4b64a21f6dc
    * https://www.joe-bergeron.com/posts/Writing%20a%20Tiny%20x86%20Bootloader/
