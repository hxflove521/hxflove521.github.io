---
title: 'VMware 下Linux扩展磁盘空间（CentOS 7）'
layout: post
tags:
  - study
published: false
category: 工作笔记
---
在使用中需要给VMware安装的CentOS7进行硬盘扩容，一下给出操作流程。

####查看挂载点：
```shell
[root@localhost ~]# df -h
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root   48G  4.7G   43G  10% /
devtmpfs                 481M     0  481M   0% /dev
tmpfs                    490M     0  490M   0% /dev/shm
tmpfs                    490M  6.7M  484M   2% /run
tmpfs                    490M     0  490M   0% /sys/fs/cgroup
/dev/sda1                497M  124M  374M  25% /boot
[root@localhost ~]#
```
在这里我们需要将`/`挂载点扩容，记住Filesystem为`/dev/mapper/centos-root`，
### 一、扩展VMware硬盘空间
关闭Vmware 的 Linux系统，虚拟机设置--硬盘--硬盘实用工具--扩展(E),输入最大磁盘大小(GB)。
### 二、对新增加的硬盘进行分区、格式化
1. 查看当前硬盘状态：
		[root@localhost ~]# fdisk -l
		Disk /dev/sda: 75.2 GB, 75161927680 bytes, 146800640 sectors
		Units = sectors of 1 * 512 = 512 bytes
		Sector size (logical/physical): 512 bytes / 512 bytes
		I/O size (minimum/optimal): 512 bytes / 512 bytes
		Disk label type: dos
		Disk identifier: 0x00095bf4

		Device Boot      Start         End      Blocks   Id  System
		/dev/sda1   *        2048     1026047      512000   83  Linux
		/dev/sda2         1026048    41943039    20458496   8e  Linux LVM
		/dev/sda3        41943040   104857599    31457280   8e  Linux LVM

		Disk /dev/mapper/centos-root: 50.9 GB, 50864324608 bytes, 99344384 sectors
		Units = sectors of 1 * 512 = 512 bytes
		Sector size (logical/physical): 512 bytes / 512 bytes
		I/O size (minimum/optimal): 512 bytes / 512 bytes

		Disk /dev/mapper/centos-swap: 2147 MB, 2147483648 bytes, 4194304 sectors
		Units = sectors of 1 * 512 = 512 bytes
		Sector size (logical/physical): 512 bytes / 512 bytes
		I/O size (minimum/optimal): 512 bytes / 512 bytes
		[root@localhost ~]#
