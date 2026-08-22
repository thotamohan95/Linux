# Process Management, System Monitoring, Networking & Disk Management (Beginner's Guide)

## Introduction
This document covers basic Linux commands and concepts for:
- Process management
- System monitoring (CPU, memory, disk, network)
- Networking commands
- Disk and storage management (including LVM and swap)

Each section shows common commands and short explanations so beginners can quickly learn and use them.

---

## Table of Contents
1. Process Management
   - Viewing processes
   - Managing processes
   - Background & foreground jobs
   - Daemon/service management
2. System Monitoring
   - CPU & memory
   - Disk
   - Network
   - Logs
3. Networking Commands
4. Disk & Storage Management
   - Viewing disks
   - Partitioning & formatting
   - Mounting
   - LVM basics
   - Swap management
5. Quick Reference

---

## 1. Process Management

### What is a process?
A process is an instance of a running program. Each process has a Process ID (PID) and other attributes such as user, CPU/memory usage, and priority.

### Viewing processes
- `ps aux` — show all running processes.
- `ps -u username` — show processes for a specific user.
- `ps -C processname` — show processes by command name.
- `pgrep processname` — print PIDs matching a name.
- `pidof processname` — show PID(s) of a program.

Examples:
```bash
ps aux
pgrep sshd
pidof bash
```

### Managing processes (stop/kill)
- `kill PID` — send SIGTERM (ask process to terminate).
- `kill -9 PID` — send SIGKILL (force kill; cannot be trapped).
- `pkill processname` — kill processes by name.
- `pkill -9 processname` — force-kill by name.

Examples:
```bash
kill 1234
pkill firefox
```

### Stop & resume
- `kill -STOP PID` — suspend a process.
- `kill -CONT PID` — resume a suspended process.

### Changing process priority
- `nice -n 10 command` — run `command` with lower priority (higher nice value).
- `renice -n 10 -p PID` — change priority of an existing process (positive value = lower priority).
- Use negative nice values (e.g., `-5`) only as root to increase priority.

Check priority in `top` (NI column).

### Background & foreground jobs (shell jobs)
- `command &` — run in background.
- `jobs` — list background jobs in the current shell.
- `fg %jobnumber` — bring a job to foreground.
- `bg %jobnumber` — resume suspended job in background.
- `Ctrl+Z` — suspend current foreground job.

Example:
```bash
sleep 100 &
jobs
fg %1
```

### Interactive viewers
- `top` — interactive system/process viewer. Commands inside `top`: `k` (kill), `r` (renice), `q` (quit).
- `htop` — friendlier alternative (install with your package manager).

### Daemon / Service management (systemd)
- `systemctl list-units --type=service` — list services.
- `systemctl start service-name` — start a service now.
- `systemctl stop service-name` — stop a service.
- `systemctl enable service-name` — enable service at boot.
- `systemctl status service-name` — check status.

Example:
```bash
sudo systemctl status sshd
sudo systemctl start nginx
```

---

## 2. System Monitoring

### CPU & memory
- `top` / `htop` — real-time view of processes and resource usage.
- `vmstat 1 5` — report CPU, memory, I/O statistics (sample every 1 sec, 5 times).
- `free -m` — show memory usage in megabytes.
- `uptime` — load averages (system load).

Examples:
```bash
free -m
vmstat 1 5
```

### Disk space & I/O
- `df -h` — disk free space (human-readable).
- `du -sh /path` — disk usage for a directory.
- `iostat` — CPU and disk I/O statistics (install `sysstat` package on many distros).

Examples:
```bash
df -h
du -sh /var/log
iostat -x 1 3
```

### Network monitoring
- `ip a` — show network interfaces and addresses.
- `ss -tulnp` — show listening sockets and processes (preferred over `netstat`).
- `netstat -tulnp` — older tool (may be deprecated).
- `ping host` — test connectivity.
- `traceroute host` — show route to host.
- `curl https://example.com` — fetch a web resource.

Examples:
```bash
ip a
ss -tulnp
ping -c 4 google.com
```

