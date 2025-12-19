---
tags:
  - infrastructure/virtualization
  - infrastructure/vm
---

VMware Workstation is another Hypervisor offering from VMware. Previously requiring the purchase of a license, Broadcom, VMware's owner, made it free for personal use. Workstation is considered a type 2 (hosted) Hypervisor and is more feature-rich than alternatives such as VirtualBox because of features such as:

- Being able to organise VMs in folders
- Finer controls over network adapters
- 3D Graphics
- Better performance

Additionally, VMware Workstation integrates well with its other products, such as vSphere and ESXI.

We can retrieve settings and configuration information in  C:\ProgramData\VMware\VMware Workstation. We'll usually find 3 files:

| **settings.ini**    | This file contains configuration settings, such as printers and disks, which are applied globally across different VMs. |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **config.ini**      | Contains specific configurations like Auto-Update, access ports, and others.                                            |
| **vmautostart.xml** | Contains configuration information on how Virtual Machines are supposed to start.                                       |

**Note**: The directory `C:\ProgramData` is hidden by default in Windows OS.

Let's explore the content of **vmautostart.xml**, which is displayed below.  

```xml
<!-- 
 This is a sample configuration file for the Virtual Machine Autostart
 functionality. You may uncomment the below section and specify the path to
 the vmx files of those Virtual Machine which you want to automatically
 power-on when the host machine starts or whenever the "VMware Autostart
 Service" is started.
 -->
<ConfigRoot>
    <AutoStartOrder>
    <!-- e id="0">
          <vmxpath>C:\Users\Administrator\Documents\Virtual Machines\Windows10x64\Windows10x64.vmx</vmxpath>
          <startOrder>0</startOrder>
        </e>
        <e id="1">
          <vmxpath>C:\Users\Administrator\Documents\Virtual Machines\VM2\VM2.vmx</vmxpath>
          <startOrder>0</startOrder>
    </e -->
    </AutoStartOrder>
</ConfigRoot>
```

In the above output, we can observe and reveal relevant information, such as the **vmxpath** filesystem path, where a VMware Workstation virtual machine's files, including .vmx and .vmdk files, are stored. Specific logs for each VM can be found in the path of the virtual machine location, in this case: C:\Users\Administrator\Documents\Virtual Machines\{name of the vm}

## VMware Workstation Logs
We can find the Hypervisor logs in Windows in the following location: `C:\ProgramData\VMware\logs`; the name of the log usually starts with the name of the logging process vmmsi.log, followed by the date and time information, for example, `vmmsi.log_20240822_234016`

## VMware VM Memory Dump
If we want to create a memory dump of a VM on a VMware workstation, we need to use or take a screenshot of the VM we want to dump the memory on, and then we need to use a tool to convert it to a memory dump. A commonly used tool for this is "vmss2core." The following command will allow us to dump the memory of a VM.  

`vmss2core -W {snampshot.vmss} {new name of dump.vmem}`