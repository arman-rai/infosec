Hosted Hypervisors, also known as Type 2 Hypervisors, are a form of virtualization technology that operates on top of an existing operating system rather than directly interfacing with the underlying hardware. This setup allows them to manage virtual machines within the context of the host OS.  
While Hosted Hypervisors are not as common an area for investigations as their server counterparts, there are situations where investigations are required on either the host containing the Hypervisor or a VM running on the target computer.

We need to know about the internals of the VM
It's types are:
### Type 1 (Bare metal)
These types of Hypervisors have direct access to a system's physical hardware, don’t have to go through another operating system layer (such as Windows or Linux), and are used to run many virtual machines on devices such as servers.
Hypervisors like Hyper-V and VMware ESXi are great examples of type 1 Hypervisors. Additionally, type 1 Hypervisors form the backbone of cloud computing.
### Type 2 (Hosted)
These Hypervisors, also known as hosted Hypervisors, differ from bare metal (type 1) Hypervisors because they run on top of an existing operating system (such as Windows or Linux). Hosted Hypervisors are usually found in small environments, such as developers or end-users machines, where only a small handful of virtual machines are required (such as running Windows on Linux).
Examples of hosted Hypervisors included VirtualBox and VMware Workstation/Player.

Most used ones: Hyper-V(type 1),  