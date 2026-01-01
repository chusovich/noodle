#nixos #nixos-anywhere 
## Links
- [nixos-anywhere GitHub](https://github.com/nix-community/nixos-anywhere)
- [nixos-anywhere-examples GitHub](https://github.com/nix-community/nixos-anywhere-examples)
- 
## Requirements
- You need a flake.nix
- You need root ssh access to the system
- The drive name (e.g. `/dev/sda`) to install NixOS on

## Look at the Example Files in the Examples Repo
- Set the disk-config.nix as desired
- Add your publish ssh key to the configuration.nix file

## Commands
```bash
nix run nixpkgs#nixos-anywhere -- \
--flake .#generic
--generate-hardware-config nixos-generate-config /hardware-configuration.nix \
root@192.168.10.10
```

