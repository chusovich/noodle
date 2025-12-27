## Downloading the Firmware

use the [OpenWRT Firmware Selector](https://firmware-selector.openwrt.org/) to download the appropriate image.

## USB Ethernet Adapters

"for USB-to-Ethernet ASIX AX88179 based USB 3.0/2.0\\ to Gigabit Ethernet adapters" (see [here](https://openwrt.org/packages/pkgdata/kmod-usb-net-asix-ax88179))

```
opkg update
opkg install kmod-usb-net-asix-ax88179
```

## Updates

it is highly discouraged to update all packages use opkg update

see [here](https://openwrt.org/meta/infobox/upgrade_packages_warning)

```
opkg update
opkg upgrade <package>
```

## Hostname

``` title="/etc/config/system"
config system
	option hostname '<your-hostname>'
```

## Tailscale
### Installation

see [this OpenWRT wiki page](https://openwrt.org/docs/guide-user/services/vpn/tailscale/start) for the basic setup guide

see [this OpenWRT wiki page](https://openwrt.org/toh/raspberry_pi_foundation/raspberry_pi) for all things Raspberry Pi

```
opkg update
opkg install tailscale
opkg install iptables-nft kmod-ipt-conntrack kmod-ipt-conntrack-extra kmod-ipt-conntrack-label kmod-nft-nat 
opkg install kmod-ipt-nat

service tailscale restart
tailscale up --ssh --advertise-routes=192.168.10.0/24
```

### Configuraiton

```title="/etc/config/firewall"
config zone
	option name 'tailscale'
	list network 'tailscale'
	option input 'ACCEPT'
	option output 'ACCEPT'
	option forward 'ACCEPT'
	option masq '1'
	option mtu_fix '1'

# At the end of the file
config rule
	option name 'Allow Tailscale'
	option src '*'
	option target 'ACCEPT'
	option proto 'udp'
	option dest_port '41641'

config forwarding
	option src 'tailscale'
	option dest 'lan'

config forwarding
	option src 'lan'
	option dest 'tailscale'
```

``` title="/etc/config/network"
config interface 'tailscale'
	option device 'tailscale0'	
	option proto 'none'
```

## Raspberry Pi 4
### Case Fan

see [here](https://openwrt.org/toh/raspberry_pi_foundation/raspberry_pi#adding_a_case_fan)

```
opkg update
opkg install kmod-hwmon-gpiofan
```

``` title="/boot/config.txt"
# temperature in milliCelsius
dtoverlay=gpio-fan,gpiopin=14,temp=80000
```

### Configure Wireless WAN
#### Configure Wireless Interface

``` title="/etc/config/wireless"
config wifi-device 'radio0'
    option type 'mac80211'
    option path 'platform/soc/fe300000.mmcnr/mmc_host/mmc1/mmc1:0001/mmc1:0001:1'
    option channel '7'
    option band '5g'
    option htmode 'HT20'
    option hwmode '11g'
    option short_gi_40 '0'
    option disabled '0'

config wifi-iface 'wifinet1'
    option device 'radio0'
    option mode 'sta'
    option network 'wwan' # or is it lan?
    option ssid 'Lothlorien'
    option encryption 'psk2'
    option key 'EnChus81222'
```

#### Update the Wifi Settings on Boot

``` title="/etc/rc.local"
uci commit wireless
wifi
```

## DNS Configuration

Add the last line for ip addresses

```
config dnsmasq
        option domainneeded '1'
		... 
		option domain 'husovich.com'
		...
		option ednspacket_max '1232'
        list address '/.mymedia.husovich.com/192.168.10.124'
```
