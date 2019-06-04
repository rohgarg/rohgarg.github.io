---
layout: post
title: Setting up Lustre in a virtual cluster
date: 2018-05-10 14:08
tags: [lustre]
category: Notes
author: Rohan Garg
summary: Notes on building, configuring, and setting up a virtual cluster for Lustre.
---


## Create VM images

### Use Vagrant to create a base image

Create a base VM image, preferably using CentOS-7, or RHEL-7.  Use the
attached Vagrant [file]({{ site.url }}/downloads/Vagrantfile).

    $ mkdir base-image
    $ mv $DOWNLOADS/Vagrantfile base-image
    $ cd base-image
    $ vagrant up
    $ vagrant ssh

### Build and install modified kernel and Lustre RPMs

Use the install scripts from [here]({{ site.url }}/downloads/install-scripts.tar.bz2). Run them in sequence
in the base VM.

    $ cd $HOME

    # Install dependencies
    $ sh 00-install-deps.sh

    # Get Lustre source
    $ git clone git://git.hpdd.intel.com/fs/lustre-release.git

    # Get kernel source
    $ yumdownloader --source  kernel-3.10.0-693.21.1.el7
    $ rpm -ivh kernel-[0-9].*.src.rpm

    # Apply Lustre patches to the kernel source
    $ sh 01-create-patch-files.sh
    $ sh 02-change-kernel-spec.sh
    $ sh 03-fix-kernel-config.sh

    # Build kernel rpms
    $ sh 04-build-kernel-rpm.sh

    # Save kernel rpms
    $ sh 05-save-kernel-rpm.sh

    # Install kernel rpms
    $ cd releases/lustre-kernel/
    $ sudo yum localinstall --nogpgcheck *.rpm

    # Reboot and make sure the kernel boots 
    $ vagrant ssh
    $ sudo reboot
    $ uname -a

    # Build Lustre
    $ cd $HOME/lustre-release
    $ ./configure --with-zfs=no
    $ make

    # Install Lustre.
    $ sudo yum localinstall --nogpgcheck *.rpm
    $ sudo reboot
    $ vagrant ssh

### Clone VM images for different nodes

Clone the base image as many times as necessary, for different machines.
For example, for 1 client, 1 MDS, and 1 OSS, clone it 3 times.

    # Shutdown the base image before cloning to get a consistent state
    $ vagrant halt

    # Package and clone the VM to a different directory
    $ vagrant package
    $ mkdir ../client/
    $ cp package.box ../client/client1.box
    $ cp Vagrantfile ../client/
    $ cd ../client

    # Change the box name to something relevant (e.g.: client01),
    # and enable the box url to point to client1.box
    $ vi Vagrantfile
    $ vagrant up       # This will take a while..
    $ rm client1.box   # Clean up the box; Vagrant keeps its own copy.

## Configure networking

Vagrant doesn't like a non-NAT interface for eth0. Other kinds of networking
options (available in VirtualBox), such as, NAT networking, private, or public
interfaces, will not work. So it's better to leave the first interface (eth0)
untouched, and use the second or subsequent interfaces for Lustre network.

On each VM, edit `/etc/hosts` to add the entries for all other VMs.
For example:

    127.0.0.1    localhost localhost.localdomain localhost4 localhost4.localdomain4
    ::1          localhost localhost.localdomain localhost6 localhost6.localdomain6
    10.0.2.15    client1.lfs.local client1
    192.168.50.5 oss1.lfs.local oss1
    192.168.50.7 client1.lfs.local client1
    192.168.50.9 mds1.lfs.local mds1

Reboot all the VMs. The network configuration should look like this
after the reboot.

    Client: eth0: 10.0.2.15, eth1: 192.168.50.7, hostname: client1
       OSS: eth0: 10.0.2.15, eth1: 192.168.50.5, hostname: oss1
       MDS: eth0: 10.0.2.15, eth1: 192.168.50.9, hostname: mds1

All the VMs should have SELinux disabled.

    $ vi /etc/sysconfig/selinux
    # Edit to change SELINUX=enforcing to SELINUX=disabled.

Verify with `sestatus`.

All the VMs should be able to ping each other and should be able to use
password-less ssh among themselves. Note that this may need to be done
for the root user as well.

For example, on the OSS do:

    $ ssh-keygen             # for the non-root user
    $ ssh-copy-id mds1       # for the non-root user
    $ sudo ssh-keygen        # for the root user
    $ sudo ssh-copy-id mds1  # for the root user

And similarly for other nodes.

All the 4 VM's should have the following line in `/etc/modprobe.d/lnet.conf`:

    options lnet networks="tcp(eth1)"

## Format storage for ldiskfs

We use the `mkfs.lustre` tool to format storage for Lustre's ldiskfs
filesystem.

MGS is the *key* system. All other services -- MDS, OSS, or client --
need to be able to access the MGS. While formatting the OST's and MDS's,
we need to specify the address of the MGS (IP address, or IB-based addressing
scheme).

### Format MDS

On the MDS, execute:

    # mkfs.lustre --reformat --fsname=testfs --mdt --mgsnode=192.168.50.9 --network=tcp --index=0 --mgs /dev/sdb

Note the use of `--mgs` and `--mdt`. We are using the same server as the MDS
and the MGS. The IP address (`--mgsnode`) is the same as that of the MDS node.

The `--index=0` option is to indicate the index of the MDS.

The `--reformat` keyword is to force reformatting of the storage partition.

### Format OSS/OST

On the OSS, execute:

    # mkfs.lustre --reformat --ost --fsname=testfs --mgsnode=192.168.50.9@tcp0 --index=0 /dev/sdb

The `--index=0` option is to indicate the index of the OSS. For the second OSS,
the index would be set to 1, and so on.

The `--reformat` keyword is to force reformatting of the storage partition.

## Mount Lustre FS

### Mount on MDS

    # modprobe lnet
    # mkdir /mnt/mds
    # mount -t lustre -o loop /dev/sdb /mnt/mds/

Verify with `lctl list_nids`, and `lctl dl`.

### Mount on OSS

    # modprobe lnet
    # mkdir /mnt/most
    # mount -t lustre /dev/sdb /mnt/ost

Verify with `lctl list_nids`, and `lctl dl`.

### Mount on Client

    # modprobe lustre
    # mkdir /mnt/client
    # mount -t lustre 192.168.50.9@tcp:/testfs /mnt/client

Verify with `lctl list_nids`, and `lctl dl`.

## References

1. [Lustre Manual](https://build.hpdd.intel.com/job/lustre-manual/lastSuccessfulBuild/artifact/lustre_manual.xhtml)
2. [Lustre HPDD Wiki](https://wiki.hpdd.intel.com/display/PUB/Setting+up+a+Lustre+Test+Environment+on+a+local+system)
3. [Lustre HPDD Wiki 2](https://wiki.hpdd.intel.com/display/PUB/Create+and+Mount+a+Lustre+Filesystem)
4. [Lustre Setup Walkthrough](https://wiki.hpdd.intel.com/pages/viewpage.action?pageId=52104622)
5. [Vagrant Lustre Tutorial](https://github.com/marcindulak/vagrant-lustre-tutorial-centos6)
6. [Quick three-node Lustre Setup](https://gist.github.com/joshuar/4e283308c932ec62fc05)
