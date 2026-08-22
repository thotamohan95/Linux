# Process Management in Linux

## Introduction to Process Management
A process is an instance of a running program. Linux provides multiple utilities to monitor, manage, and control processes effectively. Each process has a unique **Process ID (PID)** and belongs to a parent process.

## Index of Commands Covered

### Viewing Processes
- `ps aux` – View all running processes
- `ps -u username` – View processes for a specific user
- `ps -C processname` – Show a process by name
- `pgrep processname` – Find a process by name and return its PID
- `pidof processname` – Find the PID of a running program

### Managing Processes
- `kill PID` – Terminate a process by PID
- `pkill processname` – Terminate a process by name
- `kill -9 PID` – Force kill a process
- `pkill -9 processname` – Kill all instances of a process
- `kill -STOP PID` – Stop a running process
- `kill -CONT PID` – Resume a stopped process
- `renice -n 10 -p PID` – Lower priority of a process
- `renice -n -5 -p PID` – Increase priority of a process (requires root)

### Background & Foreground Processes
- `command &` – Run a command in the background
- `jobs` – List background jobs
- `fg %jobnumber` – Bring a job to the foreground
- `Ctrl + Z` – Suspend a running process
- `bg %jobnumber` – Resume a suspended process in the background

### Monitoring System Processes
- `top` – Interactive process viewer
- `htop` – User-friendly process viewer (requires installation)
- `nice -n 10 command` – Run a command with a specific priority
- `renice -n -5 -p PID` – Change priority of an existing process

### Daemon Process Management
- `systemctl list-units --type=service` – List all system daemons
- `systemctl start service-name` – Start a daemon/service
- `systemctl stop service-name` – Stop a daemon/service
- `systemctl enable service-name` – Enable a service at startup

## Viewing Process Details
### Using `ps`
Show processes for a specific user:
```bash
ps -u username
```
Show a process by name:
```bash
ps -C processname
```

### Using `pgrep`
Find a process by name and return its PID:
```bash
pgrep processname
```

### Using `pidof`
Find the PID of a running program:
```bash
pidof processname
```

## Managing Processes
### Killing Processes
To terminate a process by PID:
```bash
kill PID
```
To terminate using process name:
```bash
pkill processname
```
Force kill a process:
```bash
kill -9 PID
```
Kill all instances of a process:
```bash
pkill -9 processname
```

### Stopping & Resuming Processes
Stop a running process:
```bash
kill -STOP PID
```
Resume a stopped process:
```bash
kill -CONT PID
```

### Changing Process Priority
View process priorities:
```bash
top  # Look at the NI column
```
Change priority of a running process:
```bash
renice -n 10 -p PID  # Lower priority (positive values)
renice -n -5 -p PID  # Higher priority (negative values, root required)
```

### Running Processes in the Background
Run a command in the background:
```bash
command &
```
List background jobs:
```bash
jobs
```
Bring a job to the foreground:
```bash
fg %jobnumber
```
Send a running process to the background:
```bash
Ctrl + Z  # Suspend process
bg %jobnumber  # Resume in background
```

## Monitoring System Processes
### Using `top`
Interactive process viewer:
- Press `k` and enter a PID to kill a process.
- Press `r` to renice a process.
- Press `q` to quit.

### Using `htop`
A user-friendly alternative to `top`:
```bash
htop
```
Allows mouse-based interaction for process management.

### Using `nice` & `renice`
Run a command with a specific priority:
```bash
nice -n 10 command
```
Change the priority of an existing process:
```bash
renice -n -5 -p PID
```

## Daemon Processes
Daemon processes run in the background without user intervention.
List all system daemons:
```bash
systemctl list-units --type=service
```
Start a daemon:
```bash
systemctl start service-name
```
Stop a daemon:
```bash
systemctl stop service-name
```
Enable a service at startup:
```bash
systemctl enable service-name
```

## Conclusion
Process management is crucial for system performance and stability. By using tools like `ps`, `top`, `htop`, `kill`, and `nice`, you can efficiently control and monitor Linux processes.

### Linux System Monitoring
Introduction to System Monitoring
Monitoring system resources is essential to ensure optimal performance, detect issues, and troubleshoot problems in Linux. Various tools allow us to monitor CPU, memory, disk usage, network activity, and running processes.

Index of Commands Covered
CPU and Memory Monitoring
top – Real-time system monitoring
htop – Interactive process viewer (requires installation)
vmstat – Report system performance statistics
free -m – Show memory usage
Disk Monitoring
df -h – Check disk space usage
du -sh /path – Show disk usage of a specific directory
iostat – Display CPU and disk I/O statistics
Network Monitoring
ifconfig – Show network interfaces (deprecated, use ip a)
ip a – Show network interface details
netstat -tulnp – Show active connections and listening ports
ss -tulnp – Alternative to netstat for socket statistics
ping hostname – Test network connectivity
traceroute hostname – Show network path to a host
nslookup domain – Get DNS resolution details
Log Monitoring
tail -f /var/log/syslog – Live monitoring of system logs
journalctl -f – Live system logs for systemd-based distros
dmesg | tail – View kernel logs
CPU and Memory Monitoring
Using top
To view real-time CPU and memory usage:

top
Press q to quit.

Using htop
A user-friendly alternative:

htop
Use arrow keys to navigate and F9 to kill processes.

Using vmstat
To check CPU, memory, and I/O stats:

