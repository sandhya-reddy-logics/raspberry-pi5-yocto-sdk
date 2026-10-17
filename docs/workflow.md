# Raspberry Pi 5 Yocto SDK Workflow

## 1. Project Objective

The objective of this project is to build a Yocto Linux image for Raspberry Pi 5, generate and install a Software Development Kit (SDK), and use the SDK to cross-compile a C application on an x86-64 Linux host for the AArch64 target architecture.

The generated executable is then transferred to the Raspberry Pi and executed to verify the cross-compilation workflow.

## 2. Development Environment

- **Host system:** Linux x86-64
- **Target board:** Raspberry Pi 5
- **Yocto release branch:** Wrynose
- **Yocto machine configuration:** `raspberrypi5`
- **Image recipe:** `core-image-minimal`
- **Target architecture:** AArch64
- **SDK installation directory:** `/home/sandhya/rpi5-sdk`

## 3. Build the Yocto Image

Initialize the Yocto build environment using the setup procedure for the local workspace.

Configure the build for Raspberry Pi 5 by setting the machine configuration to:

```bash
MACHINE = "raspberrypi5"
```

Build the minimal image:

```bash
bitbake core-image-minimal
```

BitBake parses the metadata, resolves recipe dependencies, and builds the packages required for the image.

## 4. Generate the SDK

Generate the SDK for the image:

```bash
bitbake core-image-minimal -c populate_sdk
```

The SDK installer is generated in the Yocto build output under:

```text
tmp/deploy/sdk/
```

The installer provides the cross-compiler, target headers and libraries, and other development tools needed to build applications for the target.

## 5. Install and Initialize the SDK

Run the generated SDK installer and follow its prompts. The SDK used in this project was installed under:

```text
/home/sandhya/rpi5-sdk
```

Initialize the SDK environment in the terminal:

```bash
source /home/sandhya/rpi5-sdk/environment-setup-cortexa76-oe-linux
```

Check the compiler target:

```bash
$CC -dumpmachine
```

Expected output:

```text
aarch64-oe-linux
```

This identifies the target toolchain as an AArch64 cross-compilation toolchain.

## 6. Cross-Compile the C Application

The application source is stored in `src/hello.c` in this repository.

From the project root, after initializing the SDK environment, compile the program:

```bash
$CC src/hello.c -o hello-rpi5
```

Inspect the generated executable:

```bash
file hello-rpi5
```

The executable was verified as a 64-bit ELF binary for the AArch64 architecture.

The generated executable is a build artifact and is not tracked in this Git repository.

## 7. Transfer the Executable to the Raspberry Pi

Replace the example IP address with the current IP address of the Raspberry Pi:

```bash
scp hello-rpi5 root@<RASPBERRY_PI_IP>:/root/
```

This transfers the executable to the target device over SSH using SCP.

The target must be reachable over the network, and the selected account must have permission to write to the destination directory.

## 8. Execute the Application on the Target

Connect to the Raspberry Pi and run:

```bash
chmod +x /root/hello-rpi5
/root/hello-rpi5
```

Expected output:

```text
Hello Sandhya..
```

Successful execution verifies that the application was transferred to the target and ran there.

## 9. Results

The Yocto image was built for Raspberry Pi 5, the SDK was generated and installed on the Linux host, and the SDK cross-compiler was used to create an AArch64 executable.

The executable was transferred to the Raspberry Pi and successfully executed.

## 10. Repository Scope

This repository contains the application source code and documentation for the workflow.

The full Yocto source trees, build output, generated disk images, SDK installer, and installed SDK are kept outside this repository. These artifacts can be recreated using the appropriate Yocto configuration and documented commands.
