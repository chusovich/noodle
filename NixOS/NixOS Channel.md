#nix

## Changing NixOS Channel (Upgrading)

[NixOS Manul Ref](https://nixos.org/manual/nixos/stable/#sec-upgrading)

```bash
nix-channel --add https://channels.nixos.org/channel-name nixos
```

```bash
nixos-rebuild switch --upgrade
```

Example channel names:

- nixos-25.05
- nixos-unstable
- nixos-25.05-small
