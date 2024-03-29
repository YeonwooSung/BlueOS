# Blue OS

https://github.com/davidcallanan/os-series
https://github.com/torvalds/linux

## Prerequisites

- Docker for creating the build-environment.
- Qemu for emulating the operating system.

## Setup

Build an image for our build-environment:

```bash
docker build buildenv -t myos-buildenv
```

## Build

Enter build environment:

    - Linux or MacOS: docker run --rm -it -v "$(pwd)":/root/env myos-buildenv
    - Windows (CMD): docker run --rm -it -v "%cd%":/root/env myos-buildenv
    - Windows (PowerShell): docker run --rm -it -v "${pwd}:/root/env" myos-buildenv
    - Please use the linux command if you are using WSL, msys2 or git bash
    - NOTE: If you are having trouble with an unshared drive, ensure your docker daemon has access to the drive you're development environment is in. For Docker Desktop, this is in "Settings > Shared Drives" or "Settings > Resources > File Sharing".

Build for x86 (other architectures may come in the future):

```bash
make build-x86_64
```

- If you are using Qemu, please close it before running this command to prevent errors.

To leave the build environment, enter exit.

## Emulate

You can emulate your operating system using Qemu: (Don't forget to add qemu to your path!)

- qemu-system-x86_64 -cdrom dist/x86_64/kernel.iso
- Note: Close the emulator when finished, so as to not block writing to kernel.iso for future builds.

If the above command fails, try one of the following:

- Windows: qemu-system-x86_64 -cdrom dist/x86_64/kernel.iso -L "C:\Program Files\qemu"
- Linux: qemu-system-x86_64 -cdrom dist/x86_64/kernel.iso -L /usr/share/qemu/
- Alternatively, install a custom BIOS binary file and link it to Qemu using the -L option.

Alternatively, you should be able to load the operating system on a USB drive and boot into it when you turn on your computer. (I haven't actually tested this yet.)

## Cleanup

Remove the build-evironment image:

```bash
docker rmi myos-buildenv -f
```