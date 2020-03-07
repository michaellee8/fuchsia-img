# Fuchsia image build
20200306
by michaellee8

## Migrated to GitLab

Migrated to GitLab since GitHub is unhappy about the LFS storage 😓.

https://gitlab.com/michaellee8/fuchsia-build-image

## Instructions

1. Make sure your device have Vulkan suppport.
2. I assume you clone this repo to ~/Downloads, change the command accordingly if you isn't.
3. You need to configure tuntap per boot with `sudo ip tuntap add dev qemu mode tap user $USER` and `sudo ip link set qemu up`.
4. Run the following command.

```sh
~/Downloads/fuchsia-img/aemu/linux-x64/emulator -feature VirtioInput,RefCountPipe,KVM,GLDirectMem,Vulkan -window-size 1280x800 -gpu host -fuchsia -kernel ~/Downloads/fuchsia-img/multiboot.bin -initrd ~/Downloads/fuchsia-img/fuchsia-ssh.zbi -m 2048 -serial stdio -vga none -device virtio-keyboard-pci -device virtio_input_multi_touch_pci_1 -smp 4,threads=2 -machine q35 -device isa-debug-exit,iobase=0xf4,iosize=0x04 -enable-kvm -cpu host,migratable=no,+invtsc -netdev type=tap,ifname=qemu,script=no,downscript=no,id=net0 -device e1000,netdev=net0,mac=52:54:00:63:5e:7a -drive file=~/Downloads/fuchsia-img/fvm.blk,format=raw,if=none,id=vdisk -device virtio-blk-pci,drive=vdisk -append 'TERM=xterm-256color kernel.serial=legacy kernel.entropy-mixin=e930a069b790f73cb04fb0a1b49846cace094e1c77d839860ccbc875a37efbc5 kernel.halt-on-panic=true '
```

## Notes

Currently this build is somehow broken, only the three default packages can be displayed (flutter give error when try to search for other packages), and `simple_browser` seems not working right now. Also the very nice UI shell in `topaz` has been removed as well, and replaced by a very minimalistic rust-based shell. Will try to investigate it later.

Also it seems that to access the full set of packages, one must provide the package repository through the source with `fx serve`. Currently it does not work. I may try to expose an instance of it through a cheap cloud VM later so one may do a local port forwarding to access it.

It is Google Fuchsia team who holds the copyright of the source of these images that I compile from, which is licensed under a mix BSD, MIT and Apache 2.0. I assume no copyright over them. Please contact me if there are any copyright infringement and I will immediately remove everything here.
