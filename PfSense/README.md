# Why PfSense?

On AWS you can easily take back control on the VPN traffic setup using some thing more familare tool such as PfSense with network shaping options and more effective routing. There are a few PfSense community or commercial tool available on the AWS Marketplace, but you may want to build your own AMI for PfSense to support automation or audit purpose.

# Why do you ENA driver?

ENA (Elastic Network Adapter) is a high performance virtual Ethernet interface on amazon used by the latest instance types such as m5 and t3 or t3a. With ENA you can achive up to 10Gb throughput on AWS instance. PfSense unfortunately doesn't deliver it out of the box and you may need to spend time if you want to produce a working kernel module for you PfSense. But lucky you, I just share the (current) latest [2.2.1](https://github.com/amzn/amzn-drivers/releases/tag/ena_linux_2.2.11)  version ready to use.

# How to add?

It's easy, you only need to fetch it from the here and place under your kernel modules then just load it manually and ensure it's enabled at boot. let see in details.

Download the code on the PfSense box
