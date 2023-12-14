# UEFI application (UEFI-GPT bootloader) for the x86-64 platform
UEFI-GPT is the “new” and, naturally, the ideal way to boot modern programs (like GRUB2). Unlike legacy BIOS, [U]EFI does NOT check for the boot sectors to “detect” a bootable program. Instead it relies on a storage device partition called EFI System Partition (ESP).

* Inspired by https://wiki.osdev.org/UEFI_Bare_Bones

## Prerequisites
### Host platform
* Install llvm (to include lld-link), mtools and automake
    ```
    $ brew install llvm
    $ brew install mtools
    $ brew install automake
    ```
* Setting up the execution platform
    * The platform on which we run the run a UEFI implementation that starts our EFI application
    * The machine emulator and virtualizer QEMU ("Quick Emulator") is used
    * Install
        ```
        $ brew install qemu
        ```
    * Enable UEFI support for virtual machines
        * OVMF contains sample UEFI firmware for QEMU
        * OVMF via https://gist.github.com/haharoit/a81fecd847003626ef9ef700e4901d15#homebrew-tap
            * Use homebrew tab provided by author and
            ```
            $ brew tap uenob/qemu-hvf
            $ brew install ovmf
            ```
            * or build from source https://gist.github.com/haharoit/a81fecd847003626ef9ef700e4901d15#build-ovmf-from-the-source
* Download gnu-efi repository

## Development
### Coding
See https://wiki.osdev.org/UEFI_Bare_Bones
* Follow all steps in subsection _Preparing the files_

### Build (on host platform)
* Compile and link
    ```
    // Compile
    $ clang -target x86_64-unknown-windows  -ffreestanding -fshort-wchar -mno-red-zone -I<path/to>/gnu-efi/inc -I<path/to>/gnu-efi/inc/x86_64 -I<path/to>/gnu-efi/inc/protocol -c -o data.o <path/to>/gnu-efi/lib/data.c

    // Link
    $ clang -target x86_64-unknown-windows -nostdlib -Wl,-entry:efi_main -Wl,-subsystem:efi_application -fuse-ld=lld-link -o BOOTX64.EFI hello.o data.o
    ```
* Creating the FAT filesystem image
    * Ref: https://wiki.osdev.org/UEFI_Bare_Bones
        ```
        dd if=/dev/zero of=fat.img bs=1k count=1440
        mformat -i fat.img -f 1440 ::
        mmd -i fat.img ::/EFI
        mmd -i fat.img ::/EFI/BOOT
        mcopy -i fat.img BOOTX64.EFI ::/EFI/BOOT
        ```

### Execution (on execution platform)
* I did not have much luch with the instructions _Running as a USB stick image_, however _Creating and running the CD image_ worked on my host machine
* Creating the CD image:
    ```
    mkdir iso
    cp fat.img iso
    xorriso -as mkisofs -R -f -e fat.img -no-emul-boot -o cdimage.iso iso
    ```
* Let x86_64 emulator fire up EFI application via the CD image
    ```
    $ qemu-system-x86_64 -pflash /opt/homebrew/Cellar/ovmf/stable202102/share/OVMF/OvmfX64/OVMF_CODE.fd -cdrom cdimage.iso
    ```
