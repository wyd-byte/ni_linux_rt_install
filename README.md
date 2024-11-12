# NI Linux RT Installation

## Generate NI Linux RT USB

- Open the NI MAX

  ![Generate NI Linux RT USB](./resources/NIMAX_LinuxRTrecoveryUSB.png)

- Select the image to flash to the USB.

  ![DiskFlashUtility](./resources/NiMaxDiskFormatUtility.png)
  > For using LV 2020, that uses the recovery image 8.0, the maximum allowed intel generation, in DELL computers, is the
  > 10th generation. This recovery version 8.0 is not compatible with newer intel versions, tested **only** in DELL machines.
  > If a DELL computer with higher intel chip is used the LV image for 2023 Q1 must be used (23.0), this is compatible
  > with LV 2020 as well.

## BIOS Config

For installing the recovery image the BIOS must be set accordingly:

- Boot mode set to UEFI
- Disable secure BOOT
- Disable RAID for better compatibility and ensure hard drive detection

  ![BIOS Storage Configuration](./resources/BiosStorageConfiguration.jpeg)

## After using the USB

Install real time packages from the NI MAX.

- Install RT image

  ![Step 1](./resources/MaxStepsAfterUsbUsage_1.png)

- Select programming environment

  ![Step 2](./resources/MaxStepsAfterUsbUsage_2.png)

- Install required packages

  ![Step 3](./resources/MaxStepsAfterUsbUsage_3.png)

- Set SSH password

  ![Step 4](./resources/MaxStepsAfterUsbUsage_4.png)

## After installation to set as PXI

Define the system as a PXI.

- Log into the machine and type the following commands
  - Set as PXI

    ```bash
    grub-editenv - set TargetClass=PXI
    grub-editenv - set DeviceCode=0x77E1
    grub-editenv - set NITarget=true
    ```

  - Define desired device description

    ```bash
    grub-editenv - set DeviceDesc=PXIe-8880_Dell
    ```

  - Define desired host name

    ```bash
    grub-editenv - set hostname=ats_AXES-PXI
    ```

  - Reboot to apply the changes

- Check the settled configuration command

  ```bash
  fw_printenv
  ```

- Example configuration

  ```bash
  admin@NI-cRIO-903x-VM-e50e7166:~# grub-editenv - set TargetClass=PXI
  admin@NI-cRIO-903x-VM-e50e7166:~# grub-editenv - set DeviceCode=0x77E1
  admin@NI-cRIO-903x-VM-e50e7166:~# grub-editenv - set DeviceDesc=PXIe-8880_Dell
  admin@NI-cRIO-903x-VM-e50e7166:~# grub-editenv - set hostname=ats_TMA-PXI
  admin@NI-cRIO-903x-VM-e50e7166:~# grub-editenv - set NITarget=true
  admin@NI-cRIO-903x-VM-e50e7166:~# fw_printenv
  serial#=e50e7166
  DeviceCode=0x77E1
  DeviceDesc=PXIe-8880_Dell
  TargetClass=PXI
  BIOSBootMode=efi
  NITarget=true
  primaryethaddr=CC96E50E7166
  hostname=ats_TMA-PXI
  safemodeenabled=False
  admin@NI-cRIO-903x-VM-e50e7166:~# reboot
  ```

  after the reboot

  ```bash
  admin@ats_TMA-PXI:~# fw_printenv
  serial#=e50e7166
  DeviceCode=0x77E1
  DeviceDesc=PXIe-8880_Dell
  TargetClass=PXI
  BIOSBootMode=efi
  NITarget=true
  primaryethaddr=CC96E50E7166
  hostname=ats_TMA-PXI
  safemodeenabled=False
  ```

## Discover the target from the LV project

After setting the target as a PXI, the new target appears as a PXI target in the project and the NI MAX.

![Graphical user interface, text, application Description automatically generated](./resources/e8ab3760f971c904dd232a18ac306d68.png)

## After installation to set as cRIO

Define the system as a cRIO.

