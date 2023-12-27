# Increase Disk on VM

## Requirements

* Access to host hardware, [proxmox-ve.md](../service-overviews/infrastructure/proxmox-ve.md "mention")

## Process

### Increase the Disk

1. Shutdown the VM
2. Select the hardware tab and select the disk
3. Click on disk action > resize
4. Input how many GB's you want to increase it by

### Boot into GParted to extend the Partition

1. Open the VM console and power it on
2. Interupt the boot and select Utilities
3. Select GParted and wait for it to download
4. Accept the defaults in GParted
5.  Select the disk and click on the resize button\


    <figure><img src="../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>
6. Drag the partition and extend it over the unallocated space and hit ok/save
7. Click on the tick to apply
8. Shutdown the VM

If your VM is Windows based, you're now finished!

### Ubuntu

You'll need to increase the LVM

1. SSH into the server
2. &#x20;???
