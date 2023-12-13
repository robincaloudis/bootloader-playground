## Intel x86 asssembly language syntax bootloader for BIOS firmware

* Prerequisites: qemu, nasm
* To make the tutorial https://www.joe-bergeron.com/posts/Writing%20a%20Tiny%20x86%20Bootloader/ (intel-syntax x86 asssembly) work on a Mac host machine, line 79 has to be adapted from `times 510-(\$-$$) db 0` to `times 510-($-$$) db 0`
* Things that did not work: Executing on x86 execution platform (x86 emulator) bochs
* Run an asssembler for the x86 CPU architecture: `nasm boot_sect.asm -f bin -o boot_sect.bin`
    * Luckily, nasm runs on a arm64 host machine
* Let x86 machine emulator be used as execution platform to run the x86 binary: `qemu-system-i386 -fda boot_sect.bin`
    * Use boot section as floppy disk: `-fda`
    * Use boot section as hard disk `-hda`

