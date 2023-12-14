## Intel x86 asssembly language syntax bootloader for BIOS firmware

* Prerequisites: qemu, nasm
* To make the tutorial https://www.joe-bergeron.com/posts/Writing%20a%20Tiny%20x86%20Bootloader/ (intel-syntax x86 asssembly) work on a Mac host machine, line 79 has to be adapted from `times 510-(\$-$$) db 0` to `times 510-($-$$) db 0`
* Things that did not work: Executing on x86 execution platform (x86 emulator) bochs
* Run an asssembler for the x86 CPU architecture: `nasm boot_sect.asm -f bin -o boot_sect.bin`
    * Luckily, nasm runs on a arm64 host machine
* Let x86 machine emulator be used as execution platform to run the x86 binary: `qemu-system-i386 -fda boot_sect.bin`
    * Use boot section as floppy disk: `-fda`
    * Use boot section as hard disk `-hda`

## UEFI bootloader
UEFI-GPT is the “new” and, naturally, the ideal way to boot modern programs (like GRUB2). Unlike legacy BIOS, [U]EFI does NOT check for the boot sectors to “detect” a bootable program. Instead it relies on a storage device partition called EFI System Partition (ESP).

* Download gnu-efi
* Install llvm to include lld-link
* brew install mtools
* brew install automake
* Prepare virtual machine
    * enable UEFI support for Virtual Machines. OVMF contains sample UEFI firmware for QEMU
    * OVMF via https://gist.github.com/haharoit/a81fecd847003626ef9ef700e4901d15

## Compile
clang -target x86_64-unknown-windows  -ffreestanding -fshort-wchar -mno-red-zone -I/Users/robincaloudis/workdir/gnu-efi/inc -I/Users/robincaloudis/workdir/gnu-efi/inc/x86_64 -I/Users/robincaloudis/workdir/gnu-efi/inc/protocol -c -o data.o /Users/robincaloudis/workdir/gnu-efi/lib/data.c

## Link
clang -target x86_64-unknown-windows -nostdlib -Wl,-entry:efi_main -Wl,-subsystem:efi_application -fuse-ld=lld-link -o BOOTX64.EFI hello.o data.o

## Start VM
qemu-system-x86_64 -pflash /opt/homebrew/Cellar/ovmf/stable202102/share/OVMF/OvmfX64/OVMF_CODE.fd -cdrom cdimage.iso
