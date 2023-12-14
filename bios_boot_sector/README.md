# Intel-syntax x86 bootloader
* Inspired by https://www.joe-bergeron.com/posts/Writing%20a%20Tiny%20x86%20Bootloader/
* Other examples:
    * https://steve-mu.medium.com/getting-started-writing-assembly-code-f4b64a21f6dc
## Prerequisites
### Host platform
* The platform on which we we compile the boot section
* The asssembler for x86 CPU architectures NASM ("Netwide Assembler") is used, which already has been ported to Apple Silicon and can therefore be used on Apple Silicon Mac host machine
* Install

    ```brew install nasm```

### Execution platform
* The platform on which we run the BIOS that selects the boot section
* The machine emulator and virtualizer QEMU ("Quick Emulator") is used
* Install

    ```brew install qemu```
* Things that did not work: Executing on emulator "bochs"

## Development
### Coding
* To make the tutorial https://www.joe-bergeron.com/posts/Writing%20a%20Tiny%20x86%20Bootloader/ (intel-syntax x86 asssembly) work on a Mac host machine, line 79 has to be adapted from `times 510-(\$-$$) db 0` to `times 510-($-$$) db 0`

### Build (on host platform)
* Run an asssembler for the x86 CPU architecture
    ```
    $ nasm boot_sect.asm -f bin -o boot_sect.bin
    ```
### Execution (on execution platform)
* Let x86 machine emulator QEMU be used as execution platform to run the x86 binary:
    ```
    $ qemu-system-i386 -fda boot_sect.bi
    ```
    * Use boot section as floppy disk: `-fda`
    * Use boot section as hard disk `-hda`
* What goes on under the hood:
    1. Boot section gets selected by BIOS firmware
        * BIOS relies on boot sectors to “detect” a bootable program
        * The BIOS selects a boot device, then copies the first sector from the device (any executable code), into physical memory at memory address `0x7C00`
            * Reference: https://en.wikipedia.org/wiki/Boot_sector#The_IBM_PC_and_compatible_computers
        * Bootstrap loader service: http://vitaly_filatov.tripod.com/ng/asm/asm_001.12.html
        * Since the bootloader extends from 0x7C00 for 512 bytes to 0x7E00, the stack segment, SS, will be 0x7E0
    2. To be extended

