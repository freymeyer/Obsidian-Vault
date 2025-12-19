---
tags:
  - infrastructure/virtualization
  - infrastructure/vm
---
Hypervisors are an old concept in computing. In the 1960s, IBM released the first Hypervisor that provided full virtualisation, the CP-40. Since then, the Hypervisor landscape and virtualisation technology have become sophisticated and even form the backbone of services such as cloud computing. We will discuss this in greater detail in further tasks.

These technologies provide many benefits in a modern IT environment, including:

- **Cost saving**: by consolidating what would otherwise be done on multiple machines, money is saved by reducing the physical footprint (i.e. rack space), as well as saving on power and cooling costs by consolidating into one server.
- **High availability**: Hypervisors can be configured to "offload" their workload to another Hypervisor in case of failure. Additionally, Hypervisors typically run on powerful server hardware that often comes with redundancy, such as multiple power supplies and error-checking RAM.
- **Easier management**: workloads running on a Hypervisor are managed from an admin console. For example, you can directly access the virtual machine via SSH and display it through the admin console. This makes managing considerably easier than, say, 50 separate servers.
- **Scalability**: Hypervisors typically run on powerful computing devices. Because the resources available to a virtual machine can be controlled on a granular level, virtual machines can be scaled up and down depending on their usage. Moreover, Hypervisors make importing and exporting virtual machines easy, such as packaging up a virtual machine as an "OVA" (Open Virtual Appliance).

## Type 1 (Bare metal)
These types of Hypervisors have direct access to a system's physical hardware, don’t have to go through another operating system layer (such as Windows or Linux), and are used to run many virtual machines on devices such as servers. Some benefits of type 1 Hypervisors include:

- **Performance:** By having direct access to the hardware, bare metal Hypervisors have full control over the available resources and do not have to contend with the overhead of an operating system.
- **Scalability:** Type 1 Hypervisors often run on powerful servers designed to run a large number of virtual machines. This makes it easy to scale various environments during high workloads. Additionally,  type 1 Hypervisors can be "clustered", meaning workloads can be distributed amongst each other.
- **Sophistication:** These Hypervisor offerings are dedicated to performance and reliability and often interact with multiple services, such as the entire vSphere suite and technologies, such as large data lakes. Moreover, it is possible to include automation with some type 1 Hypervisors.

Hypervisors like Hyper-V and VMware ESXi are great examples of type 1 Hypervisors. Additionally, type 1 Hypervisors form the backbone of cloud computing.

A diagram depicting the structure of a type 1 (bare metal) Hypervisor has been provided below.
![[Pasted image 20251210105558.png]]
### Hyper-V
Microsoft's take on virtualisation is Hyper-V, which comes in several forms. Traditionally, Hyper-V is a full operating system (for example, Hyper-V Server 2019), which is a Type 1 Hypervisor. However, the Hyper-V service can be installed on Windows servers and desktops as a role/feature (such as Pro and Enterprise editions). If installed, the Hyper-V service manages the Windows OS, giving the illusion that it is a type 2 hypervisor, where, in fact, the Windows desktop is managed by Hyper-V from there on.

### VMware ESXi
ESXi is a member of VMware's vSphere suite. This type 1 Hypervisor is a popular choice for large-scale virtualisation. Especially because of features such as:

- **Clustering:** ESXi installations on multiple machines can be installed and connected to one another, allowing workloads to be distributed
- **Automation:** ESXi includes a rich API, allowing the automation of various tasks. For example, automatic snapshotting, VM deployment and management.
- **High availability:** In the event of an error, failover tasks such as virtual machine restart and "offloading" are possible within ESXi.


## Type 2 (Hosted)
These Hypervisors, also known as hosted Hypervisors, differ from bare metal (type 1) Hypervisors because they run on top of an existing operating system (such as Windows or Linux). Hosted Hypervisors are usually found in small environments, such as developers or end-users machines, where only a small handful of virtual machines are required (such as running Windows on Linux).

