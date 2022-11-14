---
title: "Ubuntu Network Manager Enabling and disabling NetworkManager on Ubuntu"
tags:
- 
weight: 0
---

NetworkManager is a backend service that controls the network interfaces on your Ubuntu operating system. An alternative to NetworkManager is systemd-networkd. On Ubuntu desktop, network manager is the default service that manages network interfaces through the graphical user interface. Therefore, If you want to configure IP addresses via GUI, then the network-manager should be enabled.

An Alternative to Ubuntu network manager is systemd-networkd, which is the default backend service in Ubuntu server 18.04.

So if you want to disable the NetworkManager, then the networkd service should be enabled, while it is better to disable networkd service when network manager is running.

## Disable Network Manager and enable systemd-networkd

First, run the following set of commands to disable the NetworkManager:

```
sudo systemctl stop NetworkManager
sudo systemctl disable NetworkManager
sudo systemctl mask NetworkManager
```

Next, start and enable the systemd-networkd service:

```
sudo systemctl unmask systemd-networkd.service
sudo systemctl enable systemd-networkd.service
sudo systemctl start systemd-networkd.service
```

Add the interface configuration to the netplan config file (in the /etc/netplan directory):

```
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: yes
```

Apply the changes by running the following command:

```
sudo netplan apply
```

In the preceding example, we configured the enp0s3 interface to lease IP addresses from the DHCP Server. If you want to set static IP addresses, Click on the following link to learn [how to configure static IP address with netplan](https://www.configserverfirewall.com/ubuntu-linux/configure-ubuntu-server-static-ip-address/).

### Enable NetworkManager and Disable systemd networkd

Starting and enabling Ubuntu Network Manager can be done with the following steps (Which is not recommended in Ubuntu server).

First, stop the systemd-networkd service:

```
sudo systemctl disable systemd-networkd.service
sudo systemctl mask systemd-networkd.service
sudo systemctl stop systemd-networkd.service
```

Install NetworkManager on Ubuntu:

```
sudo apt-get install network-manager
```

Open the .yaml config file inside the /etc/netplan directory and replace the existing configuration with following:

```
network:
  version: 2
  renderer: NetworkManager
```

Generate backend specific configuration files for NetworkManager with netplan command:

```
sudo netplan generate
```

Start the NetworkManager Service:

```
sudo systemctl unmask NetworkManager
sudo systemctl enable NetworkManager
sudo systemctl start NetworkManager
```

Now the NetworkManager is enabled, interface configurations can be done via the GUI or from the command line, using the nmcli command.

While it is possible to manage networking on Ubuntu server via network manager, it has largely been replaced with [Netplan](https://www.configserverfirewall.com/ubuntu-linux/configure-ubuntu-server-static-ip-address/#ubuntu-netplan-yaml).