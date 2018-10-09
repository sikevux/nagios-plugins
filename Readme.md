# Check ZFS for FreeBSD
This tool checks if your zpools are healty.

Just execute:
```
./check_zfs <poolname> <verbosity>

./check_zfs zroot 3
```

It will return something like:
```
OK ZPOOL zroot : ONLINE {Size:1.81T Used:1.08T Avail:746G Cap:59%} <mirror-0:ONLINE> gpt/zroot0:ONLINE gpt/zroot1:ONLINE
```

To use it with Nagios add in commands.conf:
```
object CheckCommand "zfs" {
        import "plugin-check-command"

        command = [ CustomPluginDir + "/check_zfs/check_zfs" ]
        command += [ "$zfs_pool$", "$zfs_verbosity$" ]
        vars.zfs_pool = "zroot"
        vars.zfs_verbosity = "3"
}
```

Then add to services.conf:
```
apply Service "zfs-zroot" {
  import "generic-service"
  check_command = "zfs"
  vars.zfs_pool = "zroot"
  assign where host.name == NodeName
}

apply Service "zfs-zstorage" {
  import "generic-service"
  check_command = "zfs"
  vars.zfs_pool = "zstorage"
  assign where host.name == NodeName
}

```