Examples of hosted Hypervisors included VirtualBox and VMware Workstation/Player. You may recognise or use these to run your hacking machine! The benefits of hosted Hypervisors include:

- **Ease of Use:** These Hypervisors are installed on your device the same way you would any other piece of software. Because they run on top of the existing operating system, you are free to use the device for purposes other than running virtual machines and are typically friendlier to use than type 1 Hypervisors.
- **Free offerings:** Providers such as VirtualBox and, recently, VMware Workstation are free for personal use. This is a great way to get started with Hypervisors. On the contrary, bare metal Hypervisors often involve complex and expensive licensing schemes.
- **Widely compatible:** Most hosted Hypervisor offerings are cross-platform compatible, meaning they can run on Windows, Linux, and Mac.

A diagram depicting the structure of a type 2 (hosted) Hypervisor has been provided below.
![[Pasted image 20251210105630.png]]
### Examples of Hosted Hypervisor
- [[VirtualBox]]
-  [[VMware Workstation]]


#### QEMU
QEMU, also known as Quick EMUlator, is another open-source Hypervisor. This Hypervisor is known for its ability to emulate full systems. For example, QEMU can emulate ARM on an x86_x64 host, making it an ideal platform for emulating different hardware and environments.

Additionally, QEMU can be combined with KVM to have near-native performance by executing instructions directly on the host's CPU.  While QEMU has features of other Hypervisors, such as snapshotting, it can be arguably considered less user-friendly since it is used on the CLI. However, with that said, front-end interfaces have been created that can be used. These two technologies (QEMU & KVM) are often used in cloud environments due to their performance and compatibility.

# Hypervisors in Cyber Security
## Security Research
Virtual machines are popular amongst the cyber security community for numerous reasons such as:
- **Development**: Enthusiasts use virtual machines to develop their tooling or signatures in a practical, safe environment.
- **Analysis**: Security analysts use the benefits of virtual machines to replicate various real-world scenarios by maintaining diverse software and operation systems environments with minimal hardware.
- **Research**: Virtual machines also provide security thanks to their sandboxing mechanism; you can run tooling to investigate malicious code with lesser risk of exposing your physical device. Researchers can take snapshots of a machine in various states that can be reverted back to during experimentation with malicious code. 
- **Testing:** This concept is conjoined with development. Virtual machines are a fantastic way to simulate scenarios and setups. For example, building an Active Directory network to aid in both blue and red teaming techniques for practice.
- **Pentesting / Red Teaming:** Organisations such as Kali and Parrot distribute popular community tooling through their own managed operating systems (Kali Linux, ParrotSecurity, etc.), which are often based on a variant of Debian or Ubuntu. Running these environments in a virtual machine is recommended for security and ease-of-use (I.e. snapshotting).
## Adversaries
Adversaries and APT groups are increasingly focusing their efforts on Hypervisors because they form the backbone of modern computing today. For example, why ransomware one host when you can ransomware a Hypervisor and take down n+1 systems? These types of attacks are emerging in recent trends, such as the [[ALPHAV]] attack on over 50 organisations [ForeScout., 2024], with that said, Hypervisors can be considered a high-priority target for sophisticated threat actors.

Coupled with that, very few endpoint detection services run on Hypervisors due to their nature. This means that there is a lack of oversight in these Hypervisor systems, making them potential targets for an adversary. Organisations such as VMware post their security advisories and best practices. For example, VMware has its advisories portal. 

## Hypervisor Vulnerabilities
Hypervisors, like any system, are not without vulnerabilities. Due to the sensitivity and popularity of Hypervisor platforms (especially type 1 Hypervisors), discovered vulnerabilities can fetch a handsome reward in various bug bounty programs and the underground market.

