# Provision a CentOS VM on KVM

Create a hypervisor with KVM, QEMU, and libvirt & deploy CentOS guests with PXE and Kickstart.

> This is an old project that has been rescued and tailored for CentOS 6 deployments, using the latest Python and Ansible versions. Keep in mind that CentOS 6 reached EOL in 2020, and its content at http://mirror.centos.org/centos-6/ has been taken down. In the future I may extend this for newer CentOS versions.

## Usage

#### 1. Install dependencies.
 
```
$ ansible-playbook 00_hypervisor_install.yml -i inventory
```

<details>
    <summary>Dependency package details</summary>

* qemu-kvm – Provides hardware emulation.
* libvirt-daemon-system – Configuration files required to run the libvirt daemon.
* libvirt-clients – Client-side libraries and APIs for managing and controlling virtual machines & hypervisors from the command line.
* virtinst – A  set of command-line utilities for provisioning and modifying virtual machines.
* virt-manager – A Qt-based graphical interface for managing virtual machines via the libvirt daemon.
* bridge-utils – A set of tools for creating and managing bridge devices.
* cpu-checker – To check whether your system is cabable of of running hardware accelerated KVM virtual machines (run ```kvm-ok``` from the cmd)
</details>

#### 2. Install CentOS on the VM.

Update ```group_vars``` variables, or overwrite these on the ```01_guest_install.yml``` playbook before running it.

```
$ ansible-playbook 01_guest_install.yml -i inventory
```

The root user password is currently set to ```root``` (same as the user name). Feel free to generate a new hash with the command below, and update the ```guest_root_pwd``` variable.

```
$ python3 -c 'import crypt; print(crypt.crypt("test", crypt.mksalt(crypt.METHOD_SHA512)))'
```

#### 3. Start the guest VM.

Update the ```guest_name``` variable in ```02_guest_boot.yml``` to match the guest host to start, and run the playbook.

```
$ ansible-playbook 02_guest_boot.yml -i inventoy
```

You can now start an SSH session into the newly create guest. Alternitevely, you can access the guest shell with Virtual Machine Manager (VMM).

## Compatibility

This project has been tested and is compatible with the following versions:

- Ansible: 2.15.3
- Python: 3.10.12

