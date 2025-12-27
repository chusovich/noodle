## Installing on Proxmox
### Download .iso

[OPNsense® Download page](https://opnsense.org/download/)

select architecture, image type (dvd is typical), and location (LeaseWeb East Coast)

### Proxmox Setup
> [!note]
> You don't need to configure any network settomgs yet. You can add software VLANs or PCI pass-through NICs in the later. By default,
OPNsense will be assigned an IP address on the network that Proxmox is on.

Import the .iso into Proxmox and and create a VM with the following settings:

- 2 GB of ram
- 2 cores (?)
- 12 GB of disk space

Everything else should be able to be kept at default.

### Create Our LAN

The network that proxmox is on will be the WAN from the perspective or OPNsense. We can't access OPNsense from it's WAN, so we need to add a machine to it's LAN so we can get to the web UI. First, we need to create the LAN.

#### Creating a Software VLAN

In Proxmox, create a new interface and make it a VLAN

Add this VLAN to the OPNsense VM

Then add an Ubuntu Desktop VM and make it's network adapter the VLAN.

You may want to add a PiHole VM to this VLAN to be the router.

#### Adding an addition Network Interface

you can also give OPNsense access to a physical LAN with an additional NIC with PCIe passthrough

## Tailscale
### References

see [Using OPNsense with Tailscale - Tailscale Docs](https://tailscale.com/kb/1097/install-opnsense)

![](https://www.youtube.com/watch?v=XXx7NDgDaRU&list=PL31RKk0b8QyFtB8vb4jLhQVJdWCcwGSWH)

![](https://youtu.be/Uzcs97XcxiE?si=D7hs8wFd42T_QBh-)

### Installation

This installation is based on the Tailscale docs (see link above).

Open up command line and open the shell (option 8) as the root user.

Run this command twice:

```
# opnsense-code ports
```

Then install Tailscale

```
# cd /usr/ports/security/tailscale
# make install
```

Start and enable the Tailscale daemon

```
# service tailscaled enable
# service tailscaled start
# tailscale version
```

### Connecting to Your Tailnet

Add subnet routes and exit node status as necessary

```
# tailscale up sudo tailscale up /
--advertise-routes=192.0.2.0/24,198.51.100.0/24 /
--advertise-exit-node
```

### Updating Tailscale

Run the following commands to update Tailscale

```
# opnsense-code ports
# cd /usr/ports/security/tailscale
# make deinstall
# make clean
# make install
# service tailscaled restart
```