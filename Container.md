---
tags:
  - infrastructure/containers
  - infrastructure/cloud
---

Containers are packages of software that bundles up code, and all its dependencies so it can be run reliably in any environment.

In computing terms, containerisation is the process of packaging an application and the necessary resources (such as libraries and packages) required into one package named a container. The process of packaging applications together makes applications considerably portable and hassle-free to run.

Modern applications are often complex and usually depend on frameworks and libraries being installed on a device before the application can run. These dependencies can:

- Be difficult to install depending on the environment the application is running (some operating systems might not even support them!)
- Create difficulty for developers to diagnose and replicate faults, as it could be a problem with the application's environment - not the application itself!
- Can often conflict with each other. For example, having multiple versions of Python to run different applications is a headache for the user, and an application may work with one version of Python and not another.

Containerisation platforms remove this headache by packaging the dependencies together and “isolating” (**note:** this is not to be confused with "security isolation" in this context) the application’s environment.

If the device supports the containerisation engine, a user will be able to run the application and have the same behaviours.

![[Pasted image 20251207152610.png]]
Applications and their environments (such as dependencies) are packaged together and do not directly interact with the physical computer - but rather the containerisation engine (in this case, it is Docker)
## Namespace
Is the kernel feature in Docker that allows for processes to use resources of the Operating System without being able to interact with other processes. It essentially segregate system resources such as processes, files and memory away from other namespaces.

Every process running on Linux will be assigned two things:  

- A namespace
- A process identifier (PID)

Namespaces are how containerisation is achieved! Processes can only "see" other processes that are in the same namespace - no conflicts in theory. Take Docker, for example, every new container will be running as a new namespace, although the container may be running multiple applications (and in turn, processes).

Let's prove the concept of containerisation by comparing the number of processes there are in a Docker container that is running a web server versus the host operating system at the time:

![an image depicting the large amount of processes running within a normal operating system](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/ca9a3fd100bc4f0f9709d62925678cbc.png)  

Put simply, the process with an ID of 0 is the process that is started when the system boots. Process numbers increment and must be started by another process, so naturally, the next process ID will be #1. This process is the systems `init` , for example, the latest versions of Ubuntu use `systemd`. Any other process that runs will be controlled by `systemd` (process #1).

We can use process #1's namespace on an operating system to escalate our privileges. Whilst containers are designed to use these namespaces to isolate from each other, they can instead coincide with the host computer's processes... This gives us a nice opportunity to escape!

![[Pasted image 20251207153358.png]]