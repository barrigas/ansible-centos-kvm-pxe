# ansible-centos-kvm-pxe

Ansible roles to deploy CentOS KVM guests with Kickstart & HTTP boot server for PXE.

> This project is tailored for CentOS 6 deployments. Keep in mind that CentOS 6 reached EOL in 2020, and the content at http://mirror.centos.org/centos-6/ has been taken down.

## Usage

#### 1. Install dependencies.

Ensure you have QEMU and Libvirt installed.

```
$ ansible-playbook 00_hypervisor_install.yml -i inventory
```
Check the role task for the full package dependency list.

#### 2. Install CentOS on the VM.

Update ```group_vars``` variables to set the OS specs before running the ```01_guest_install.yml``` playbook. Alternetively, overwrite these on the playbook itself.

```
$ ansible-playbook 01_guest_install.yml -i inventory
```

The OS root user password is currently set to ```root``` (same as the username). Feel free to generate a new hash with the command below, and update the ```guest_root_pwd``` variable accordingly.

```
$ python3 -c 'import crypt; print(crypt.crypt("test", crypt.mksalt(crypt.METHOD_SHA512)))'
```

#### 3. Start the guest VM.

Ensure that the ```guest_name``` variable in ```02_guest_boot.yml``` matches the guest host name installed in the previous step, and run the playbook.

```
$ ansible-playbook 02_guest_boot.yml -i inventoy
```

Once booted, you can now start an SSH session into the deployed host using the IP address set in the ```guest_ip``` variable.

Alternetively, you can access the guest shell with Virtual Machine Manager (VMM).

## Compatibility

This project has been tested and is compatible with the following versions:

- Ansible: 2.15.3
- Python: 3.10.12

