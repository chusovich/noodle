#nixos #nixos-anywhere #raspberrypi #disko

## References
- [nmvd/nixos-raspberrypi README.md](https://github.com/nvmd/nixos-raspberrypi/tree/develop?tab=readme-ov-file#)
- [nixos discourse topic](https://discourse.nixos.org/t/how-best-to-deploy-to-raspberry-pi/70410/8)

### Guide
#### 1) Build the SD Card Image
- Clone the repo: `git clone https://github.com/nvmd/nixos-raspberrypi.git`
- `cd nixos-raspberrypi`
- Build the image `sudo nix build .#installerImages.rpi4`
- Image is built in `/result/sd-image`

#### 2) Uncompresss

`/result` is read-only so you have to decompress the file to somewhere else:

`zstd -d nixos-installer-rpi4-uboot.img.zst -o ~/nixos-rpi4.img`

#### 3) Flash USB Drive

I used `nix-shell -p mediawriter`

#### 4) Boot the Raspberry Pi from the USB Drive
- Power off the Pi
- Remove the SD card
- Connect the USB drive
- Power on
- Reinsert SD card
- Root credentials with appear on the screen after a while

#### 4) Confirm Ssh Root Access

```
ssh root@<ip-address>
```

#### 5) Build the Flake