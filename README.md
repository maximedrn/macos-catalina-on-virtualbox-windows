# How to install macOS Catalina 10.15 on VirtualBox under Windows.
_Small tutorial with all the sources that allowed me to install MacOS Catalina on VirtualBox under Windows._

This tutorial explains how to install macOS Catalina on a virtual machine. Customizable resolution and video memory.  
Do not hesitate to open an exit in case of a problem or for any technical question.

## Prerequisites

### VirtualBox

* Go to this [website](https://www.virtualbox.org/wiki/Downloads) and download VirtualBox and the Extension Pack.
* Once downloaded, install VirtualBox and the Extension Pack.

### Required files

All the necessary files except VirtualBox, the Extension Pack and the macOS ISO are available by downloading the repository.

* Download the [ISO of macOS Catalina 10.15](http://www.mediafire.com/file/2mwxpooe0da6z3n/Catalina_10.15.5.iso/file) (8.3GB).
* Download the [VMWare Tools.iso](https://github.com/maximedrn/macos-catalina-on-virtualbox-windows/raw/master/VM%20Tools.iso) file.
* Download the [Boot.vmdk](https://github.com/maximedrn/macos-catalina-on-virtualbox-windows/raw/master/Boot.vmdk) file.


## Activate the Virtualization Technology

The virtualization technology is available for Intel and AMD processors under different names. For Intel, it is named **VT-X** and for AMD, it is named **AMD-V**. This technology must be enabled in order to run macOS Catalina on VirtualBox.

To activate the virtualization technology option, you need to access either the BIOS or the UEFI of your computer. In order to do this, you need to know the key·s required when booting your PC. **It can be one of these keys: `F1`, `F2`, `F10`, `F12`, or `DEL` (may require pressing the `Fn` key)**.

### How to activate Intel's VT-X virtualization technology?

Once on your BIOS or UEFI, you need to find the **Virtualization Technology**, **VT-X** or **VT** option which is usually found in the **Advanced** options. Make sure you set this option to **Enabled**.
  
### How to activate AMD's AMD-V virtualization technology?

Once on your BIOS or UEFI, you need to find the **Secure Virtual Machine Mode**, **AMD-V** or **SVM Mode** option which is usually found in the **Advanced** options. Make sure you set this option to **Enabled**.

### How do I know if virtualization technology is enabled on my computer?

Open the Task Manager and in the **Performance** tab check the **Virtualization** field is enabled.


## Configuration of the virtual machine

* Run VirtualBox.
* Create a new virtual machine. Enter the name you want, select **Mac OS X** for the type and **Mac OS X (64-bit)** or **Mac OS X (32-bit)**, depending on your computer architecture, for the version.
* Select the amount of RAM. The minimum is **4096MB**, but for best performance, I recommend setting it to **8192MB**.
* Create a **VDI** virtual disk with a **fixed size** and a minimum of **80GB**.
* Click on your virtual machine and open the **Settings** options.
  * In **System** ➜ **Motherboard**, set the **Chipset** to **PIIX3**.
  * In **System** ➜ **Processor**, set the number of CPUs to a minimum of **2**.
  * In **Display** ➜ **Screen**, set the **Graphics Controller** to **VMSVGA**.
  * In **USB**, enable the USB controller and check **USB 3.0 (xHCI) Controller**.
  * In **Storage**, click on the disk button with a green `+` and add the macOS Catalina ISO file.
  * In **Storage**, click on the hard disk button with a green `+` and add the `boot.vmdk` file.
* Click **OK** to confirm the changes.
* Open a command prompt as administrator and type these commands:  
  _Make sure to replace the `Virtual Machine` with the name of your virtual machine._

  ```
  cd "C:/Program Files/Oracle/VirtualBox/"
  ./VBoxManage.exe modifyvm "Virtual Machine" --cpuidset 00000001 000106e5 00100800 0098e3fd bfebfbff
  ./VBoxManage.exe setextradata "Virtual Machine" "VBoxInternal/Devices/efi/0/Config/DmiSystemProduct" "iMac11,3"
  ./VBoxManage.exe setextradata "Virtual Machine" "VBoxInternal/Devices/efi/0/Config/DmiSystemVersion" "1.0"
  ./VBoxManage.exe setextradata "Virtual Machine" "VBoxInternal/Devices/efi/0/Config/DmiBoardProduct" "Iloveapple"
  ./VBoxManage.exe setextradata "Virtual Machine" "VBoxInternal/Devices/smc/0/Config/DeviceKey" "ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc"
  ./VBoxManage.exe setextradata "Virtual Machine" VBoxInternal2/EfiGraphicsResolution 1920x1080
  ./VBoxManage.exe modifyvm "Virtual Machine" --vram 256
  ```

  The values in the last two lines can be changed.
  * The first is the resolution. It can be: `1280×720`, `1920×1080`, `2560×1440`, `2048×1080`, `3840×2160`, `5120×2880`.
  * The second is the video memory. It can be a number between 0 and 256. The minimum is 128 and the best is 256.


## Installation of macOS on the virtual machine

* Start the virtual machine.
* After a few lines of white text on a black background, the macOS installation screen for Catalina should appear.
* Select your language.
* Click on **Disk Utility** and format the `VBOX HARDDISK Media`.
* Close the tab and click on **Install macOS**.
* Continue and accept the terms of the software license agreement.
* Select the hard drive and click **Install**.
* **Important step**: during the automatic restart of the virtual machine, stop it.
* Restart the virtual machine and spam the `Esc` key to open the BIOS.
* Move to the **Boot Manager** menu with the arrow keys and select **EFI Internal Shell**.
* A few random yellow, gray and white texts should appear until it gets to `Shell> _`.
* Type `install.nsh` and press `Enter`.
* After a few moments, you will be on the macOS Catalina configuration pages.
* Follow the basic configuration steps and you will be on the macOS Catalina desktop as soon as possible.


## Change resolution and video memory

* Shut down the virtual machine.
* Open the **Settings** options and in **Storage**, eject the macOS Catalina ISO file.  
  Then click on the disk button with a green `+` and add the `VMWare Tools.iso` file.
* Restart the virtual machine.
* Double-click on the VMWare Tools disk and click on **Install VMWare Tools**.
* The extension should be blocked, so open the **Security and Privacy** section in the **System Preferences** and click **Open anyway**.
* Once the installation is complete, restart the virtual machine. Note that the installation may need to be performed twice.
* Open the **Terminal** application and type this command:  
  _Replace the `1920 1080` value with the same value as the screen resolution you set earlier._

  ```
  sudo /Library/Application\ Support/VMware\ Tools/vmware-resolutionSet 1920 1080
  ```
* To automate this command every time you start your system, open the **Automator** application.
* Choose **Application**, open **Utilities** and add **Run Shell Script**.
* Set the Shell to `/bin/bash` and type this command line:  
  _Replace the value `password` with the macOS password for your virtual machine._  
  _Replace the `1920 1080` value with the same value as the screen resolution you set earlier._

  ```
  echo "password" | sudo -S /Library/Application\ Support/VMware\ Tools/vmware-resolutionSet 1920 1080
  ```
* Click on file and save it in **Applications**.
* Open the **System Preferences** and click on **Users & Groups**.
* In **Login Items**, click on the `+` button and add the application.


## Sources

* _French website_ - **Comment installer macOS Catalina 10.15 sur VirtualBox sur Windows?**  
  https://www.tech2tech.fr/comment-installer-macos-catalina-10-15-sur-virtualbox-sur-windows
* **How to change screen resolution of macOS on VirtualBox?**  
  https://techsviewer.com/how-to-change-screen-resolution-of-macos-on-virtualbox
* **How to increase the display memory to 256 MB under macOS Catalina on VirtualBox and change the screen resolution?**  
  https://www.youtube.com/watch?v=gDwFdGUsBOo
