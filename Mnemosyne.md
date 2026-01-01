```nix
# flake.nix
{
  description = "a flake for a media server";
  
  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs/nixos-unstable";
  };
  
  outputs = {nixpkgs, ...} @ inputs:
  {
	# this line sets a nixosConfiguration output
	# using the hostname "nixos"
	# using the nixosSystem function in lib in the nixpkgs lake
    nixosConfigurations.mnemosyne = nixpkgs.lib.nixosSystem
	  specialArgs = { inherit inputs };
      modules = [
        ./configuration.nix # also includes hardware-configuration.nix
      ]
  };
}
```

```nix
# configuration.nix
{ config, lib, pkgs, ... }:

{
  imports = [
    ./hardware-configuration.nix
  ]
}
```