2. 硬盘操作
		[root@localhost ~]# fdisk /dev/sda 		操作 /dev/sda 的分区表
		Welcome to fdisk (util-linux 2.23.2).

		Changes will remain in memory only, until you decide to write them.
		Be careful before using the write command.

		Command (m for help): p		查看已分区数量（看到有 sda1 sda2 sda3）

		Disk /dev/sda: 75.2 GB, 75161927680 bytes, 146800640 sectors
		Units = sectors of 1 * 512 = 512 bytes
		Sector size (logical/physical): 512 bytes / 512 bytes
		I/O size (minimum/optimal): 512 bytes / 512 bytes
		Disk label type: dos
		Disk identifier: 0x00095bf4

		   Device Boot      Start         End      Blocks   Id  System
		/dev/sda1   *        2048     1026047      512000   83  Linux
		/dev/sda2         1026048    41943039    20458496   8e  Linux LVM
		/dev/sda3        41943040   104857599    31457280   8e  Linux LVM

		Command (m for help): n		新增加一个分区
		Partition type:
		   p   primary (3 primary, 0 extended, 1 free)
		   e   extended
		Select (default e): p		分区类型选择为主分区
		Selected partition 4		分区号选4（因为1,2,3已经用过了，见上）
		First sector (104857600-146800639, default 104857600): 		回车　默认（起始扇区）
		Using default value 104857600
		Last sector, +sectors or +size{K,M,G} (104857600-146800639, default 146800639): 		回车　默认（结束扇区）
		Using default value 146800639
		Partition 4 of type Linux and of size 20 GiB is set

		Command (m for help): t		修改分区类型
		Partition number (1-4, default 4): 4		选分区4
		Hex code (type L to list all codes): L		

		 0  Empty           24  NEC DOS         81  Minix / old Lin bf  Solaris        
		 1  FAT12           27  Hidden NTFS Win 82  Linux swap / So c1  DRDOS/sec (FAT-
		 2  XENIX root      39  Plan 9          83  Linux           c4  DRDOS/sec (FAT-
		 3  XENIX usr       3c  PartitionMagic  84  OS/2 hidden C:  c6  DRDOS/sec (FAT-
		 4  FAT16 <32M      40  Venix 80286     85  Linux extended  c7  Syrinx         
		 5  Extended        41  PPC PReP Boot   86  NTFS volume set da  Non-FS data    
		 6  FAT16           42  SFS             87  NTFS volume set db  CP/M / CTOS / .
		 7  HPFS/NTFS/exFAT 4d  QNX4.x          88  Linux plaintext de  Dell Utility   
		 8  AIX             4e  QNX4.x 2nd part 8e  Linux LVM       df  BootIt         
		 9  AIX bootable    4f  QNX4.x 3rd part 93  Amoeba          e1  DOS access     
		 a  OS/2 Boot Manag 50  OnTrack DM      94  Amoeba BBT      e3  DOS R/O        
		 b  W95 FAT32       51  OnTrack DM6 Aux 9f  BSD/OS          e4  SpeedStor      
		 c  W95 FAT32 (LBA) 52  CP/M            a0  IBM Thinkpad hi eb  BeOS fs        
		 e  W95 FAT16 (LBA) 53  OnTrack DM6 Aux a5  FreeBSD         ee  GPT            
		 f  W95 Ext''d (LBA) 54  OnTrackDM6      a6  OpenBSD         ef  EFI (FAT-12/16/
		10  OPUS            55  EZ-Drive        a7  NeXTSTEP        f0  Linux/PA-RISC b
		11  Hidden FAT12    56  Golden Bow      a8  Darwin UFS      f1  SpeedStor      
		12  Compaq diagnost 5c  Priam Edisk     a9  NetBSD          f4  SpeedStor      
		14  Hidden FAT16 <3 61  SpeedStor       ab  Darwin boot     f2  DOS secondary  
		16  Hidden FAT16    63  GNU HURD or Sys af  HFS / HFS+      fb  VMware VMFS    
		17  Hidden HPFS/NTF 64  Novell Netware  b7  BSDI fs         fc  VMware VMKCORE 
		18  AST SmartSleep  65  Novell Netware  b8  BSDI swap       fd  Linux raid auto
		1b  Hidden W95 FAT3 70  DiskSecure Mult bb  Boot Wizard hid fe  LANstep        
		1c  Hidden W95 FAT3 75  PC/IX           be  Solaris boot    ff  BBT            
		1e  Hidden W95 FAT1 80  Old Minix      
		Hex code (type L to list all codes): 8e		修改为LVM（8e就是LVM）
		Changed type of partition 'Linux' to 'Linux LVM'

		Command (m for help): w		写分区表
		The partition table has been altered!

		Calling ioctl() to re-read partition table.
		系统提示你重启，重启吧
		WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
		The kernel still uses the old table. The new table will be used at
		the next reboot or after you run partprobe(8) or kpartx(8)
		Syncing disks.
		[root@localhost ~]#

3. 开机后，格式化：
```shell
    [root@localhost ~]# mkfs.xfs /dev/sda4
    meta-data=/dev/sda4              isize=256    agcount=4, agsize=1310720 blks
             =                       sectsz=512   attr=2, projid32bit=1
             =                       crc=0        finobt=0
    data     =                       bsize=4096   blocks=5242880, imaxpct=25
             =                       sunit=0      swidth=0 blks
    naming   =version 2              bsize=4096   ascii-ci=0 ftype=0
    log      =internal log           bsize=4096   blocks=2560, version=2
             =                       sectsz=512   sunit=0 blks, lazy-count=1
    realtime =none                   extsz=4096   blocks=0, rtextents=0
```

4.查看虚拟卷：
```shell
[root@localhost ~]# vgdisplay 		显示LVM卷组的信息
  --- Volume group ---
  VG Name               centos
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  5
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               49.50 GiB
  PE Size               4.00 MiB
  Total PE              12673
  Alloc PE / Size       12639 / 49.37 GiB
  Free  PE / Size       34 / 136.00 MiB
  VG UUID               E0k319-HVL2-rMxo-Zccc-XM9k-aCt8-WHyAIC
```
### 三、添加新LVM到已有的LVM组，实现扩容
```shell
[root@localhost ~]# lvm		进入lvm管理
lvm> pvcreate /dev/sda4		初始化刚才的分区，必须的
  Physical volume "/dev/sda4" successfully created
lvm> vgextend centos /dev/sda4		将初始化过的分区加入到虚拟卷组centos（通过vgdisplay查看）
  Volume group "centos" successfully extended
lvm> lvextend -l +100%FREE /dev/mapper/centos-root		扩展已有卷的容量（+100%FREE标识所有空间）
  Size of logical volume centos/root changed from 47.37 GiB (12127 extents) to 67.50 GiB (17280 extents).
  Logical volume root successfully resized
lvm> pvdisplay		查看卷容量
  --- Physical volume ---
  PV Name               /dev/sda2
  VG Name               centos
  PV Size               19.51 GiB / not usable 3.00 MiB
  Allocatable           yes (but full)
  PE Size               4.00 MiB
  Total PE              4994
  Free PE               0
  Allocated PE          4994
  PV UUID               TMVS9b-MYr9-BY81-0ueL-firn-I1dv-qWpMux
   
  --- Physical volume ---
  PV Name               /dev/sda3
  VG Name               centos
  PV Size               30.00 GiB / not usable 4.00 MiB
  Allocatable           yes (but full)
  PE Size               4.00 MiB
  Total PE              7679
  Free PE               0
  Allocated PE          7679
  PV UUID               nZaZah-Ly08-rbDu-xVvM-nnTh-5Pjs-djNRyw
   
  --- Physical volume ---
  PV Name               /dev/sda4
  VG Name               centos
  PV Size               20.00 GiB / not usable 4.00 MiB
  Allocatable           yes (but full)
  PE Size               4.00 MiB
  Total PE              5119
  Free PE               0
  Allocated PE          5119
  PV UUID               LpItEO-OBwa-ohf9-196h-OsAg-KdyZ-Uz8Nz7
   
lvm> quit		退出lvm管理
  Exiting.
[root@localhost ~]#
```

### 四、文件系统的真正扩容
```shell
[root@localhost ~]# xfs_growfs /dev/centos/root 
meta-data=/dev/mapper/centos-root isize=256    agcount=11, agsize=1144832 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=0        finobt=0
data     =                       bsize=4096   blocks=12418048, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=0
log      =internal               bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 12418048 to 17694720
```
#### 验证结果
可以看到，`/`挂载点已经成功扩容至68G
```shell
[root@localhost ~]# df -h
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root   68G  4.7G   63G   7% /
devtmpfs                 481M     0  481M   0% /dev
tmpfs                    490M     0  490M   0% /dev/shm
tmpfs                    490M  6.7M  484M   2% /run
tmpfs                    490M     0  490M   0% /sys/fs/cgroup
/dev/sda1                497M  124M  374M  25% /boot
[root@localhost ~]#
```

**参考文献**
 lvextend -l +100%FREE lvname
 https://access.redhat.com/solutions/44089
