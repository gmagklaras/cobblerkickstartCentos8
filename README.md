Cobbler Kickstart install script
================================
A kickstart script that can reproduce a CentOS 8 distro with basic Cobbler functionality installed.

Info
----
The script kshpxemastercentos8.cfg is a suitably crafted [kickstart](https://en.wikipedia.org/wiki/Kickstart_(Linux)) script that provisions a CentOS 8 distro with all the package deps to serve a [Cobbler](https://en.wikipedia.org/wiki/Cobbler_(software)) installation. The script can easily be adapted. In its current form, it can build a VM/physical server with two NICs, one DHCPed NIC on the management/public network, the second (static IP) is the one to offer provisioning services. User accounts and passwords are included (modify the sections) and you shoukd be able to get a basic Cobbler server up and running that serves the static IP NIC. You can place that script on an HTTP path and if, for example, you use a KVM/QEMU, you could do something like:

```
sudo virt-install \
     --name PXEMASTER \
     --memory=2048 \
     --vcpus=2 \
     --os-type="linux" \
     --location YOUR_ISO_FILE_PATH \
     --disk YOUR_IMAGE_PATH  \
     --network default,model=virtio \
     --network bridge=br0,model=virtio \
     --graphics=none \
     --os-variant="rhel8.3" \
     --console pty,target_type=serial \
     -x 'console=ttyS0,115200n8 serial' \
     -x "ks=http://YOUR_WEB_SERVER_IP_OR_FQDN/kshpxemastercentos8.cfg" \
     -x "ksdevice=enp1s0"
```

where:

-YOUR_ISO_FILE_PATH is the path of the [downloaded ISO of the CentOS distro](http://isoredirect.centos.org/centos/8/isos/x86_64/)
-YOUR_IMAGE_PATH is the path of the your VM image (at least 80 Gigs, Cobbler is a heavy user of /var)
-YOUR_WEB_SERVER_IP_OR_FQDN is the reachable IP or DNS name from the VM/server you will try to provision



Author Information
------------------
Written by [Georgios Magklaras](mailto:georgios@steelcyber.com) for Steelcyber Scientific

