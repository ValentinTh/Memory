Make a ProxMox Node with NAT VM available outside the local network.
- OS : DEBIAN 9
- Host : Kimsufi

## Basic fresh install Proxmox

First install your Proxmox OS, then :
- "nano /etc/apt/sources.list.d/pve-enterprise.list"
- coment the deb line #

nano /etc/apt/sources.list.d/pve-install-repo.list (OVH config file for DEB PVE No sub)

```
deb http://download.proxmox.com/debian stretch pvetest
deb http://download.proxmox.com/debian stretch pve-no-subscription
```

nano nano /etc/apt/sources.list (Config file for debian DEB based on OVH sources)

```
#
deb http://debian.mirrors.ovh.net/debian stretch main contrib non-free
deb-src http://debian.mirrors.ovh.net/debian stretch main contrib non-free

deb http://security.debian.org/debian-security stretch/updates main
deb-src http://security.debian.org/debian-security stretch/updates main

# stretch-updates, previously known as 'volatile'
deb http://debian.mirrors.ovh.net/debian stretch-updates main
deb-src http://debian.mirrors.ovh.net/debian stretch-updates main'
```

- apt-get update && apt-get upgrade -y

Method for basic fresh install (Other hosts)
- setup "nano /etc/apt/sources.list"
- add "deb http://download.proxmox.com/debian stretch pve-no-subscription"

## Software configurations

### Add templates for LXC containers
- pveam update
- pveam available # List all the templates
- pveam download local #$$$$$$Package$$$$$ #Add template
- pveam remove local:vztmpl/#$$$$$Package$$$$$ #Remove template

## Container LXC creation

##### General
- CT ID : number
- HostName : reverse
- Tick unprivileged
- Password : root passwd

##### Modele
- Storage : Local
- Modele : choose OS

##### Disk root
- Size : Number of GB

##### CPU
- Core : nb vcore

##### Memory
- Ram : nb gb
- Swap : 256mo

##### Network
- Name (i.e. eth0): eth0
- MAC Adress:
- Bridge: vmbr2
- Tag VLAN:
- Networking limit (MB/s):
- Firewall :
- IPv4:
- IPv4/CIDR: 192.168.1.X/24
- Gateway (IPv4): 192.168.1.254
- IPv6:
- IPv6/CIDR:
- Gateway (IPv6):

##### DNS
- Let empty

## VM configurations

### Unlock Root Login Debian 8/9
- Visit console
- Enter credentials
- nano /etc/ssh/sshd_config
- Change "PermitRootLogin" for yes
- PermitRootLogin yes
- Save and exit
- systemctl restart ssh

### SSH Port
In case of use of a different SSH port, you only need to uncomment the line Port and setup the number that you want.
Default : 22

- Port $$
- Save and exit
- systemctl restart ssh

### Usefull tools

- apt install htop fail2ban nload

## Tips

ls /sys/class/net/ # Allow to know every single network interfaces available on the node.

# CONFIG
nano /etc/network/interfaces

```
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

# The loopback network interface
auto lo
iface lo inet loopback

# for Routing
auto vmbr1
iface vmbr1 inet manual
        bridge_ports dummy0
        bridge_stp off
        bridge_fd 0

# vmbr0 : Bridging. Make sure to use only MAC adresses that were assigned to you.
auto vmbr0
iface vmbr0 inet static
        address xx.xx.xx.xx/24
        gateway xx.xx.xx.254
        bridge_ports eno1
        bridge_stp off
        bridge_fd 0

auto vmbr2
iface vmbr2 inet static
        address 192.168.1.254
        netmask 255.255.255.0
        broadcast 192.168.1.255
        bridge_ports LAN
        bridge_stp off
        bridge_fd 0
        post-up echo 1 > /proc/sys/net/ipv4/ip_forward
        post-up iptables -t nat -A POSTROUTING -s '192.168.1.0/24' -o vmbr0 -j MASQUERADE
        post-down iptables -t nat -D POSTROUTING -s '192.168.1.0/24' -o vmbr0 -j MASQUERADE

#VM NAT
post-up iptables -t nat -A PREROUTING -i vmbr0 -p tcp --dport $$$$ -j DNAT --to 192.168.1.x:$$$$
post-down iptables -t nat -A PREROUTING -i vmbr0 -p tcp --dport $$$$ -j DNAT --to 192.168.1.x:$$$$
...

```
systemctl restart networking
systemctl status networking
systemctl restart systemd-networkd