- Log into the machine and type the following commands
  - Set as PXI

    ```bash
    grub-editenv - set TargetClass=cRIO
    grub-editenv - set DeviceCode=0x7735
    ```

  - Define desired device description

    ```bash
    grub-editenv - set DeviceDesc=cRIO-903x-VM
    ```

  - Define desired host name

    ```bash
    grub-editenv - set hostname=name
    ```

  - Reboot to apply the changes

## Installation on a Virtual Box VM

### Creating a Virtual Machine for NI Linux RT

- Choose Linux as Type and Ubuntu (64-bit) as version

  ![Graphical user interface, application Description automatically generated](./resources/13a10f4949efa159508082f2fb011dec.png)

- Choose 1024 or 2048 as your memory size (this can be changed later if needed)

  ![Graphical user interface, text, application Description automatically generated](./resources/2ee6ee07da398799ba65d5d2a833192c.png)

- Choose to create a virtual hard drive (at least 4 GB)

  ![Graphical user interface, text, application Description automatically generated](./resources/7d0f798ec8815c5186a514429d7dc88b.png)

- Choose VDI as your hard disk type

  ![Graphical user interface, text, application, email Description automatically generated](./resources/4e4fc832bb90df31c35e292600d0fbe1.png)

- Choose Dynamically allocated as how the data is stored (both options will work, this takes less space on your host system)

  ![Graphical user interface, text, application, email Description automatically generated](./resources/6319dcbf4760d3f7e8263a8071356599.png)

- Once the virtual machine has been created, copy the image from
  `C:\\Program Files (x86)\\National Instruments\\RT Images\\Utilities\\Linux RT PXI Safemode\\8.0` called
  standard_x64_recovery.iso to your virtualbox workspace (this is strictly not necessary, however I prefer to do this in
  case the image get corrupted during the install process, we have a backup and don't have to reinstall the
  LabVIEW Real Time Module)
  ![Graphical user interface, application Description automatically generated](./resources/14b7d08b3ff697666b284b6dd878f2ea.png)

### Configuring the Virtual Machine

- Select your virtual machine (NILRT in my case) and click on the settings gear
- Go to settings -> system -> Motherboard and check the box "Enable EFI (Special OSes only)

  ![Graphical user interface, text, application, email Description automatically generated](./resources/4d7e535ec0b747d43ddcb9e61be2a2da.png)

- Go to Settings -> Storage and select the optical storage device

  ![Graphical user interface, application Description automatically generated](./resources/c65596c83c8aec6ae8a3440de759f66d.png)

- Click the options menu of the optical drive in the top right corner (little blue "wheel") and select the option "Choose a disk file"

  ![Graphical user interface, text, application Description automatically generated](./resources/b8dc84104a1bb469b0a640dec1c34399.png)

- Navigate to where you have stored your NILRT recovery iso file and select it

  ![Graphical user interface, application Description automatically generated with medium confidence](./resources/f739a9c60e03284fe134399ef9601771.png)
  ![Graphical user interface, application Description automatically generated](./resources/b4df7f41ab65107f543cf6a451a18195.png)

- Go to network and in the adapter 1 tab, select the `Enable Network Adapter` checkbox (usually selected by default)
- Choose `Attached to: Bridged Adapter` and select your main network card as the Name

