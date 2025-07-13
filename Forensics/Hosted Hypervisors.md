Hosted Hypervisors, also known as Type 2 Hypervisors, are a form of virtualization technology that operates on top of an existing operating system rather than directly interfacing with the underlying hardware. This setup allows them to manage virtual machines within the context of the host OS.  
While Hosted Hypervisors are not as common an area for investigations as their server counterparts, there are situations where investigations are required on either the host containing the Hypervisor or a VM running on the target computer.

We need to know about the internals of the VM
# Internals of Hypervisors
It's types are:
### Type 1 (Bare metal)
These types of Hypervisors have direct access to a system's physical hardware, don’t have to go through another operating system layer (such as Windows or Linux), and are used to run many virtual machines on devices such as servers.
Hypervisors like Hyper-V and VMware ESXi are great examples of type 1 Hypervisors. Additionally, type 1 Hypervisors form the backbone of cloud computing.
### Type 2 (Hosted)
These Hypervisors, also known as hosted Hypervisors, differ from bare metal (type 1) Hypervisors because they run on top of an existing operating system (such as Windows or Linux). Hosted Hypervisors are usually found in small environments, such as developers or end-users machines, where only a small handful of virtual machines are required (such as running Windows on Linux).
Examples of hosted Hypervisors included VirtualBox and VMware Workstation/Player.

Examples:
Hyper-V(type 1), VirtualBox, VMware ESXi, VMware Workstation, QEMU

#### Network Virtualization
There are multiple types of networking with virtual machines. These have been provided in the table below:

| **Type**  | **Description**                                                                                                                                                                                                                                                                                                                                                                                                  |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Bridged   | This type allows for a VM to appear as if it is another device on the host network. For example, the guest VM will obtain an IP address on the same network as the host.                                                                                                                                                                                                                                         |
| NAT       | With this type, all guest network activity appears as if it originates from the host.                                                                                                                                                                                                                                                                                                                            |
| Host-only | Host-only will only allow the guest to be accessible from the host itself.                                                                                                                                                                                                                                                                                                                                       |
| Specific  | The "specific" type allows you to manually assign what vNIC is used by the guest. Hypervisors allow you to create vNICs, where you can specify what subnet and address range is used.<br><br>For example, you can create a vNIC that assigns IP addresses on 10.10.10.0/24, and then configure each guest to use that vNIC so that they will all be placed on 10.10.10.0/24 and can communicate with each other. |
#### Paravirtualisation
Unlike full virtualisation, guest virtual machines using paravirtualisation are aware that they are operating on virtualised hardware. For example, the guest knows that the CPU, RAM, storage and network are virtualised. This allows the guest virtual machine to make direct calls to the Hypervisor.
Generally, paravirtualisation allows for better performance as there is less overhead due to the fact that the hardware isn't fully emulated (as is the case with full virtualisation). However, the guest's operating system needs to support paravirtualisation for this to work, so it is not entirely possible in every case.
Hypervisors such as KVM use a combination of full virtualisation and paravirtualisation to achieve its performance. For example, KVM uses paravirtualisation for I/O (disk read/writes and network activity) for its performance increase whilst using full virtualisation for components such as CPU & RAM for compatibility.
#### Nested Virtualisation
Nested virtualisation allows for virtual machines within virtual machines. For example, running VirtualBox within a guest. This is achieved by using hardware-supported virtualisation such as Intel VT-x or AMD-V. These technologies are features within the CPU that manage virtual machine operations directly rather than using the Hypervisor as the middle-person. 
The use of Intel VT-x and AMD-V improves performance because the instructions are handled on the hardware directly rather than through software. Additionally, technologies such as these provide better security by isolating virtual machines.
However, with that said, nested virtualisation adds complexity because of the additional abstraction layers. Moreover, there is a significant performance overhead, and requires more computing resources to be allocated to the guest.