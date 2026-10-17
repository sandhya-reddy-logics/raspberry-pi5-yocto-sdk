# Raspberry Pi 5 Yocto SDK and Cross-Compilation

## Project Overview

This project demonstrates the generation of a Yocto Software Development Kit (SDK) for Raspberry Pi 5, cross-compilation of a C application on an x86-64 Linux host, and execution of the generated AArch64 executable on the target device.

## Environment

- **Host:** Linux x86-64
- **Target:** Raspberry Pi 5
- **Yocto branch:** Wrynose
- **Machine:** `raspberrypi5`
- **Target architecture:** AArch64
- **SDK installation directory:** `/home/sandhya/rpi5-sdk`

## Workflow

1. Configure the Yocto environment for Raspberry Pi 5.
2. Build the `core-image-minimal` image.
3. Generate the SDK using BitBake.
4. Install and initialize the SDK on the Linux host.
5. Cross-compile a C program for AArch64.
6. Verify the executable architecture.
7. Transfer the executable to the Raspberry Pi using SCP.
8. Execute the program on the target device.

## Generate the SDK

```bash
bitbake core-image-minimal -c populate_sdk
```

## Initialize the SDK

```bash
source /home/sandhya/rpi5-sdk/environment-setup-cortexa76-oe-linux
```

Verify the compiler target:

```bash
$CC -dumpmachine
```

Expected output:

```text
aarch64-oe-linux
```

## Cross-Compile the C Program

```bash
$CC src/hello.c -o hello-rpi5
```

Verify the executable:

```bash
file hello-rpi5
```

The generated executable was verified as a 64-bit ELF binary for the AArch64 architecture.

## Transfer and Execute

Transfer the executable to the Raspberry Pi, substituting its current IP address:

```bash
scp hello-rpi5 root@<RASPBERRY_PI_IP>:/root/
```

On the Raspberry Pi:

```bash
chmod +x /root/hello-rpi5
/root/hello-rpi5
```

Expected output:

```text
Hello Sandhya..
```

## Results

The SDK was generated and installed successfully. The C application was cross-compiled on the Linux host, transferred to the Raspberry Pi, and successfully executed on Raspberry Pi OS.

## Learning Outcomes

- Yocto SDK generation and installation
- Cross-compilation for AArch64
- Compiler target triplets
- ELF executable architecture verification
- Secure file transfer using SCP
- Running cross-compiled applications on a target device

## Repository Contents

- `src/hello.c`: C application source code
- `docs/`: additional project documentation and configuration examples

Build output, generated images, and the installed SDK are excluded from this repository because they can be recreated using the documented workflow.
