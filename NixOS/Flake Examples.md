#nixos #flakes

From [Vimjoyer's Ultimate Nix Flakes Guide video](https://www.youtube.com/watch?v=JCeYq72Sko0)

## Super Basic Flake

From [the official NixOS wiki](https://wiki.nixos.org/wiki/Flakes)

```nix
{
  description = "A very basic flake";

  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs?ref=nixos-unstable";
  };

  outputs = { self, nixpkgs }: {
    packages.x86_64-linux = {
      hello = nixpkgs.legacyPackages.x86_64-linux.hello;
      # the expression below is the same as nixpkgs.legacyPackages.x86_64-linux.hello
      default = self.packages.x86_64-linux.hello; 
    };
  };
}
```

## Nix Commands to Interact with Flakes

```nix
nix run         # runs a packages binary
nix build       # builds a package
nix develop     # activates a dev shell
nixos-rebuild   # builds a nixos system 
home-manager    # builds a home configuration
# and many more

nix flake show  # shows the outputs of the flake

# if not configuration is specified it will use the default
nix run 
nix run .
nix run ~/myflake

# specifying an output
nix run .#hello
nix run ~/myflake#hello
```

Each interacts with a different output of the flake

- `nix run` and `nix build` looks for `outputs.packages."SYSTEM".default`
- `nix develop` looks for `outputs.devShells."SYSTEM".default`
- `nixos-rebuild` looks for `outputs.nixosConfigurations."HOSTNAME"`
- `home-manager` looks for `outpust.homeConfigurations."USERNAME"`

## Basic Flake with Variables

```nix
{
  description = "A very basic flake";

  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs?ref=nixos-unstable";
  };

  outputs = { self, nixpkgs }:
  let 
    pkgs = nixpkgs.legacyPackages.x86_64-linux;
  in
  {
    packages.x86_64-linux = {
      default = pkgs.hello; # shortened using variable in let statement
      hello = pkgs.hello;
    };
  };
}
```

## Multiple Inputs in a Flake

```nix
{
  description = "A very basic flake";

  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs/nixos-unstable";
    nixpkgsveryold.url = "github:nixos/nixpkgs/21.11";
  };

  outputs = { self, nixpkgs, nixpkgsveryold }:
  let 
    pkgs = nixpkgs.legacyPackages.x86_64-linux;
    pkgsold = nixpkgsveryold.legacyPackages.x86_64-linux;
  in
  {
    packages.x86_64-linux = {
      default = pkgs.hello; # shortened using variable in let statement
      hello = pkgs.hello;
    };
    
    # add some dev shells with custom inputs
    # note mkShell is a function from nixpgs
    devShells.x86_64-linux.default = pgks.mkShell {
      buildInputs = [ pkgs.neovim pkgsold.vim ];
    }
  };
}
```

### Simplifying the Inputs Section

```nix
{
# ...


# if we write it this way, we can refer to nixpkgs directly, or to any
# inputs if we first put inputs.
outputs = {nixpkgs, ...} @ inputs:
  let 
    pkgs = nixpkgs.legacyPackages.x86_64-linux; 
    pkgsold = inputs.nixpkgsveryold.legacyPackages.x86_64-linux; # must use inputs.
  in
}
```

## The Lock File

Run `nix flake metadata` to get info about the `flake.lock` file

Run `nix flake update` to update the lock file

## Inheriting Inputs

```nix
```nix
{
  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs/nixos-unstable";
    nixpkgsveryold.url = "github:nixos/nixpkgs/21.11";
    home-manager.url = "github.nixcommunity/home-manager";
    # the home-manager flake has it's own nixpkgs input, but we can have it inherit
    # from our nixpkgs version using the following
    home-manager.inputs.nixpgs.follows = "nixpkgs";
  };
```

## Flakes for NixOS Systems
### Bare Bones NixOS Flake

```nix
{
  description = "A very basic NixoOS system flake";
  
  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs/nixos-unstable";
  };
  
  outputs = {nixpkgs, ...} @ inputs:
  {
	# this line sets a nixosConfiguration output
	# using the hostname "nixos"
	# using the nixosSystem function in lib in the nixpkgs lake
    nixosConfigurations.nixos = nixpkgs.lib.nixosSystem
      modules = [
        ./configuration.nix # also includes hardware-configuration.nix
      ]
  };
}
```

### NixOS System Flake with Custom Inputs

```nix
{
  description = "A very NixoOS system flake that passes inputs to the modules";
  
  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs/nixos-unstable";
    home-manager.url = "github.nixcommunity/home-manager";
    home-manager.inputs.nixpgs.follows = "nixpkgs";
  };
  
  outputs = {nixpkgs, ...} @ inputs:
  {
	# this line sets a nixosConfiguration output
	# using the hostname "nixos"
	# using the nixosSystem function in lib in the nixpkgs lake
    nixosConfigurations.nixos = nixpkgs.lib.nixosSystem
	  specialArgs = { inherit inputs };
      modules = [
        ./configuration.nix # also includes hardware-configuration.nix
      ]
  };
}
```

```nix
# configuration.nix
{ pkgs, lib, inputs, ... }: {
  imports = [
    ./hardware-configuration.nix
  ];
  
  environment.systemPackages = with pkgs; [
    vim
    # an old version of neovim example
    inputs.nixpkgsveryold.legacyPackages.${pkgs.system}.neovim
  ];


}
```