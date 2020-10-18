Why PfSense
===========
On AWS you can easily take back control on the VPN traffic setup using some thing more familare tool such as PfSense with network shaping options and more effective routing. There are a few PfSense community or commercial tool available on the AWS Marketplace, but you may want to build your own AMI for PfSense to support automation or audit purpose.


Table of contents
=================

<!--ts-->
   * [Why PfSense](#why-pfsense)
   * [Table of contents](#table-of-contents)
   * [Why do you need ENA driver](#why-do-you-need-ena-driver)
      * [How to add it](#how-to-add-it)
<!--te-->


Why do you need ENA driver
==========================
ENA (Elastic Network Adapter) is a high performance virtual Ethernet interface on amazon used by the latest instance types such as m5 and t3 or t3a. With ENA you can achive up to 10Gb throughput on AWS instance. PfSense unfortunately doesn't deliver it out of the box and you may need to spend time if you want to produce a working kernel module for you PfSense. But lucky you, I just share the (current) latest [2.2.1](https://github.com/amzn/amzn-drivers/releases/tag/ena_linux_2.2.11)  version ready to use.

How to add it
-------------
It's easy, you only need to fetch it from the here and place under your kernel modules then just load it manually and ensure it's enabled at boot. let see in details.

Download the code on the PfSense box

<img src="https://github.com/pazarr/aws-misc/blob/main/img/pfsense-login.png" alt="SSH into your PfSense" border="10" />

Fetch the latest kernel module from here 

```bash
$ curl https://raw.githubusercontent.com/pazarr/aws-misc/main/PfSense/if_ena.ko --output /boot/kernel/if_ena.ko
$ cp /boot/kernel/if_ena.ko /boot/modules/
```
For MD5 verification 5241169661c82ac8f8660c4083bfb473

Ensure it has the right ownership and access bits

```
$ chown root:wheel /boot/kernel/if_ena.ko /boot/modules/if_ena.ko
$ chmod 0555 /boot/kernel/if_ena.ko /boot/modules/if_ena.ko
```

Try to load the module

```bash
$ kldload /boot/kernel/if_ena.ko
$ kldstat
Id Refs Address            Size     Name
 1    7 0xffffffff80200000 37191d8  kernel
 2    1 0xffffffff8391a000 28290    if_ena.ko
 3    1 0xffffffff83a19000 10c0     cpuctl.ko
 ```
 If everything goes well, you suppose to see the module loaded.
 
 To load this kernel module during boot, we need to do following changes
 ```
 $ echo 'if_ena_load="YES"' >> /boot/loader.conf
 $ sync
 ```
It's ready to reboot it.