vmstat 1 5  # Update every 1 sec, show 5 updates
Checking Memory Usage
free -m
Shows free and used memory in megabytes.

Disk Monitoring
Using df
Check available disk space:

df -h
Using du
Find the size of a directory:

du -sh /var/log
Using iostat
Check disk and CPU usage:

iostat
Network Monitoring
Checking Network Interfaces
ip a  # Show IP addresses and interfaces
Viewing Open Ports and Connections
netstat -tulnp  # Show listening ports
ss -tulnp  # Alternative to netstat
Testing Connectivity
ping google.com  # Test internet connection
traceroute google.com  # Trace the path to Google
Checking DNS Resolution
nslookup example.com
Log Monitoring
Live Monitoring of System Logs
tail -f /var/log/syslog  # Follow logs in real-time
journalctl -f  # Systemd logs
Checking Kernel Logs
dmesg | tail


### Networking Commands
ping google.com – Checks connectivity to a remote server.
ifconfig – Displays network interfaces (deprecated, use ip).
ip a – Shows IP addresses of network interfaces.
netstat -tulnp – Displays open network connections.
curl https://example.com – Fetches a webpage's content.
wget https://example.com/file.zip – Downloads a file from the internet.

### Disk and Storage Management in Linux
Introduction to Disk and Storage Management
Managing disks and storage efficiently is crucial for system performance and stability. Linux provides various commands to monitor, partition, format, mount, and manage disk storage.

Index of Commands Covered
Viewing Disk Information
lsblk – Display block devices
fdisk -l – List disk partitions
blkid – Show UUIDs of devices
df -h – Check disk space usage
du -sh /path – Show size of a directory
Partition Management
fdisk /dev/sdX – Create and manage partitions
parted /dev/sdX – Alternative to fdisk for GPT disks
mkfs.ext4 /dev/sdX1 – Format a partition as ext4
mkfs.xfs /dev/sdX1 – Format a partition as XFS
Mounting and Unmounting
mount /dev/sdX1 /mnt – Mount a partition
umount /mnt – Unmount a partition
mount -o remount,rw /mnt – Remount a partition as read-write
Logical Volume Management (LVM)
pvcreate /dev/sdX – Create a physical volume
vgcreate vg_name /dev/sdX – Create a volume group
lvcreate -L 10G -n lv_name vg_name – Create a logical volume
mkfs.ext4 /dev/vg_name/lv_name – Format an LVM partition
mount /dev/vg_name/lv_name /mnt – Mount an LVM partition
Swap Management
mkswap /dev/sdX – Create a swap partition
swapon /dev/sdX – Enable swap space
swapoff /dev/sdX – Disable swap space
Viewing Disk Information
Using lsblk
List all block devices:

lsblk
Using fdisk
View partition details:

fdisk -l
Using df
Check available disk space:

df -h
Using du
Find the size of a directory:

du -sh /var/log
Partition Management
Creating a Partition with fdisk
fdisk /dev/sdX
Follow the interactive prompts to create a partition.

Formatting a Partition
Format as ext4:

mkfs.ext4 /dev/sdX1
Format as XFS:

mkfs.xfs /dev/sdX1
Mounting and Unmounting
Mount a Partition
mount /dev/sdX1 /mnt
Unmount a Partition
umount /mnt
Remount a Partition
mount -o remount,rw /mnt
LVM Management
Create a Physical Volume
pvcreate /dev/sdX
Create a Volume Group
vgcreate vg_name /dev/sdX
Create a Logical Volume
lvcreate -L 10G -n lv_name vg_name
Format and Mount the Logical Volume
mkfs.ext4 /dev/vg_name/lv_name
mount /dev/vg_name/lv_name /mnt
Swap Management
Create a Swap Partition
mkswap /dev/sdX
Enable Swap
swapon /dev/sdX
Disable Swap
swapoff /dev/sdX
Additional Notes - When to Use fdisk, mount, or Both
Check Available Disks
Before creating or mounting anything, always check what block devices exist:

lsblk
Example output:
NAME	MAJ:MIN	RM	SIZE	RO	TYPE	MOUNTPOINT
sda	8:0	0	100G	0	disk	
├─sda1	8:1	0	96G	0	part	/
└─sda2	8:2	0	4G	0	part	[SWAP]
sdb	8:16	0	20G	0	disk	
sda → existing disk (already partitioned)

sdb → new disk, no partitions yet

When to use fdisk
Use fdisk when:

The disk is brand new and has no partitions
You want to create /dev/sdb1, /dev/sdb2, etc.
Inside fdisk:

Press n → create a new partition
Press w → write changes
Then confirm:

lsblk
When to Use mount
Use mount when: The partition already exists and is formatted You just want to make it accessible

sudo mkdir /mnt/mydisk
sudo mount /dev/sdb1 /mnt/mydisk
Now your disk is available at /mnt/mydisk.

When to Use fdisk + mount (Full Setup)
Use fdisk + mkfs + mount when: The disk is completely new You need to partition → format → mount it

# 1. Check available disks
lsblk
# 2. Create partition
sudo fdisk /dev/sdb
# 3. Format the partition
sudo mkfs.ext4 /dev/sdb1
# 4. Mount it
sudo mkdir /data
sudo mount /dev/sdb1 /data
Quick Reference
Use Case	Command(s)
View disks and partitions	lsblk
Partition a new disk	fdisk
Mount an existing partition	mount
Full setup (new disk)	fdisk + mkfs + mount
