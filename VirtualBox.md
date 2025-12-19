---
tags:
  - infrastructure/virtualization
  - infrastructure/vm
---
VirtualBox is an open-source software that enables us to create and manage multiple virtual machines (VMs) on a single physical computer. This is particularly useful for testing, development, and running applications that require different OS environments. VirtualBox provides the flexibility to allocate resources like CPU, memory, and storage to each virtual machine, ensuring that they operate independently and efficiently.

Oracle's VirtualBox is a popular choice among Hypervisors due to its open-source, free, and cross-platform nature. This type 2 Hypervisor is usually found on personal devices such as desktops, where only a few virtual machines are expected to run simultaneously.

VirtualBox has the advantage of being open-source, so if we want to understand better how a Hypervisor works, we can take a look at the actual source code and build it.

One of the tools is `VBoxManage`. We can find it in the default location `C:\Program Files\Oracle\VirtualBox`, and from there, we can find an executable called `VBoxManage.exe`. 

There are several uses for this tool; for example, we could start a VM from the command line with a command similar to the one below.  

`Vboxmanage.exe startvm {name of the vm}`  

We could also enable debug logs if we want more verbosity with the following command.  

`VBoxManage.exe debugvm {name of the vm} log --all`


## VirtualBox Logs
The locations provided in this room are the locations used by VirtualBox by default. Still, they can be different if another path is chosen during installation, so we should always check for logs in other directories if we are unable to find them in their default location.  

First, we can find general information about the Hypervisor in `C:\Users\{username}\.VirtualBox`. This folder contains logs related to the Hypervisor itself and not to a specific VM, as well as some other basic information. Let's review some of the files we can find in this folder:

| **VirtualBox.xml**     | While this is not strictly a log file but rather a configuration file used by VirtualBox, we can get good information from it, like the interface IP used by the Hypervisor, the virtual interfaces used by a VM, uid, log locations, VM files location, etc. We need to be careful not to edit this file since the Hypervisors use it, and it could lead to failure if settings are not properly set. |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **selectorwindow.log** | contains information about the VirtualBox process (usually the details used to install/start).                                                                                                                                                                                                                                                                                                         |
| **VboxSVC**            | saves information regarding the start/stop use of VirtualBox and also saves/deletes components.                                                                                                                                                                                                                                                                                                        |
| **Vbox.log**           | This file contains information such as the OS, with details about their architecture and installation dates (timestamps). It also contains information about the VM plugins and drivers that were implemented (audio, USB, etc.).                                                                                                                                                                      |
| **VBoxHardening.log**  | This file will provide us with information about the hardening performed on VirtualBox; this is focused on the memory management of the Hypervisor. While this information can be hard to read and is not well documented,  if we were looking for some memory manipulation (like an escapeVM attack), we could detect it here. Also, some security-related crashes will be visible in this file.      |
## VirtualBox Memory Dump
 `VBoxManage` to create the image. We can do it by using cmd.exe and navigate to `C:\Program Files\Oracle\VirtualBox`, and execute the `VBoxManage` tool using the following syntax.

`VBoxManage.exe debugvm {name of the vm} dumpvmcore --filename={output file name}`  

After that, we need to use Volatility2; since this feature is not supported on Volatility3, the command to convert the core dump into a memory dump is the following:

`python vol.py -f {output file name} imagecopy -O {new output file name}`  

This will create a memory dump of the machine for us to investigate the internal memory of it.  

**Note**: It's also worth mentioning that you can inspect snapshots on the default directory. `C:\Users\user01\VirtualBox VMs\secretvm\Snapshots`. Also, the Virtual disk used by the VM can be found in the VM directory, and while it is not strictly a Hypervisor investigation, it can be helpful.