> If you want to use a bridged connection at CERN, your virtual Ethernet adapter has to be registered in netops,
> if not use a Host-only Adapter connection instead please see
> [**this guide**](https://readthedocs.web.cern.ch/display/MTA/%5BNILRT%5D+Virtual+Box+Host+Only+Adapter) for
> setting up a host only adapter

  ![Graphical user interface, text, application, email Description automatically generated](./resources/403a2d3a4f515402ca3e7f3328387e94.png)

- Expand the advanced menu and select the Intel PRO/1000 MT Desktop as your Adapter type (usually the default one.
  NI ships most of their controllers with Intel based hardware)

  ![Graphical user interface, text, application, email Description automatically generated](./resources/cacb2cfe9a689a43967e40d6eaac6b2b.png)

- Select the settings -> Serial Ports -> Port 1 tab
- Check the box `Enable Serial Port`
  - This is needed for one of the hardware authentication steps in the NILRT installer and will fail if not enabled

  ![Graphical user interface, text, application Description automatically generated](./resources/0391e8f095ee02c6fac358aa35fd4d18.png)

- Select **USB 3.0 as your USB controller** (USB keyboards and other USB based peripherals will not work unless you do this)

  ![Graphical user interface, text, application Description automatically generated](./resources/d63637ca5b7d30ad39054033f9266be4.png)

### Installing NILRT from the ISO

- DoubleClick or hit the `start` arrow to run the machine

  ![Graphical user interface, application Description automatically generated](./resources/bbeec7ed20a3c6f5268d7f123315d53b.png)

- Select the ISO as your startup drive/disk

  ![Graphical user interface, application, Word Description automatically generated](./resources/946aa48c65926b750805fb621de1d763.png)

- choose "y" (yes) when the installer prompts you if you want to continue

  ![Graphical user interface, text Description automatically generated](./resources/03428fc46af881ddf35519247d1f86f4.png)

> If you are using a USB based keyboard, the installer might refuse to take any keyboard inputs at this step. If this
> happens, right click the USB icon in the down right corner of your NILRT virtual machine window and select your keyboard.
> This is only needed during the installation and can be unchecked on the next reboot (once the system is installed)

  ![Graphical user interface, text, application Description automatically generated](./resources/971ec6af1f98b71e542c32a62d84104c.png)

- When the process finishes, it will ask you to restart the machine:

  ![Restart request](./resources/3b74a8b55d0980b2c2dc63109d897054.png)

- After restarting you'll get the following screen:

  ![NI Linux RT welcome screen](./resources/8d58c75a5554dbc51ae894b079a02449.png)

- That's it, the system should now be discoverable from MAX and you can install all drivers from there.

> If the system is discoverable but you're not allowed to install, it might be because the network configuration in
> virtualbox is not correct.

- From NI MAX -> Remote systems -> Your remote device -> Open the device submenu clicking on the arrow ->
  Right click on `Software` -> Click on Add/Remove Software

  ![Graphical user interface, text, application Description automatically generated](./resources/43365f05427d7b23c09a9459e8d5d623.png)

- If the credentials screen opens, the default user is "admin" and leave password empty. Click enter to proceed to the
  following screen

  ![Graphical user interface, text, application, Word, email Description automatically generated](./resources/2e89e61037b1a40bf763007efe2d5877.png)

- Select the OS to install:

  ![Graphical user interface, text, application Description automatically generated](./resources/4d24dc34038547bd096a75b72167b45c.png)

- Wait for this to finish and restart

  ![Graphical user interface, text, application, Word Description automatically generated](./resources/a03fafe9825d3cdc1356760743e755e5.png)
  ![Graphical user interface, text, application Description automatically generated](./resources/0ad646e604d41c7af92084b98ad900d4.png)

- Selected software to install by default:

  - CompactRIO Support.
  - LabVIEW Real-Time.
  - NI Web-Based Configuration and Monitoring
  - NI Wireless Certificate Management Web Service
  - NI-VISA

  ![Graphical user interface, text Description automatically generated with medium confidence](./resources/1e0057a4977f7b31b7264a0c354cc501.png)
  ![Graphical user interface, text, application Description automatically generated](./resources/b6ebdf457912470d75a431b33523f34c.png)
  ![Graphical user interface, text Description automatically generated](./resources/e57049e31e547940c9df2e25fdfeda77.png)
  ![Graphical user interface, text Description automatically generated](./resources/d336cd491612e5069e38bf8028f13fe9.png)
  ![Welcome screen](./resources/e042f4d4aff20159b6cdacd10a63901c.png)
  ![Graphical user interface, text, application, chat or text message Description automatically generated](./resources/7f12f433b77eb67d820b15739d2c5332.png)

- Done, a cRIO system is installed in a VM, to change the system to be a PXI, go to [this section](#after-installation-to-set-as-pxi)
