# selinux-policy-creator

> ️⚠️ You can find information on my GnuPG public key in my [GitHub profile README](https://github.com/duxsco/)! ⚠️

`selinux-policy-creator.sh` creates SELinux policies within `~/my_selinux_policies` while running in permissive mode.

Sample run:

```shell
❯ getenforce
Permissive

❯ mkdir ~/my_selinux_policies

❯ selinux-policy-creator.sh
******************** IMPORTANT ***********************
To make this policy package active, execute:

semodule -i my-0000000-udev_t-cgroup_t-allow.pp

******************** IMPORTANT ***********************
To make this policy package active, execute:

semodule -i my-0000000-udev_t-cgroup_t-dontaudit.pp

******************** IMPORTANT ***********************
To make this policy package active, execute:

semodule -i my-0000001-mdadm_t-udev_runtime_t-allow.pp
etc.

❯ ls -1 ~/my_selinux_policies/
my-0000000-udev_t-cgroup_t-allow.pp
my-0000000-udev_t-cgroup_t-allow.te
my-0000000-udev_t-cgroup_t-dontaudit.pp
my-0000000-udev_t-cgroup_t-dontaudit.te
my-0000000-udev_t-cgroup_t-readme.txt
my-0000001-mdadm_t-udev_runtime_t-allow.pp
my-0000001-mdadm_t-udev_runtime_t-allow.te
my-0000001-mdadm_t-udev_runtime_t-dontaudit.pp
my-0000001-mdadm_t-udev_runtime_t-dontaudit.te
my-0000001-mdadm_t-udev_runtime_t-readme.txt
my-0000002-mdadm_t-debugfs_t-allow.pp
my-0000002-mdadm_t-debugfs_t-allow.te
my-0000002-mdadm_t-debugfs_t-dontaudit.pp
my-0000002-mdadm_t-debugfs_t-dontaudit.te
my-0000002-mdadm_t-debugfs_t-readme.txt
etc.

❯ head -n 99 ~/my_selinux_policies/my-0000000-udev_t-cgroup_t-*.{te,txt}
==> /root/my_selinux_policies/my-0000000-udev_t-cgroup_t-allow.te <==

module my-0000000-udev_t-cgroup_t-allow 1.0;

require {
	type cgroup_t;
	type udev_t;
	class dir { add_name create write };
	class file write;
}

#============= udev_t ==============
allow udev_t cgroup_t:dir { add_name create write };
allow udev_t cgroup_t:file write;

==> /root/my_selinux_policies/my-0000000-udev_t-cgroup_t-dontaudit.te <==

module my-0000000-udev_t-cgroup_t-dontaudit 1.0;

require {
	type cgroup_t;
	type udev_t;
	class dir { add_name create write };
	class file write;
}

#============= udev_t ==============
dontaudit udev_t cgroup_t:dir { add_name create write };
dontaudit udev_t cgroup_t:file write;

==> /root/my_selinux_policies/my-0000000-udev_t-cgroup_t-readme.txt <==
The SELinux denial(s):
----
[   12.184148] audit: type=1400 audit(1664055813.880:3): avc:  denied  { write } for  pid=995 comm="systemd-udevd" name="systemd-udevd.service" dev="cgroup2" ino=1952 scontext=system_u:system_r:udev_t:s0 tcontext=system_u:object_r:cgroup_t:s0 tclass=dir permissive=1
[   12.192303] audit: type=1400 audit(1664055813.880:4): avc:  denied  { add_name } for  pid=995 comm="systemd-udevd" name="udev" scontext=system_u:system_r:udev_t:s0 tcontext=system_u:object_r:cgroup_t:s0 tclass=dir permissive=1
[   12.206204] audit: type=1400 audit(1664055813.880:5): avc:  denied  { create } for  pid=995 comm="systemd-udevd" name="udev" scontext=system_u:system_r:udev_t:s0 tcontext=system_u:object_r:cgroup_t:s0 tclass=dir permissive=1
[   12.219030] audit: type=1400 audit(1664055813.880:6): avc:  denied  { write } for  pid=995 comm="systemd-udevd" name="cgroup.procs" dev="cgroup2" ino=1987 scontext=system_u:system_r:udev_t:s0 tcontext=system_u:object_r:cgroup_t:s0 tclass=file permissive=1

Object(s) mentioned in SELinux denials with inode number(s):
❯ find / -inum "1987" -context "system_u:object_r:cgroup_t:s0"
/sys/fs/cgroup/system.slice/systemd-udevd.service/udev/cgroup.procs
❯ find / -inum "1952" -context "system_u:object_r:cgroup_t:s0"
/sys/fs/cgroup/system.slice/systemd-udevd.service
```