### Logs
- `tail -f /var/log/syslog` — follow syslog (path depends on distro).
- `journalctl -f` — follow systemd journal logs.
- `dmesg | tail` — view kernel messages.

Examples:
```bash
sudo journalctl -f
tail -n 200 /var/log/syslog
```

---

## 3. Networking Commands (Common)
- `ping google.com` — check connectivity.
- `ip a` — show interface addresses.
- `ss -tulnp` — show listening ports and associated processes.
- `curl https://example.com` — fetch webpage.
- `wget https://example.com/file.zip` — download file.

DNS troubleshooting:
- `nslookup example.com` or `dig example.com` (if installed)

---

## 4. Disk & Storage Management

### Viewing disks & partitions
- `lsblk` — list block devices and mountpoints.
- `fdisk -l` — list partition tables (requires sudo).
- `blkid` — show UUIDs and file system types.

Example:
```bash
lsblk
sudo fdisk -l
sudo blkid
```

### Partitioning (fdisk) — when disk is new
1. Check disks: `lsblk`
2. Partition: `sudo fdisk /dev/sdX` (follow interactive prompts: `n` = new partition, `w` = write)
3. Format the partition: `sudo mkfs.ext4 /dev/sdX1`

Example:
```bash
sudo fdisk /dev/sdb
sudo mkfs.ext4 /dev/sdb1
```

Notes:
- Use `parted` for GPT disks or when you prefer non-interactive commands.
- Replace `/dev/sdX` with the correct device (always double-check to avoid data loss).

### Mounting & unmounting
- Mount: `sudo mount /dev/sdX1 /mnt`
- Unmount: `sudo umount /mnt`
- Remount read-write: `sudo mount -o remount,rw /mnt`

Example:
```bash
sudo mkdir -p /mnt/mydisk
sudo mount /dev/sdb1 /mnt/mydisk
sudo umount /mnt/mydisk
```

### LVM (Logical Volume Manager) basics
- Create physical volume: `pvcreate /dev/sdX`
- Create volume group: `vgcreate vg_name /dev/sdX`
- Create logical volume: `lvcreate -L 10G -n lv_name vg_name`
- Format logical volume: `mkfs.ext4 /dev/vg_name/lv_name`
- Mount: `mount /dev/vg_name/lv_name /mnt`

Example:
```bash
sudo pvcreate /dev/sdb1
sudo vgcreate data_vg /dev/sdb1
sudo lvcreate -L 10G -n data_lv data_vg
sudo mkfs.ext4 /dev/data_vg/data_lv
sudo mount /dev/data_vg/data_lv /data
```

### Swap management
- Create swap partition: `mkswap /dev/sdX2`
- Enable swap: `swapon /dev/sdX2`
- Disable swap: `swapoff /dev/sdX2`

Example:
```bash
sudo mkswap /dev/sdb2
sudo swapon /dev/sdb2
```

---

## 5. Quick Reference (Commands at a Glance)

Process
- View all: `ps aux`
- Interactive: `top` / `htop`
- Kill: `kill PID`, `pkill name`
- Background jobs: `command &`, `jobs`, `fg`, `bg`

System Monitoring
- Memory: `free -m`
- I/O: `iostat`
- Disk usage: `df -h`, `du -sh /path`

Network
- Interfaces: `ip a`
- Listening ports: `ss -tulnp`
- Connectivity: `ping`, `traceroute`, `curl`

Disk & Storage
- List disks: `lsblk`
- Partition: `fdisk /dev/sdX`
- Format: `mkfs.ext4 /dev/sdX1`
- Mount: `mount /dev/sdX1 /mnt`

---

## Safety Tips for Beginners
- Always run destructive commands (fdisk, mkfs, dd) as root only after double-checking device names.
- Use `lsblk` and `blkid` to confirm which device is which.
- Back up important data before making partition or filesystem changes.
- Use `--help` (e.g., `fdisk --help`) or man pages (`man fdisk`) if unsure.

---

## Conclusion
This guide provides the essential commands and workflows for basic process, monitoring, networking, and disk tasks in Linux. Practice using these commands in a safe environment (VM or test machine) before running them on production systems.