For example, at the time of writing, Microsoft offers up to $250,000 for Hyper-V vulnerabilities submitted to its [bug bounty](https://www.microsoft.com/en-us/msrc/bounty-hyper-v?rtc=1) program. 

Fortunately, vulnerabilities such as RCE or virtual machine escapes due to the Hypervisor engine itself are rare to come across, but when they happen, it's a big deal. Vulnerabilities in these platforms usually present themselves within the management consoles.

Additionally, these management consoles also share the risk of user misconfiguration, such as weak or no credentials, no lockout mechanism, and other insecure practices.

# Hypervisor Internals
Virtual Machines (VMs), much like any regular computing device, have access to CPU, RAM, storage, and network components. Hypervisors create virtual hardware components and present these to the virtual machines. By virtualising these, each virtual machine thinks it has dedicated components. However, in fact, the virtual machines share the same physical components amongst one another.
## [[Central Processing Unit (CPU)]] & Memory Virtualisation

Hypervisors use virtual CPUs (vCPUs) to virtualise the CPU, which are mapped to the cores available on the physical CPU.

In modern Hypervisors, a vCPU is not permanently assigned to a core on the physical  CPU. Instead, the Hypervisor will take the instructions coming from the VM and spread these across the physical cores as needed.

For example, if you have a CPU with 4 cores and a virtual machine with 1 vCPU, the Hypervisor will provide execution time amongst these physical cores (4). The use of vCPUs allows for better performance whilst tricking the virtual machine into thinking it has a physical CPU. Additionally, this method allows you to assign more vCPUs to the virtual machine without requiring more physical CPU cores.

Generally, the best practice is to limit the number of vCPUs in use to the physical cores available to prevent oversubscribing.
## Storage  Virtualisation
Known as a virtual storage device, a Hypervisor uses virtual disks to create the illusion of a virtual machine having its own physical storage drive. When, in fact, it is sharing the host's physical storage device (such as a hard drive).

Each Hypervisor uses its own format to denote a virtual storage device. For example, VMware uses "VMDK",  whereas VirtualBox uses "VDI".

## Network Virtualisation
Much like CPU, RAM, and storage, Hypervisors abstract the physical networking devices of the host machine to the guest virtual machines. This abstraction is achieved by using Virtual Network Interfaces (vNICs). These vNICs work just like a network adapter and handle data, obtain an IP address and are assigned a MAC address.

Guest virtual machines can have multiple vNICs attached (for example, operating on different subnets simultaneously) while one physical network adapter on the host is used. Additionally, vNICs allow for multiple virtual machines to communicate with one another on the same network.

There are multiple types of networking with virtual machines. These have been provided in the table below:

| **Type**  | **Description**                                                                                                                                                                                                                                                                                                                                                                                                  |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Bridged   | This type allows for a VM to appear as if it is another device on the host network. For example, the guest VM will obtain an IP address on the same network as the host.                                                                                                                                                                                                                                         |
| NAT       | With this type, all guest network activity appears as if it originates from the host.                                                                                                                                                                                                                                                                                                                            |
| Host-only | Host-only will only allow the guest to be accessible from the host itself.                                                                                                                                                                                                                                                                                                                                       |
| Specific  | The "specific" type allows you to manually assign what vNIC is used by the guest. Hypervisors allow you to create vNICs, where you can specify what subnet and address range is used.<br><br>For example, you can create a vNIC that assigns IP addresses on 10.10.10.0/24, and then configure each guest to use that vNIC so that they will all be placed on 10.10.10.0/24 and can communicate with each other. |
## Paravirtualisation
Unlike full virtualisation, guest virtual machines using paravirtualisation are aware that they are operating on virtualised hardware. For example, the guest knows that the CPU, RAM, storage and network are virtualised. This allows the guest virtual machine to make direct calls to the Hypervisor.

Generally, paravirtualisation allows for better performance as there is less overhead due to the fact that the hardware isn't fully emulated (as is the case with full virtualisation). However, the guest's operating system needs to support paravirtualisation for this to work, so it is not entirely possible in every case.

Hypervisors such as KVM use a combination of full virtualisation and paravirtualisation to achieve its performance. For example, KVM uses paravirtualisation for I/O (disk read/writes and network activity) for its performance increase whilst using full virtualisation for components such as CPU & RAM for compatibility.

## Nested Virtualisation
Nested virtualisation allows for virtual machines within virtual machines. For example, running VirtualBox within a guest. This is achieved by using hardware-supported virtualisation such as Intel VT-x or AMD-V. These technologies are features within the CPU that manage virtual machine operations directly rather than using the Hypervisor as the middle-person. 

The use of Intel VT-x and AMD-V improves performance because the instructions are handled on the hardware directly rather than through software. Additionally, technologies such as these provide better security by isolating virtual machines.

However, with that said, nested virtualisation adds complexity because of the additional abstraction layers. Moreover, there is a significant performance overhead, and requires more computing resources to be allocated to the guest.

# Guest Additions
Guest additions are drivers and software that are used to enhance the functionality and performance of a virtual machine. They are installed on the guest machine and provide additional functionality like those in the table below.

| **Feature**          | **Description**                                                                                                                                                                                                                               |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Enhanced performance | Guest additions allow the guest to use optimised drivers, (such as network adapters and disk drives (SATA, SCSI, NVME, etc.)).                                                                                                                |
| Shared folders       | Shared folders allow easy file exchange between the host and guest. This allows both environments to access specific folders that will be mounted within the guest.                                                                           |
| Shared clipboard     | Much like shared folders, a shared clipboard allows the guest and host to access one another's clipboard, allowing files, text, and images to be shared.                                                                                      |
| Improved graphics    | Graphical drivers are installed to allow enhancements such as dynamic resolution. Hypervisors such as VMware allow for 3D acceleration, making better use of the host's graphics card to improve graphics performance.                        |
| USB devices          | Guest additions also allow the guest to access the host's USB devices. For example, you can mount a host's webcam to the guest. Please note, in most cases, you are unable to access the device on the host whilst the guest has it attached. |
Guest additions are generally installed after the VM's OS has been set up. Hypervisors such as VMware workstations will attempt to install these during the VM's provision via the "Easy install" if you use a supported operating system. However, guest additions can be installed at any time.

To do so, you will need to mount the guest additions ISO in the guest's DVD/CD tray. Hypervisors store these ISOs in different locations, but they are generally stored in the same place where the Hypervisor program is installed. Additionally, there are multiple versions of guest additions for the different operating systems.

For VMware Workstation, with Windows as the host, these are stored in  `C:\Program Files (x86)\VMware\VMware Workstation` on a default installation.

## Shared Folders
- the option to map the shared folder as a network drive in the guest enabled.
- can see that the shared folder has been mounted, and the guest can read/write to the respective folder on the host
- specify the shared folder as "read only" when specifying it in the options.
- This is useful when you want the guest to see the files within the host folder but cannot write to it.

## Improved Graphics
- Hypervisors can use guest additions to improve graphical performance. In VMware, 3D acceleration can be enabled.

## USB Devices
- To attach a USB device to a VMware Workstation, left-click on the USB device located at the bottom right of the display pane of the guest VM.

## Guest Perspective
- Once guest additions are installed, services, drivers and processes are added to the guest. The names of these will vary across Hypervisors; however, for VMware, you can see the processes named "VMware Tools" on the guest.

## Vulnerabilities
While guest additions add multiple useful features to a guest VM, they do potentially increase the risk of attack. For example, ransomware can potentially escape onto the host in the case of shared folders, or a data leak can occur.

Additionally, guest additions can serve as an opportunity for attackers to escalate their privileges. As these additions run with elevated privileges due to their nature, a vulnerability within them can serve as a potential method for an attacker to obtain administrative privileges within the guest.

Moreover, one of an attacker's goals is to perform a VM escape. This type of attack involves breaking the virtualisation layer and escaping from the guest environment to the host. For example, in 2018, CVE-2018-2693 was disclosed within the guest additions of VirtualBox. This CVE abused a vulnerability within the guest additions package to escape from the guest to the host.

Whilst these disclosures are rare, they are certainly not impossible. Likewise, who knows what zero-day vulnerabilities threat actors are sitting on and using for their benefit?