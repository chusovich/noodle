See [Oracle's Documentation Guide](https://docs.oracle.com/cd/E23823_01/html/819-5461/toc.html) for more detailed information and examples.

## Pools and vDevs

### Create a Mirror Pool

Create a new pool called new-pool using the devices /dev/sdb and /dev/sdc in a mirror

```terminal
sudo zpool create new-pool mirror /dev/sdb /dev/sdc
```

You can also use the ```-m``` option to specify the mount point. The default mount point is ```/poolname```

```terminal
sudo zpool create -m /usr/sharepool new-pool mirror /dev/sdb /dev/sdc
```

### Checking the Status of All Zfs Pools

```terminal
sudo zpool status
```

### Add Devices to an Existing ZFS Pool

Add the devices /dev/sdb and /dev/sdc to the pool called existing-pool

```terminal
zpool add existing-pool mirror /dev/sdb /dev/sdc
```

## Datasets

### Create a New Dataset

Create a dataset named 'mydataset' on the pool 'mypool'

```terminal
sudo zfs create mypool/mydataset
``` 

### Check the Dataset Seize

```terminal
sudo zfs list mypool/dataset
```

### Set Additional Properties

```terminal
sudo zfs set compression=on mypool/mydataset # turns on compression
sudo zfs set quota=10G mypool/mydataset # set the sixe quota to 10GB
sudo zfs get quota /mypool/mydataset # view the quota of the dataset
sudo zfs set canmount=on and mountpoint=/mount/point mypool/mydataset # these two options mount the dataset at boot
```

## Permissions

```terminal
zfs allow
```

## Snapshots

```
coming soon
```