```
root@opennebula:~# mkdir /var/lib/image
root@opennebula:~# cd /var/lib/image
root@opennebula:/var/lib/image# qemu-img create -f raw win.img 15G
root@opennebula:/var/lib/image# chown -R oneadmin:oneadmin /var/lib/image
```

```
root@opennebula:/var/lib/image# vi win-template
### #### ##### ==============================

<domain type='kvm' xmlns:qemu='http://libvirt.org/schemas/domain/qemu/1.0'>
<name>win-2016</name>
<memory>1048576</memory>
<os>
<type arch='x86_64'>hvm</type>
<boot dev='hd'/>
<boot dev='cdrom'/>
</os>
<on_reboot>restart</on_reboot>
<on_crash>restart</on_crash>
<devices>
<emulator>/usr/bin/kvm</emulator>
<disk type='file' device='disk'>
<source file='/var/lib/image/win2016.img'/>
<target dev='hda'/>
<driver name='qemu' type='raw' cache='default'/>
</disk>
<disk type='file' device='cdrom'>
<driver name='qemu' type='raw'/>
<target dev='hdc' bus='ide'/>
<readonly/>
<source file='/var/lib/image/windows2016-standard.iso'/>
<address type='drive' controller='0' bus='1' unit='0'/>
</disk>
<disk type='file' device='cdrom'>
<driver name='qemu' type='raw'/>
<target dev='hdd' bus='ide'/>
<readonly/>
<source file='/var/lib/image/virtio-win.iso'/>
<address type='drive' controller='0' bus='1' unit='0'/>
</disk>
<controller type='ide' index='0'>
<address type='pci' domain='0x0000' bus='0x00' slot='0x01' function='0x1'/>
</controller>
<interface type='network'>
<source network='default'/>
</interface>
<graphics type='vnc' port='5950' listen='0.0.0.0'/>
</devices>
<features>
<acpi/>
</features>
</domain>

### #### ##### ==============================
```

```
root@opennebula:~# virsh
virsh # create /var/lib/image/win-deployment
virsh # list
virsh # vncdisplay <number>
```

```
root@opennebula:/var/lib/image#  qemu-img convert -O qcow2 win.img win.qcow2
```

ساخت VM Image
```
root@opennebula:~# su - oneadmin
oneadmin@opennebula:~$ oneimage create --name "Windows" \
  --path "/var/lib/image/win.qcow2" --driver qcow2 --datastore default
```

ساخت VM Template
```
oneadmin@opennebula:~$ onetemplate create --name "win-template" --cpu 1 --vcpu 1 \
  --memory 512 --arch x86_64 --disk "Windows" --nic "private" --vnc
```

ساخت یک Instance
```
oneadmin@opennebula:~$ onetemplate instantiate "win-template" –name “WindowsVm"
```

منبع
  * [OpenNebula on Ubuntu – Part 3 – Windows VM Creation](https://www.supportsages.com/opennebula-iaas-cloud-on-ubuntu-windows-vm-creation/)