https://wiki.archlinux.org/index.php/systemd-networkd

=====
LINKS :
- https://linuxconfig.org/how-to-setup-a-static-ip-address-on-debian-linux
- https://medium.com/@cpt_midnight/static-ip-in-debian-9-stretch-acb4e5cb7dc1
- https://debian-facile.org/doc:reseau:interfaces
- http://community.ovh.com/t/configuration-reseau-vrack-debian-stretch/3617
- https://www.sky-future.net/2015/12/13/tuto-proxmox-n3-creation-dun-lan-prive-et-permettre-lacces-internet-sur-ce-lan-sans-ipfo/
- https://serverfault.com/questions/564445/how-can-i-forward-the-http-and-ssh-port-to-my-internal-server-using-iptables
- https://askubuntu.com/questions/311053/how-to-make-ip-forwarding-permanent
- https://www.ameir.net/blog/archives/55-running-proxmox-behind-a-single-ip-address.html/comment-page-1
- https://www.eolya.fr/2016/02/15/ovh-proxmox-reseau-prive/
- http://wiki.csnu.org/index.php/Proxmox_5
- https://raymii.org/s/tutorials/Proxmox_VE_One_Public_IP.html
- https://silentkernel.fr/proxmox-avec-une-seule-ip-forwarding-de-port/
- https://angristan.xyz/setup-network-bridge-lxc-net/

SOURCES :
- https://www.svennd.be/proxmox-ve-5-0-fix-updates-upgrades/
- https://cyberpersons.com/2016/07/27/setup-nat-proxmox/
- https://unix.stackexchange.com/questions/134682/iptables-add-rule-for-interface-before-it-comes-up
- https://serverfault.com/questions/232581/permament-iptables-port-forwarding
- https://www.tutos.snatch-crash.fr/proxmox-creation-de-conteneur-lxc/
- https://pve.proxmox.com/wiki/Unprivileged_LXC_containers
- https://serverfault.com/questions/729810/dnat-port-range-with-different-internal-port-range-with-iptables
- http://linusramos.blogspot.fr/2011/07/iptables-port-forwarding.html

A + : 
https://bash.cyberciti.biz/virtualization/shell-script-to-setup-an-lxd-linux-containers-vm-lab-for-testing-purpose/
Auto setup VM, IP attribution.
- https://www.sky-future.net/2014/01/22/tuto-installer-une-seedbox-sur-debian-avec-interface-web-lecteur-video/
- https://www.kiloroot.com/proxmox-kimsufi-ovh-soyoustart-ipv6-host-multiple-containers-and-virtual-machines-on-a-single-kimsufi-server-using-ipv6-and-proxmox/
- https://blog.blaisethirard.com/tag/kimsufi/
- http://www.viedugeek.eu/topic/227-cr%C3%A9er-des-serveurs-virtuels-debian-7-wheezy-avec-lxc-sur-un-d%C3%A9di%C3%A9-ovh-kimsufi/
- http://hw7.net/?p=252
- http://www.guiguishow.info/2012/10/30/utiliser-lxc-sur-un-kimsufi/#toc-3009-configuration-rseau
- http://www.guiguishow.info/2012/10/30/utiliser-lxc-sur-un-kimsufi/#toc-3009-compiler-un-noyau-utilisant-les-cgroups-grsec
- https://blog.cepharum.de/en/post/lxc-host-featuring-ipv6-connectivity.html
