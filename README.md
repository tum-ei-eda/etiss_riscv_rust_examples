# etiss_riscv_rust_examples

This repository aims to provide a minimal SDK to build a rust binary to run it with the [ETISS](https://github.com/tum-ei-eda/etiss) instruction set simulator.

## Prerequisites

Install rust compiler and cargo: https://doc.rust-lang.org/cargo/getting-started/installation.html

RISC-V Toolchains can be installed as follows:

```sh
rustup target add riscv32im-unknown-none-elf
```

Compile ETISS: https://github.com/tum-ei-eda/etiss


## Usage

### Compilation

```sh
cargo build --target riscv32imc-unknown-none-elf --example hello --release
```

The generated ELF is located here: `target/riscv32imc-unknown-none-elf/release/examples/hello`

### Simulation

#### Via `run_helper.sh` script

```sh
/path/to/etiss/install/bin/run_helper.sh target/riscv32imc-unknown-none-elf/release/examples/hello -iexamples/hello.ini
```

#### By running `bare_etiss_processor` directly

```sh
/path/to/etiss/install/bin/bare_etiss_processor -iexamples/hello_standalone.ini
```

The INI file uses and relative path to the ELF file, edit `vp.elf_file` in `hello_standalone.ini` to execute etiss from another directory.


### Further information

The memory layout has to be changed via `examples/device.x`. Make sure to also update the memory segments in the ETISS INI files accordingly.

Debugging via ETISS GDBServer: Add `tgdb noattach` to `run_helper.sh` command and connect via `riscv32-unknown-elf-gdb -ex 'tar rem :222' target/riscv32imc-unknown-none-elf/release/examples/hello`

Change etiss config such as `jit.type` and `arch.cpu` via INI file or command line arguments, e.g. `--arch.cpu=RV64IMACFD`.

## Disclaimer

The code in this repository is heavily inspired by https://github.com/rust-embedded/riscv (especially `riscv-semihosting` and `riscv-rt`).
