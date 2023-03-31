# Installing and configuring MicroShift


## Preparing to install MicroShift from an RPM package

When deploying this lab, you have been provided with a RHEL8.7 VM that has all the system requirements and is already subscribed to get the MicroShift RPMs and dependancies. However, you will still have to grab your own installation pull secret from the [Red Hat Hybrid Cloud Console](https://console.redhat.com/openshift/downloads#tool-pull-secret) and save it in a temporary folder (for example *$HOME/openshift-pull-secret*) on the provided VM. 

For complete instructions on how to install MicroShift from scratch on a RHEL system, please refer to the [product documentation](https://access.redhat.com/documentation/en-us/red_hat_build_of_microshift/4.12/html/installing/index).

>All the following commands assume you are logged in to the VM that have been provided to you using the credentials provided in the RHDP confirmation email 

The provided VM should have an extra disk we will use to provide LVM storage for MicroShift. Let's start with installing the required packages for LVM :

    $ sudo dnf -y install lvm2

We can then check the drive got added to our VM :

    $ sudo fdisk -l

Which should get us the following result :

<pre>
Disk /dev/nvme0n1: 200 GiB, 214748364800 bytes, 419430400 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: D209C89E-EA5E-4FBD-B161-B461CCE297E0

Device         Start       End   Sectors  Size Type
/dev/nvme0n1p1  2048      4095      2048    1M BIOS boot
/dev/nvme0n1p2  4096 419430366 419426271  200G Linux filesystem

Disk <b>/dev/nvme1n1</b> : 20 GiB, 21474836480 bytes, 41943040 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
</pre>

Finally, we'll create a physical volume and a volume group named **vmdisk** on this extra drive (**/dev/nvme1n1** in our case) :

    $ sudo pvcreate /dev/nvme1n1
    $ sudo vgcreate vmdisk /dev/nvme1n1

[next](installation.md)