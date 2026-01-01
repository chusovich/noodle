#nixos #flakes

## Boilerplate Flake

```nix
# flake.nix
{
  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs?ref=nixos-unstable";
    flake-parts.url = "github:hercules-ci/flake-parts";
  };

  outputs = inputs @ { flake-parts, ... }:
    flake-parts.lib.mkFlake { inherit inputs; }
    {
    };
}
```

## Example Flake

```nix
# flake.nix
{
  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs?ref=nixos-unstable";
    flake-parts.url = "github:hercules-ci/flake-parts";
  };

  outputs = inputs @ { flake-parts, ... }:
    flake-parts.lib.mkFlake { inherit inputs; }
    {
      systems = [ "x86_64-linux" "x86_64-darwin"]
      perSystem = { pkgs, self', ...}: {
	    packages.mypackage = pkgs.sl;
	    devShells.default = pkgs.mkShell {
	      packages = [ self'.packages.mypackage]
	    };
	  };
    };
}
```

## Modularized Version

```nix
# flake.nix
{
  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs?ref=nixos-unstable";
    flake-parts.url = "github:hercules-ci/flake-parts";
  };

  outputs = inputs @ { flake-parts, ... }:
    flake-parts.lib.mkFlake { inherit inputs; }
    {
      imports = [
        ./packages.nix
        ./shell.nix
      ];
    };
}
```

```nix
# package.nix
{
  perSystem = { pkgs, ...}: {
    packages.mypackage = pkgs.sl;
  }
}
```

```nix
# shell.nix
{
  perSystem = { pkgs, self', ...}: {
    devShells.default = pkgs.mkShell {
      packages = [ self'.packages.mypackage ]
    };
  };
}
```

## Adding NixOS Configuration

```nix
# nixos.nix
{ inputs, ... }: {
  flake = { # puts it at the top level of the flake
    nixoconfigurations.main = inputs.nixpkgs.lib.nixosSystem {
      modules = [
        ./configuration.nix
      ];
    }
  };
}
```