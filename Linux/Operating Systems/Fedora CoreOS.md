## Fedora CoreOS

### Installing on Bare Metal

Get the iso off the [download page](https://fedoraproject.org/coreos/download/?stream=stable#baremetal) and flash it do a USB drive.

If flashing using Rufus, make sure to select "DD Mode" when prompted. You can boot in either BIOS of UEFI mode.

Run:

```terminal
sudo coreos-installer install /dev/sda \
    --ignition-url https://example.com/example.ign
```

Use the following flag if using http

```
--insecure-ignition
```

then reboot the system

### Password Hashing

Using podman container:

```
podman run -ti --rm quay.io/coreos/mkpasswd --method=yescrypt
```

### Creating an Ignition File

see the [Fedora CoreOS documentation](https://docs.fedoraproject.org/en-US/fedora-coreos/producing-ign/)

Run

```
butane --pretty --strict example.bu > example.ign
```

or on Windows:

```
butane --pretty --strict example.bu --output example.ign
```

### Butane File Exmamples
#### Setting a Hostname

```yaml
variant: fcos
version: 1.6.0
storage:
  files:
    - path: /etc/hostname
      mode: 0644
      contents:
        inline: myhostname
```

#### Enabling the Docker Socket

```
systemd:
  units:
  - name: docker.socket
    enabled: true
     contents: |
       [Unit]
       Description=Docker Socket for the API
       PartOf=docker.service
        [Socket]
       ListenStream=/var/run/docker.sock
       SocketMode=0660
       SocketUser=root
       SocketGroup=docker
```

#### Basic uCore Example

```yaml
variant: fcos
version: 1.4.0
passwd:
  users:
    - name: core
      ssh_authorized_keys:
        - YOUR_SSH_PUB_KEY_HERE
      password_hash: YOUR_GOOD_PASSWORD_HASH_HERE
storage:
  directories:
    - path: /etc/ucore-autorebase
      mode: 0754
systemd:
  units:
    - name: ucore-unsigned-autorebase.service
      enabled: true
      contents: |
        [Unit]
        Description=uCore autorebase to unsigned OCI and reboot
        ConditionPathExists=!/etc/ucore-autorebase/unverified
        ConditionPathExists=!/etc/ucore-autorebase/signed
        After=network-online.target
        Wants=network-online.target
        [Service]
        Type=oneshot
        StandardOutput=journal+console
        ExecStart=/usr/bin/rpm-ostree rebase --bypass-driver ostree-unverified-registry:ghcr.io/ublue-os/ucore:stable
        ExecStart=/usr/bin/touch /etc/ucore-autorebase/unverified
        ExecStart=/usr/bin/systemctl disable ucore-unsigned-autorebase.service
        ExecStart=/usr/bin/systemctl reboot
        [Install]
        WantedBy=multi-user.target
    - name: ucore-signed-autorebase.service
      enabled: true
      contents: |
        [Unit]
        Description=uCore autorebase to signed OCI and reboot
        ConditionPathExists=/etc/ucore-autorebase/unverified
        ConditionPathExists=!/etc/ucore-autorebase/signed
        After=network-online.target
        Wants=network-online.target
        [Service]
        Type=oneshot
        StandardOutput=journal+console
        ExecStart=/usr/bin/rpm-ostree rebase --bypass-driver ostree-image-signed:docker://ghcr.io/ublue-os/ucore:stable
        ExecStart=/usr/bin/touch /etc/ucore-autorebase/signed
        ExecStart=/usr/bin/systemctl disable ucore-signed-autorebase.service
        ExecStart=/usr/bin/systemctl reboot
        [Install]
        WantedBy=multi-user.target
```

#### Example User Config

```
passwd:
  users:
    - name: core
      ssh_authorized_keys:
        - YOUR_SSH_PUB_KEY_HERE
      groups:
        - wheel
        - sudo
        - docker
```

#### Installing Docker CE on First Boot

```yaml
variant: fcos
version: 1.6.0
systemd:
  units:
    # Install Docker CE
    - name: rpm-ostree-install-docker-ce.service
      enabled: true
      contents: |
        [Unit]
        Description=Install Docker CE
        Wants=network-online.target
        After=network-online.target
        Before=zincati.service
        ConditionPathExists=!/var/lib/%N.stamp

        [Service]
        Type=oneshot
        RemainAfterExit=yes
        ExecStart=/usr/bin/curl --output-dir "/etc/yum.repos.d" --remote-name https://download.docker.com/linux/fedora/docker-ce.repo
        ExecStart=/usr/bin/rpm-ostree override remove moby-engine containerd runc docker-cli --install docker-ce
        ExecStart=/usr/bin/touch /var/lib/%N.stamp
        ExecStart=/usr/bin/systemctl --no-block reboot

        [Install]
        WantedBy=multi-user.target
``` 