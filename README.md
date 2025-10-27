# embedded-lab
Repository with embedded experiments on Rust

## Setup

1. Install Rust using [Rustup](https://www.rust-lang.org/tools/install), update if required
1. Add the Cortex-M4 target to the Rust compiler
`$ rustup target add thumbv7em-none-eab`


```
$ # ARM GCC debugger
$ brew install arm-none-eabi-gdb

$ # Minicom and OpenOCD
$ brew install minicom openocd

```


## Verify the board
Connect the discovery board ST-Link (USB mini B) port to the computer USB port and type:

```
$ lsusb | grep -i stm
Bus 001 Device 006: ID 0483:3748 STMicroelectronics ST-LINK/V2
```

Note that the Bus and Device numbers may differ, but the ID should be the same if you have a STM32F4-Discovery board.

# Build the application
Run the command:

`$ cargo build`

Note that this uses the `.cargo\config` file to set the target to `thumbv7em-none-eabi` for the Cortex-M4 processor on the STM32F4-discovery board.

Alternatively run the command:

`cargo build --target thumbv7em-none-eabi`

This build uses the memory map for the STM32F4-Discovery board recorded in the file `memory.x`.

# Flash the application

First open the OCD connection.

## Open OCD Connection

This should produce the following output:
`openocd -f interface/stlink-v2.cfg -f target/stm32f4x.cfg`

```
Open On-Chip Debugger 0.12.0
Licensed under GNU GPL v2
For bug reports, read
        http://openocd.org/doc/doxygen/bugs.html
WARNING: interface/stlink-v2.cfg is deprecated, please switch to interface/stlink.cfg
Info : auto-selecting first available session transport "hla_swd". To override use 'transport select <transport>'.
Info : The selected transport took over low-level target control. The results might differ compared to plain JTAG/SWD
Info : Listening on port 6666 for tcl connections
Info : Listening on port 4444 for telnet connections
Info : clock speed 2000 kHz
Info : STLINK V2J37M26 (API v2) VID:PID 0483:374B
Info : Target voltage: 2.893326
Info : [stm32f4x.cpu] Cortex-M4 r0p1 processor detected
Info : [stm32f4x.cpu] target has 6 breakpoints, 4 watchpoints
Info : starting gdb server for stm32f4x.cpu on 3333
Info : Listening on port 3333 for gdb connections
```

Also, one of the red LEDs, the one closest to the USB port, should start oscillating between red light and green light.

Use control-c to quit the program.

### Connect GDB
With OCD running in another terminal run the following command:

`arm-none-eabi-gdb -q -ex "target remote :3333" target/thumbv7em-none-eabi/debug/embedded-lab`




