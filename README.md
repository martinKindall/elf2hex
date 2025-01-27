# Generating .HEX file from RISC-V .elf files with elf2hex

    ./elf2hex [-h] --bit-width BIT_WIDTH --input INPUT.ELF [--output OUTPUT.HEX]

SiFive's Verilog test harnesses can't directly read ELF binaries but are
instead required to be provided with a hexidecimal dump of a particular
width and depth.  This project allows users to easily create these
files.

## Building `elf2hex` from a release

The best way to build `elf2hex` is from the [latest release's
tarball](https://github.com/sifive/elf2hex/releases/download/v1.0.1/elf2hex-1.0.1.tar.gz),
as these releases have been tested.  `elf2hex` uses the standard GNU
build flow:

   tar -xvzpf elf2hex-1.0.1.tar.gz
   cd elf2hex-1.0.1
    ./configure --target=riscv64-unknown-elf
    make
    make install

Building from the tarballs requires Python 3.5 (or newer) as well as a C
compiler, as well as a RISC-V toolchain.

## Building `elf2hex` from git

The latest source for `elf2hex` can be found on [SiFive's
GitHub](https://github.com/sifive/elf2hex).  While the master branch is
always meant to be stable, there are no guarantees.  To build from
git sources you must regenerate the build scripts from their sources and
then follow the standard build flow

    git clone git://github.com/sifive/elf2hex.git
    cd elf2hex
    autoreconf -i
    ./configure --target=riscv64-unknown-elf
    make
    make install

# Converting Assembler code to .HEX files

## Requirements

- RISC-V Toolchain (32 or 64 bits, in this example I will use the 32 version)
- Here's a [guide how to easily build](https://github.com/johnwinans/riscv-toolchain-install-guide) the 32-bits version with the ISA of your choice

## Example

```bash
riscv32-unknown-elf-as exampleProg.S -o exampleProg.o
riscv32-unknown-elf-ld -o exampleProg.elf -T bram.ld -m elf32lriscv -nostdlib --no-relax
riscv32-unknown-elf-elf2hex --bit-width 32 --input exampleProg.elf --output exampleProg.hex
```
