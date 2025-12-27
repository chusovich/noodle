#nix  #restic

## SFTP Example

```nix
services.restic.backups = {
  sftpBackup = {
    user = "calebh";
    paths = [ "/home/calebh/app-data" ];
    repository = "sftp:backup@192.168.1.100:/backups/name"
    initialize = true; # initializes the repo, don't set if you want manual control
    passwrdFile = "/home/calebh/secrets/rsync";
       timerConfig = {
      onCalendar = "friday 04:00";
    };
  };
```

## Helpful Links
- [NixOS Search Options](https://search.nixos.org/options?channel=unstable&query=services.restic)
- [Official NixOS Wiki](https://wiki.nixos.org/wiki/Restic)
- [Francis Begyn's Blog Post](https://francis.begyn.be/blog/nixos-restic-backups)
- [Restic Docs](https://restic.readthedocs.io/en/stable/index.html)