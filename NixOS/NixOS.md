## [NixOS Wiki](https://nixos.wiki/wiki/Main_Page)
- [NixOS on ARM/Raspberry Pi 4](https://nixos.wiki/wiki/NixOS_on_ARM/Raspberry_Pi_4#USB_boot)
- [NixOS on ARM/Raspberry Ri](https://nixos.wiki/wiki/NixOS_on_ARM/Raspberry_Pi#Status)
- [Flakes](https://nixos.wiki/wiki/Flakes#Basic_Usage_of_Flake)
- [Networking](https://nixos.wiki/wiki/Networking)

## Forums
- [Fan Control on Raspberry Pi](https://discourse.nixos.org/t/enabling-fan-control-on-a-raspberry-pi/40534/2)

## Basic NixOS Commands
- `sudo nixos-collect-garbage` --delete-older-than 15d` - deletes NixOS configurations older than 15 days\

## Example Flake

see the [NixOS Wiki](https://nixos.wiki/wiki/Flakes) for helpful notes.

### Building a configuration.nix the Nixos-unstable Channel

```
{
  inputs = {
    nixpkgs.url = "nixpkgs/nixos-unstable";
  };
  outputs = { self, nixpkgs }: {
    # replace 'joes-desktop' with your hostname here.
    nixosConfigurations.media = nixpkgs.lib.nixosSystem {
      system = "x86_64-linux";
      modules = [ ./configuration.nix ];
    };
  };
}
```

## ZFS on NixOS

see the [NixOS Wiki](https://nixos.wiki/wiki/ZFS#Declarative_mounting_of_ZFS_datasets) on ZFS, or look up specific options using the [NixOS options search](https://search.nixos.org/options?)

### Example Configuration Settings

```nix
# Networking
networking.hostId = "4f98920a"; # used to make sure the pool is imported correctly

# Basic ZFS Settings
boot.supportedFilesystems = [ "zfs" ]; # enable zfs
boot.zfs.forceImportRoot = false; # force the import of zfs root pools
boot.zfs.extraPools = [ "my-pool" "my-other-pool" ]; # mount the pools listed on boot

# Autoscrubbing settings
services.zfs.autoScrub = {
  enable = true; # enable autoscrubbing
  interval = monthly; # default interval for autoscrub is every month
};

# Autosnapshot settings
services.zfs.autoSnapshot = {
  enable = true; # enable snapshots
  montly = 12; # number of monthly snapshots to keep
  weekly = 52; # number of weekly snapshots to keep
  daily = 7; # dido
  hourly = 24; # dido
  frequent = 4; # number of 15 min interval snapshots to keep
};  
```