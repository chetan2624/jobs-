```markdown
# 🐧 Complete Linux Commands Reference Guide (Part 2)

## 📚 Table of Contents

- [Disk, Storage & Filesystem Management](#disk-storage--filesystem-management)
- [Boot & System Startup](#boot--system-startup)
- [Service Management (systemd)](#service-management-systemd)
- [Networking & Internet](#networking--internet)
- [Hardware & System Information](#hardware--system-information)
- [Archive & Compression](#archive--compression)
- [Shell & Environment](#shell--environment)

---

## Disk, Storage & Filesystem Management

### `df`

**What it is:**
- Reports filesystem disk space usage across all mounted filesystems
- Shows available and used space on mounted partitions and drives

**What it does:**
- Displays total size, used space, available space, and usage percentage for each filesystem
- Can show filesystem types, inode usage, and human-readable output formats

**When to use:**
- Checking how much disk space is available before installing software or storing data
- Identifying which filesystems are running out of space during troubleshooting

**Popular flags with examples:**
```bash
df                              # Shows disk space for all mounted filesystems
df -h                           # Human-readable format (GB, MB instead of bytes)
df -T                           # Shows filesystem type (ext4, xfs, etc.)
df -i                           # Shows inode usage instead of block usage
df -h /home                     # Shows disk space for specific mount point
df -h --total                   # Includes total row summarizing all filesystems
df -h -t ext4                   # Shows only ext4 filesystems
df -h --exclude-type=tmpfs      # Excludes tmpfs filesystems from output
df -a                           # Shows all filesystems including pseudo filesystems
df -h .                         # Shows disk space for filesystem containing current directory
```

---

### `du`

**What it is:**
- Estimates file and directory space usage
- Shows disk space consumed by files and folders

**What it does:**
- Calculates and displays disk usage for specified files and directories recursively
- Can summarize usage, show human-readable sizes, and sort by size

**When to use:**
- Finding which directories are consuming the most disk space
- Identifying large files or folders for cleanup or archival

**Popular flags with examples:**
```bash
du                              # Shows disk usage for current directory and subdirectories
du -h                           # Human-readable format (KB, MB, GB)
du -sh                          # Summarizes total for directory (doesn't show subdirectories)
du -sh *                        # Shows size of each item in current directory
du -ah                          # Shows sizes for all files (not just directories)
du -h --max-depth=1             # Shows only one level of subdirectories
du -h --max-depth=1 | sort -hr  # Shows directories sorted by size (largest first)
du -ch *.log                    # Shows individual sizes and total for all .log files
du -sh /var/log                 # Shows total size of /var/log directory
du -h --threshold=100M          # Shows only items larger than 100MB
du -h --exclude="*.log"         # Excludes .log files from calculation
du -h --time                    # Shows last modification time alongside size
```

---

### `mount`

**What it is:**
- Mounts filesystems and storage devices to the directory tree
- Makes filesystems accessible by attaching them to mount points

**What it does:**
- Attaches filesystems from devices (hard drives, USB, network shares) to directories
- Can mount with specific options like read-only, permissions, and filesystem types

**When to use:**
- Making USB drives or external hard drives accessible in the filesystem
- Mounting network shares or ISO images for access

**Popular flags with examples:**
```bash
mount                           # Lists all currently mounted filesystems
sudo mount /dev/sdb1 /mnt       # Mounts device /dev/sdb1 to /mnt directory
sudo mount -t ext4 /dev/sdc1 /media/disk  # Mounts with specific filesystem type
sudo mount -o ro /dev/sdb1 /mnt # Mounts as read-only
sudo mount -o remount,rw /mnt   # Remounts filesystem as read-write
sudo mount /dev/cdrom /media/cdrom  # Mounts CD/DVD drive
sudo mount -o loop disk.iso /mnt/iso  # Mounts ISO image file
sudo mount -t cifs //server/share /mnt -o username=user  # Mounts Windows network share
sudo mount -a                   # Mounts all filesystems in /etc/fstab
mount | grep /home              # Shows mount information for /home
sudo mount -o uid=1000,gid=1000 /dev/sdb1 /mnt  # Mounts with specific user/group ownership
```

---

### `umount`

**What it is:**
- Unmounts filesystems from the directory tree
- Detaches mounted filesystems making devices safe to remove

**What it does:**
- Safely disconnects filesystems ensuring all data is written to disk
- Prevents data corruption by ensuring no processes are accessing the filesystem

**When to use:**
- Safely removing USB drives or external storage devices
- Unmounting network shares or ISO images after use

**Popular flags with examples:**
```bash
sudo umount /mnt                # Unmounts filesystem at /mnt
sudo umount /dev/sdb1           # Unmounts device by device name
sudo umount -l /mnt             # Lazy unmount: detaches when no longer busy
sudo umount -f /mnt             # Force unmount (use with caution)
sudo umount -a                  # Unmounts all filesystems in /etc/fstab
sudo umount -t cifs -a          # Unmounts all CIFS (network) filesystems
lsof /mnt                       # Shows which processes are using /mnt (before unmounting)
fuser -m /mnt                   # Shows processes preventing unmount
```

---

### `fsck`

**What it is:**
- Filesystem consistency check and repair utility
- Verifies and repairs filesystem errors and corruption

**What it does:**
- Scans filesystems for errors, inconsistencies, and corruption
- Can automatically repair certain types of filesystem damage

**When to use:**
- Repairing filesystem corruption after system crashes or power failures
- Performing routine filesystem maintenance and verification

**Popular flags with examples:**
```bash
sudo fsck /dev/sdb1             # Checks and repairs filesystem on /dev/sdb1
sudo fsck -y /dev/sdb1          # Automatically answers 'yes' to all repair prompts
sudo fsck -n /dev/sdb1          # Checks without making any changes (dry run)
sudo fsck -A                    # Checks all filesystems in /etc/fstab
sudo fsck -C                    # Shows progress bar during check
sudo fsck -f /dev/sdb1          # Forces check even if filesystem appears clean
sudo fsck -t ext4 /dev/sdb1     # Specifies filesystem type
sudo fsck -p /dev/sdb1          # Automatically repairs without prompts (preen mode)
```

**Important:** Filesystem must be unmounted before running fsck, or checked during boot for root filesystem.

---

### `mkfs`

**What it is:**
- Creates a new filesystem on a device or partition
- Formats storage devices with specific filesystem types

**What it does:**
- Initializes filesystems on partitions, preparing them for data storage
- Supports various filesystem types like ext4, xfs, btrfs, vfat

**When to use:**
- Formatting new hard drives or partitions before first use
- Reformatting storage devices to change filesystem type

**Popular flags with examples:**
```bash
sudo mkfs.ext4 /dev/sdb1        # Creates ext4 filesystem on /dev/sdb1
sudo mkfs -t ext4 /dev/sdb1     # Same as above using -t flag
sudo mkfs.xfs /dev/sdc1         # Creates XFS filesystem
sudo mkfs.vfat /dev/sdd1        # Creates FAT filesystem (for USB drives)
sudo mkfs.ext4 -L MyDisk /dev/sdb1  # Creates filesystem with label "MyDisk"
sudo mkfs.ext4 -m 1 /dev/sdb1   # Sets reserved blocks to 1% (default is 5%)
sudo mkfs.ext4 -j /dev/sdb1     # Creates ext4 with journaling (default)
sudo mkfs.ext4 -F /dev/sdb1     # Forces creation even if device appears in use
sudo mkfs.btrfs /dev/sde1       # Creates Btrfs filesystem
```

**Warning:** This command destroys all existing data on the partition. Always backup data before formatting.

---

### `fdisk`

**What it is:**
- Partition table manipulator for managing disk partitions
- Creates, deletes, and modifies disk partition layouts

**What it does:**
- Displays current partition information and allows creating/deleting partitions
- Modifies partition types, sizes, and boot flags interactively

**When to use:**
- Partitioning new hard drives before creating filesystems
- Modifying existing partition layouts or viewing partition information

**Popular flags with examples:**
```bash
sudo fdisk -l                   # Lists all disk partitions on system
sudo fdisk -l /dev/sda          # Lists partitions on specific disk
sudo fdisk /dev/sdb             # Opens interactive mode for /dev/sdb

# Inside fdisk interactive mode:
# m    - Display help menu
# p    - Print partition table
# n    - Create new partition
# d    - Delete partition
# t    - Change partition type
# w    - Write changes and exit
# q    - Quit without saving changes
# l    - List known partition types

sudo fdisk -s /dev/sda1         # Shows size of partition in blocks
```

**Warning:** Changes take effect only after pressing 'w'. Be extremely careful as incorrect partitioning can cause data loss.

---

### `parted`

**What it is:**
- Advanced partition editor supporting GPT and MBR partition tables
- More powerful than fdisk with support for larger disks

**What it does:**
- Creates, resizes, moves, and deletes partitions on various partition table types
- Supports partitions larger than 2TB using GPT partition tables

**When to use:**
- Working with modern large disks (over 2TB) requiring GPT
- Resizing existing partitions without data loss

**Popular flags with examples:**
```bash
sudo parted -l                  # Lists all disk partitions
sudo parted /dev/sdb print      # Shows partition table for /dev/sdb
sudo parted /dev/sdb            # Opens interactive mode for /dev/sdb

# Inside parted interactive mode:
# print              - Display partition table
# mklabel gpt        - Create GPT partition table
# mklabel msdos      - Create MBR partition table
# mkpart primary ext4 0% 100%   - Create partition using entire disk
# rm 1               - Remove partition 1
# resizepart 1 50GB  - Resize partition 1 to 50GB
# quit               - Exit parted

sudo parted /dev/sdb mklabel gpt  # Creates GPT partition table (non-interactive)
sudo parted /dev/sdb mkpart primary ext4 0% 50%  # Creates partition using first half
```

---

### `lsblk`

**What it is:**
- Lists information about all block devices (disks and partitions)
- Displays device hierarchy showing partitions within disks

**What it does:**
- Shows block devices with their sizes, types, mount points in tree format
- Displays device relationships making it easy to understand disk layout

**When to use:**
- Quickly viewing all disks and partitions with their mount points
- Identifying device names before mounting or partitioning

**Popular flags with examples:**
```bash
lsblk                           # Shows all block devices in tree format
lsblk -f                        # Shows filesystem type, label, and UUID
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT  # Custom columns
lsblk -a                        # Shows all devices including empty ones
lsblk -p                        # Shows full device paths (/dev/sda instead of sda)
lsblk -b                        # Shows sizes in bytes instead of human-readable
lsblk -d                        # Shows only disk devices, not partitions
lsblk -e 7                      # Excludes device major number 7 (loop devices)
lsblk /dev/sda                  # Shows information for specific device
lsblk -J                        # Outputs in JSON format
```

---

### `blkid`

**What it is:**
- Displays block device attributes like UUID, filesystem type, and labels
- Shows unique identifiers for partitions and filesystems

**What it does:**
- Retrieves and displays UUIDs, filesystem types, labels for all block devices
- Helps identify devices by UUID for use in /etc/fstab

**When to use:**
- Finding UUID of partitions for permanent mounting in /etc/fstab
- Identifying filesystem types and labels of connected devices

**Popular flags with examples:**
```bash
sudo blkid                      # Shows all block devices with attributes
sudo blkid /dev/sda1            # Shows attributes for specific partition
sudo blkid -o list              # Displays in list format
sudo blkid -s UUID /dev/sda1    # Shows only UUID for device
sudo blkid -s TYPE /dev/sda1    # Shows only filesystem type
sudo blkid -t TYPE=ext4         # Finds all devices with ext4 filesystem
sudo blkid -L MyLabel           # Finds device by filesystem label
sudo blkid -U UUID-string       # Finds device by UUID
```

---

### `dd`

**What it is:**
- Copies and converts files at low level, byte by byte
- Powerful tool for disk cloning, backup, and data transfer

**What it does:**
- Reads from input file and writes to output file with optional conversions
- Can create disk images, write bootable USB drives, and backup partitions

**When to use:**
- Creating bootable USB drives from ISO images
- Backing up or cloning entire disks or partitions

**Popular flags with examples:**
```bash
sudo dd if=/dev/sda of=/dev/sdb bs=4M  # Clones entire disk sda to sdb
sudo dd if=/dev/sda1 of=backup.img  # Creates image backup of partition
sudo dd if=ubuntu.iso of=/dev/sdb bs=4M status=progress  # Creates bootable USB
sudo dd if=/dev/zero of=/dev/sdc bs=1M  # Wipes disk with zeros
sudo dd if=/dev/urandom of=random.dat bs=1M count=100  # Creates 100MB random data file
sudo dd if=/dev/sda of=mbr-backup.img bs=512 count=1  # Backs up MBR
sudo dd if=mbr-backup.img of=/dev/sda bs=512 count=1  # Restores MBR
sudo dd if=file.img of=/dev/sdb bs=4M conv=fsync  # Writes image with sync
```

**Warning:** dd is extremely powerful and dangerous. Wrong input/output device can destroy data. Always double-check device names.

---

### `hdparm`

**What it is:**
- Gets and sets hard disk parameters and performance settings
- Controls disk power management, read-ahead, and other features

**What it does:**
- Displays disk information like model, serial number, and capabilities
- Adjusts disk settings for performance tuning and power management

**When to use:**
- Checking disk performance and specifications
- Enabling or disabling disk features like write caching or power management

**Popular flags with examples:**
```bash
sudo hdparm -I /dev/sda         # Shows detailed disk information
sudo hdparm -t /dev/sda         # Tests read speed (buffered)
sudo hdparm -T /dev/sda         # Tests cache read speed
sudo hdparm -C /dev/sda         # Checks current power mode
sudo hdparm -y /dev/sda         # Puts drive into standby mode
sudo hdparm -Y /dev/sda         # Puts drive into sleep mode
sudo hdparm -S 120 /dev/sda     # Sets standby timeout (120 * 5 seconds)
sudo hdparm -B 127 /dev/sda     # Sets APM level (1=max power saving, 255=max performance)
sudo hdparm -W 1 /dev/sda       # Enables write caching
sudo hdparm -W 0 /dev/sda       # Disables write caching
```

---

### `smartctl`

**What it is:**
- Controls and monitors SMART (Self-Monitoring, Analysis and Reporting Technology) on drives
- Predicts disk failures by monitoring drive health

**What it does:**
- Displays SMART attributes, performs self-tests, and shows error logs
- Helps detect failing drives before complete failure

**When to use:**
- Monitoring disk health and predicting failures
- Running diagnostic tests on hard drives

**Popular flags with examples:**
```bash
sudo smartctl -a /dev/sda       # Shows all SMART information
sudo smartctl -H /dev/sda       # Shows overall health status
sudo smartctl -i /dev/sda       # Shows device information
sudo smartctl -A /dev/sda       # Shows SMART attributes
sudo smartctl -t short /dev/sda # Runs short self-test
sudo smartctl -t long /dev/sda  # Runs extended self-test
sudo smartctl -l error /dev/sda # Shows error log
sudo smartctl -l selftest /dev/sda  # Shows self-test results
sudo smartctl -s on /dev/sda    # Enables SMART on drive
sudo smartctl -s off /dev/sda   # Disables SMART on drive
```

**Note:** Requires smartmontools package: `sudo apt install smartmontools` or `sudo yum install smartmontools`

---

### `sync`

**What it is:**
- Flushes filesystem buffers, writing cached data to disk
- Forces synchronization of file data with storage devices

**What it does:**
- Ensures all pending write operations are committed to disk
- Guarantees data consistency before shutdown or device removal

**When to use:**
- Before unmounting filesystems or removing USB drives
- Ensuring critical data is written to disk immediately

**Usage examples:**
```bash
sync                            # Flushes all filesystem buffers
sync /path/to/file              # Syncs specific file to disk
sync && sudo umount /mnt        # Syncs then unmounts safely
```

---

## Boot & System Startup

### `systemctl` (boot management)

**What it is:**
- Controls systemd system and service manager including boot targets
- Manages system boot process and runlevel equivalents

**What it does:**
- Sets default boot targets, switches between targets, and manages boot services
- Controls what services start automatically during system boot

**When to use:**
- Changing default boot target (graphical vs text mode)
- Managing which services start at boot time

**Popular flags with examples:**
```bash
systemctl get-default           # Shows current default boot target
sudo systemctl set-default multi-user.target  # Sets text mode as default boot
sudo systemctl set-default graphical.target   # Sets graphical mode as default boot
sudo systemctl isolate rescue.target  # Switches to rescue mode
sudo systemctl isolate multi-user.target  # Switches to multi-user text mode
systemctl list-units --type=target  # Lists all available targets
sudo systemctl reboot           # Reboots the system
sudo systemctl poweroff         # Powers off the system
sudo systemctl suspend          # Suspends system to RAM
sudo systemctl hibernate        # Hibernates system to disk
systemctl list-dependencies graphical.target  # Shows target dependencies
```

---

### `grub-update` / `update-grub`

**What it is:**
- Updates GRUB bootloader configuration
- Regenerates GRUB menu based on current system configuration

**What it does:**
- Scans for installed operating systems and kernels to add to boot menu
- Creates new GRUB configuration file from /etc/default/grub settings

**When to use:**
- After installing new kernels or operating systems
- After modifying GRUB configuration settings

**Usage examples:**
```bash
sudo update-grub                # Updates GRUB configuration (Debian/Ubuntu)
sudo grub2-mkconfig -o /boot/grub2/grub.cfg  # Updates GRUB (RHEL/CentOS)
sudo update-grub2               # Alternative command on some systems
```

---

### `grub-install`

**What it is:**
- Installs GRUB bootloader to disk or partition
- Sets up bootloader in MBR or EFI partition

**What it does:**
- Writes GRUB boot code to specified disk allowing system to boot
- Configures bootloader for BIOS or UEFI systems

**When to use:**
- Installing GRUB after system installation or recovery
- Fixing bootloader after corruption or disk changes

**Popular flags with examples:**
```bash
sudo grub-install /dev/sda      # Installs GRUB to disk sda (MBR)
sudo grub-install --target=x86_64-efi --efi-directory=/boot/efi  # UEFI installation
sudo grub-install --recheck /dev/sda  # Forces device map update
sudo grub-install --boot-directory=/mnt/boot /dev/sda  # Custom boot directory
```

---

### `dmesg`

**What it is:**
- Displays kernel ring buffer messages from system boot and runtime
- Shows hardware detection, driver loading, and system events

**What it does:**
- Prints kernel messages including boot messages, hardware detection, errors
- Helps diagnose hardware issues and driver problems

**When to use:**
- Troubleshooting hardware detection problems during boot
- Viewing kernel errors and warnings for system diagnostics

**Popular flags with examples:**
```bash
dmesg                           # Shows all kernel messages
dmesg | less                    # Pages through kernel messages
dmesg -T                        # Shows human-readable timestamps
dmesg -H                        # Human-readable output with colors
dmesg --level=err,warn          # Shows only errors and warnings
dmesg -w                        # Watches for new messages (follows)
dmesg -C                        # Clears ring buffer
dmesg | grep -i usb             # Searches for USB-related messages
dmesg | grep -i error           # Searches for error messages
dmesg | tail -50                # Shows last 50 messages
sudo dmesg -n 1                 # Sets console log level
```

---

### `journalctl` (boot logs)

**What it is:**
- Queries systemd journal for system and service logs including boot logs
- Accesses structured logging data from systemd

**What it does:**
- Displays logs from current and previous boots with powerful filtering
- Shows boot-related messages for troubleshooting startup issues

**When to use:**
- Viewing logs from previous boots to diagnose startup failures
- Analyzing boot performance and service startup times

**Popular flags with examples:**
```bash
journalctl -b                   # Shows logs from current boot
journalctl -b -1                # Shows logs from previous boot
journalctl -b -2                # Shows logs from 2 boots ago
journalctl --list-boots         # Lists all available boot logs
journalctl -b <boot-id>         # Shows logs from specific boot
journalctl -k                   # Shows only kernel messages
journalctl -b -p err            # Shows only errors from current boot
journalctl --since="2024-01-01" --until="2024-01-02"  # Date range
journalctl -b --no-pager        # Shows all boot logs without paging
journalctl _COMM=systemd        # Shows systemd daemon messages
```

---

### `systemd-analyze`

**What it is:**
- Analyzes system boot performance and timing
- Shows which services slow down boot process

**What it does:**
- Displays boot time breakdown showing time spent in kernel, userspace, per service
- Generates visualization of boot process timeline

**When to use:**
- Optimizing boot time by identifying slow services
- Troubleshooting long boot times

**Popular flags with examples:**
```bash
systemd-analyze                 # Shows total boot time summary
systemd-analyze time            # Same as above
systemd-analyze blame           # Lists services by startup time (slowest first)
systemd-analyze critical-chain  # Shows critical path for boot time
systemd-analyze plot > boot.svg # Creates SVG visualization of boot
systemd-analyze dot | dot -Tsvg > boot-dependencies.svg  # Dependency graph
systemd-analyze verify unit.service  # Verifies service configuration
systemd-analyze security        # Shows security hardening status
```

---

### `init` / `telinit`

**What it is:**
- Changes system runlevel (legacy systems) or systemd target
- Controls system state and service configuration

**What it does:**
- Switches between different system states (runlevels/targets)
- Controls system initialization and shutdown processes

**When to use:**
- Changing system mode on legacy SysV init systems
- Emergency system state changes

**Usage examples:**
```bash
sudo init 0                     # Halts system (shutdown)
sudo init 1                     # Single-user mode (rescue)
sudo init 3                     # Multi-user text mode
sudo init 5                     # Multi-user graphical mode
sudo init 6                     # Reboots system
sudo telinit 3                  # Same as init 3
```

**Note:** On modern systemd systems, use `systemctl` instead.

---

## Service Management (systemd)

### `systemctl`

**What it is:**
- Central command for managing systemd services, targets, and system state
- Controls service lifecycle (start, stop, enable, disable)

**What it does:**
- Starts, stops, restarts, enables, and disables system services
- Shows service status, configuration, and dependencies

**When to use:**
- Managing system services like web servers, databases, and daemons
- Enabling or disabling services to start automatically at boot

**Popular flags with examples:**
```bash
sudo systemctl start nginx      # Starts nginx service
sudo systemctl stop nginx       # Stops nginx service
sudo systemctl restart nginx    # Restarts nginx service
sudo systemctl reload nginx     # Reloads configuration without stopping
sudo systemctl status nginx     # Shows service status and recent logs
sudo systemctl enable nginx     # Enables service to start at boot
sudo systemctl disable nginx    # Disables service from starting at boot
sudo systemctl enable --now nginx  # Enables and starts service immediately
sudo systemctl is-active nginx  # Checks if service is running
sudo systemctl is-enabled nginx # Checks if service is enabled
systemctl list-units --type=service  # Lists all services
systemctl list-units --state=failed  # Lists failed services
systemctl list-unit-files       # Lists all unit files and their states
sudo systemctl daemon-reload    # Reloads systemd configuration
systemctl show nginx            # Shows all service properties
systemctl cat nginx             # Shows service unit file content
sudo systemctl mask nginx       # Prevents service from being started
sudo systemctl unmask nginx     # Removes mask
```

---

### `service`

**What it is:**
- Legacy command for managing services (wraps systemctl on systemd systems)
- Provides backward compatibility with SysV init scripts

**What it does:**
- Starts, stops, restarts, and checks status of services
- Simpler syntax than systemctl for basic operations

**When to use:**
- Quick service management with familiar SysV-style commands
- Scripts requiring compatibility with both old and new systems

**Popular flags with examples:**
```bash
sudo service nginx start        # Starts nginx service
sudo service nginx stop         # Stops nginx service
sudo service nginx restart      # Restarts nginx service
sudo service nginx reload       # Reloads configuration
sudo service nginx status       # Shows service status
service --status-all            # Lists all services and their states
```

**Note:** On systemd systems, this is a wrapper for `systemctl`. Use `systemctl` for full functionality.

---

### `journalctl`

**What it is:**
- Queries and displays logs from systemd journal
- Centralized logging system for all services and system events

**What it does:**
- Displays logs with powerful filtering by service, time, priority, and more
- Shows structured log data with metadata for advanced troubleshooting

**When to use:**
- Viewing service logs for troubleshooting errors and issues
- Monitoring real-time log output from specific services

**Popular flags with examples:**
```bash
journalctl                      # Shows all log entries
journalctl -u nginx             # Shows logs for nginx service
journalctl -u nginx -f          # Follows nginx logs in real-time
journalctl -u nginx --since today  # Shows today's logs
journalctl -u nginx --since "2024-01-01" --until "2024-01-02"  # Date range
journalctl -p err               # Shows only error messages
journalctl -p warning..err      # Shows warnings and errors
journalctl -n 50                # Shows last 50 log entries
journalctl --disk-usage         # Shows disk space used by journal
journalctl --vacuum-size=100M   # Limits journal size to 100MB
journalctl --vacuum-time=7d     # Removes logs older than 7 days
journalctl -o json              # Outputs in JSON format
journalctl -o json-pretty       # Pretty-printed JSON output
journalctl _PID=1234            # Shows logs from specific process ID
journalctl /usr/bin/nginx       # Shows logs from specific binary
```

---

### `systemctl list-dependencies`

**What it is:**
- Shows dependency tree for systemd units
- Displays what services depend on or are required by other services

**What it does:**
- Lists all dependencies required for a unit to start successfully
- Shows reverse dependencies (what depends on the specified unit)

**When to use:**
- Understanding service dependencies during troubleshooting
- Planning service startup order and requirements

**Usage examples:**
```bash
systemctl list-dependencies nginx  # Shows nginx dependencies
systemctl list-dependencies nginx --reverse  # Shows what depends on nginx
systemctl list-dependencies nginx --all  # Shows all dependency levels
systemctl list-dependencies multi-user.target  # Shows target dependencies
```

---

### `systemctl edit`

**What it is:**
- Edits systemd service unit files using override mechanism
- Creates drop-in configuration files for customizing services

**What it does:**
- Opens editor to modify service configuration without changing original files
- Creates override files in /etc/systemd/system/ preserving upgradability

**When to use:**
- Customizing service configuration like resource limits or environment variables
- Modifying service behavior without editing distribution-provided files

**Usage examples:**
```bash
sudo systemctl edit nginx       # Creates override file for nginx
sudo systemctl edit --full nginx  # Edits full service file
sudo systemctl edit nginx --drop-in=custom  # Creates named drop-in file
sudo systemctl revert nginx     # Removes all overrides
```

---

### `systemctl show`

**What it is:**
- Displays detailed properties and configuration of systemd units
- Shows all settings and current values for services and targets

**What it does:**
- Outputs complete configuration and runtime information for units
- Can filter specific properties for scripting and monitoring

**When to use:**
- Inspecting detailed service configuration and current state
- Extracting specific properties for scripts or monitoring

**Usage examples:**
```bash
systemctl show nginx            # Shows all nginx service properties
systemctl show nginx -p MainPID # Shows only process ID property
systemctl show nginx -p ActiveState  # Shows only active state
systemctl show nginx --property=ExecStart  # Shows start command
```

---

### `systemctl daemon-reload`

**What it is:**
- Reloads systemd manager configuration
- Picks up changes to unit files without rebooting

**What it does:**
- Rescans unit files and updates systemd's internal state
- Necessary after modifying service files or adding new services

**When to use:**
- After creating or modifying service unit files
- When service changes aren't being recognized

**Usage examples:**
```bash
sudo systemctl daemon-reload    # Reloads systemd configuration
```

---

## Networking & Internet

### `ip`

**What it is:**
- Modern network configuration tool replacing older ifconfig command
- Manages network interfaces, addresses, routes, and tunnels

**What it does:**
- Displays and configures network interfaces, IP addresses, and routing tables
- Provides comprehensive network management capabilities

**When to use:**
- Configuring network interfaces and IP addresses
- Viewing and modifying routing tables and network settings

**Popular flags with examples:**
```bash
ip addr                         # Shows all network interfaces and IP addresses
ip addr show                    # Same as above
ip addr show eth0               # Shows info for specific interface
sudo ip addr add 192.168.1.100/24 dev eth0  # Assigns IP address
sudo ip addr del 192.168.1.100/24 dev eth0  # Removes IP address
ip link                         # Shows network interfaces and their states
sudo ip link set eth0 up        # Brings interface up
sudo ip link set eth0 down      # Brings interface down
ip route                        # Shows routing table
ip route show                   # Same as above
sudo ip route add default via 192.168.1.1  # Sets default gateway
sudo ip route add

10.0.0.0/8 via 192.168.1.254  # Adds static route
sudo ip route del 10.0.0.0/8    # Deletes route
ip neigh                        # Shows ARP table (neighbor cache)
ip -s link                      # Shows interface statistics
ip -4 addr                      # Shows only IPv4 addresses
ip -6 addr                      # Shows only IPv6 addresses
```

---

### `ifconfig`

**What it is:**
- Legacy network interface configuration tool
- Displays and configures network interface parameters

**What it does:**
- Shows network interface information including IP addresses and statistics
- Configures interface addresses and parameters (deprecated, use ip instead)

**When to use:**
- Quick viewing of network interface information on older systems
- Legacy scripts requiring ifconfig compatibility

**Popular flags with examples:**
```bash
ifconfig                        # Shows all active network interfaces
ifconfig -a                     # Shows all interfaces including inactive
ifconfig eth0                   # Shows specific interface information
sudo ifconfig eth0 192.168.1.100 netmask 255.255.255.0  # Sets IP address
sudo ifconfig eth0 up           # Brings interface up
sudo ifconfig eth0 down         # Brings interface down
ifconfig eth0 | grep inet       # Extracts IP address information
```

**Note:** `ifconfig` is deprecated. Use `ip` command instead for modern systems.

---

### `ping`

**What it is:**
- Tests network connectivity to hosts using ICMP echo requests
- Verifies if remote hosts are reachable and measures round-trip time

**What it does:**
- Sends ICMP packets to target host and reports responses and timing
- Helps diagnose network connectivity and latency issues

**When to use:**
- Testing if a host or website is reachable
- Measuring network latency and packet loss

**Popular flags with examples:**
```bash
ping google.com                 # Pings google.com continuously
ping -c 4 google.com            # Sends only 4 ping requests
ping -i 2 google.com            # Sets 2 second interval between pings
ping -s 1000 google.com         # Sends packets of 1000 bytes
ping -w 10 google.com           # Stops after 10 seconds
ping -q -c 10 google.com        # Quiet mode: shows only summary
ping -f google.com              # Flood ping (requires root, for testing)
ping 192.168.1.1                # Pings local gateway
ping -4 google.com              # Forces IPv4
ping -6 google.com              # Forces IPv6
```

---

### `traceroute`

**What it is:**
- Traces the route packets take to reach a destination host
- Shows all intermediate hops between your computer and target

**What it does:**
- Displays each router/hop along the path to destination with timing information
- Helps identify where network delays or failures occur

**When to use:**
- Diagnosing slow network connections to identify bottlenecks
- Troubleshooting network routing issues and unreachable hosts

**Popular flags with examples:**
```bash
traceroute google.com           # Traces route to google.com
traceroute -n google.com        # Shows IP addresses instead of hostnames
traceroute -m 15 google.com     # Sets maximum hops to 15
traceroute -q 2 google.com      # Sends 2 queries per hop (default 3)
traceroute -w 2 google.com      # Sets 2 second timeout per probe
traceroute -I google.com        # Uses ICMP instead of UDP
```

**Note:** Install with `apt install traceroute` or `yum install traceroute` if not available.

---

### `netstat`

**What it is:**
- Displays network connections, routing tables, interface statistics
- Shows active network connections and listening ports

**What it does:**
- Lists all TCP/UDP connections, listening ports, routing tables, and network statistics
- Helps identify what services are listening and what connections exist

**When to use:**
- Finding which ports are open and listening for connections
- Identifying active network connections and their states

**Popular flags with examples:**
```bash
netstat -tuln                   # Shows listening TCP/UDP ports with numbers
netstat -tun                    # Shows all TCP/UDP connections
netstat -a                      # Shows all connections and listening ports
netstat -r                      # Shows routing table
netstat -i                      # Shows network interface statistics
netstat -s                      # Shows protocol statistics
netstat -p                      # Shows process ID/name for connections (requires root)
netstat -c                      # Continuously displays connections
netstat -tulnp                  # Common combination: listening ports with processes
netstat -an | grep :80          # Shows connections on port 80
netstat -an | grep ESTABLISHED  # Shows established connections
```

**Note:** `netstat` is deprecated. Use `ss` command for modern systems.

---

### `ss`

**What it is:**
- Modern replacement for netstat showing socket statistics
- Faster and more detailed than netstat for socket information

**What it does:**
- Displays detailed information about network sockets and connections
- Shows TCP, UDP, Unix sockets with filtering capabilities

**When to use:**
- Viewing active network connections and listening ports
- Diagnosing network issues with detailed socket information

**Popular flags with examples:**
```bash
ss                              # Shows all established connections
ss -tuln                        # Shows listening TCP/UDP ports
ss -tun                         # Shows all TCP/UDP connections
ss -a                           # Shows all sockets (listening and established)
ss -l                           # Shows only listening sockets
ss -p                           # Shows process using socket
ss -s                           # Shows socket statistics summary
ss -4                           # Shows only IPv4 sockets
ss -6                           # Shows only IPv6 sockets
ss -t state established         # Shows only established TCP connections
ss dst 192.168.1.100            # Shows connections to specific IP
ss sport = :22                  # Shows sockets on source port 22
ss -tunlp | grep :80            # Shows what's listening on port 80
```

---

### `curl`

**What it is:**
- Command-line tool for transferring data with URLs
- Supports HTTP, HTTPS, FTP, and many other protocols

**What it does:**
- Downloads files, tests APIs, sends HTTP requests with various methods
- Supports authentication, headers, cookies, and complex requests

**When to use:**
- Testing RESTful APIs and web services
- Downloading files from the internet or making HTTP requests

**Popular flags with examples:**
```bash
curl https://example.com        # Fetches webpage content
curl -O https://example.com/file.zip  # Downloads file with original name
curl -o myfile.zip https://example.com/file.zip  # Downloads with custom name
curl -I https://example.com     # Fetches headers only
curl -L https://example.com     # Follows redirects
curl -X POST https://api.example.com/data  # Sends POST request
curl -d "param1=value1&param2=value2" https://api.example.com  # POST with data
curl -H "Content-Type: application/json" https://api.example.com  # Custom header
curl -u username:password https://example.com  # Basic authentication
curl -v https://example.com     # Verbose output showing request/response details
curl -s https://example.com     # Silent mode (no progress bar)
curl --limit-rate 100K https://example.com/large-file  # Limits download speed
curl -C - -O https://example.com/file.zip  # Resumes interrupted download
```

---

### `wget`

**What it is:**
- Non-interactive network downloader for retrieving files from web
- Downloads files via HTTP, HTTPS, and FTP protocols

**What it does:**
- Downloads files from URLs with support for resuming interrupted downloads
- Can download recursively, mirror websites, and handle authentication

**When to use:**
- Downloading files from the internet in scripts or automated tasks
- Mirroring websites or downloading entire directories

**Popular flags with examples:**
```bash
wget https://example.com/file.zip  # Downloads file
wget -O custom-name.zip https://example.com/file.zip  # Saves with custom name
wget -c https://example.com/file.zip  # Continues/resumes interrupted download
wget -b https://example.com/large-file  # Downloads in background
wget -i urls.txt                # Downloads all URLs from file
wget -r https://example.com     # Recursively downloads website
wget --limit-rate=200k https://example.com/file  # Limits download speed
wget -q https://example.com/file  # Quiet mode (no output)
wget --user=username --password=pass https://example.com/file  # Authentication
wget --tries=10 https://example.com/file  # Retries download 10 times
wget -m https://example.com     # Mirrors website
wget --no-check-certificate https://example.com  # Ignores SSL certificate errors
```

---

### `nslookup`

**What it is:**
- Queries DNS servers to find IP addresses and DNS records
- Interactive and non-interactive DNS lookup tool

**What it does:**
- Resolves domain names to IP addresses and vice versa
- Queries specific DNS record types (A, MX, NS, etc.)

**When to use:**
- Finding IP address of a domain name
- Troubleshooting DNS resolution issues

**Popular usage examples:**
```bash
nslookup google.com             # Looks up IP address for google.com
nslookup 8.8.8.8                # Reverse lookup (IP to hostname)
nslookup -type=mx google.com    # Queries mail server records
nslookup -type=ns google.com    # Queries nameserver records
nslookup -type=any google.com   # Queries all available records
nslookup google.com 8.8.8.8     # Uses specific DNS server (8.8.8.8)
```

---

### `dig`

**What it is:**
- DNS lookup utility providing detailed query information
- More powerful and flexible than nslookup for DNS troubleshooting

**What it does:**
- Performs DNS queries showing detailed response information
- Supports various query types and DNS server specification

**When to use:**
- Detailed DNS troubleshooting and verification
- Testing DNS server responses and record propagation

**Popular flags with examples:**
```bash
dig google.com                  # Performs DNS lookup for google.com
dig google.com +short           # Shows only IP address (brief output)
dig @8.8.8.8 google.com         # Uses specific DNS server
dig google.com MX               # Queries mail server records
dig google.com NS               # Queries nameserver records
dig google.com ANY              # Queries all record types
dig -x 8.8.8.8                  # Reverse DNS lookup
dig google.com +trace           # Traces DNS delegation path
dig google.com +noall +answer   # Shows only answer section
dig google.com AAAA             # Queries IPv6 address
```

---

### `host`

**What it is:**
- Simple DNS lookup utility for resolving hostnames
- Lightweight alternative to dig and nslookup

**What it does:**
- Performs straightforward DNS lookups showing IP addresses
- Can query specific record types with minimal output

**When to use:**
- Quick DNS lookups without detailed information
- Simple hostname to IP resolution in scripts

**Popular flags with examples:**
```bash
host google.com                 # Looks up IP address for google.com
host 8.8.8.8                    # Reverse DNS lookup
host -t MX google.com           # Queries mail server records
host -t NS google.com           # Queries nameserver records
host -a google.com              # Shows all DNS records
host google.com 8.8.8.8         # Uses specific DNS server
```

---

### `nc` (netcat)

**What it is:**
- Versatile networking utility for reading/writing data across network connections
- Often called the "Swiss Army knife" of networking tools

**What it does:**
- Creates TCP/UDP connections, listens on ports, transfers files, and port scanning
- Can act as simple TCP server or client for testing

**When to use:**
- Testing if specific ports are open and accepting connections
- Transferring files between machines or debugging network applications

**Popular flags with examples:**
```bash
nc -l 8080                      # Listens on port 8080
nc example.com 80               # Connects to example.com on port 80
nc -v example.com 22            # Verbose connection test to port 22
nc -z example.com 20-100        # Scans ports 20-100
nc -u example.com 53            # Connects using UDP
echo "Test" | nc example.com 80 # Sends data to server
nc -l 8080 > received_file      # Receives file on port 8080
nc example.com 8080 < file.txt  # Sends file to remote host
nc -w 3 example.com 80          # Sets 3 second timeout
```

---

### `telnet`

**What it is:**
- Network protocol and client for connecting to remote systems
- Simple tool for testing TCP connections and ports

**What it does:**
- Creates TCP connections to remote hosts on specified ports
- Allows interactive communication with network services

**When to use:**
- Testing if specific ports are open and accepting connections
- Manually testing protocols like HTTP, SMTP, or POP3

**Usage examples:**
```bash
telnet example.com 80           # Connects to example.com on port 80
telnet 192.168.1.1 23           # Connects to IP on telnet port
telnet example.com 25           # Tests SMTP server connection
# After connecting, type commands manually
# Press Ctrl+] then type 'quit' to exit
```

**Note:** Telnet is insecure (unencrypted). Use SSH for actual remote access.

---

### `ssh`

**What it is:**
- Secure Shell protocol client for encrypted remote login and command execution
- Securely connects to remote systems over networks

**What it does:**
- Provides encrypted remote terminal access and file transfer
- Supports authentication via passwords or SSH keys

**When to use:**
- Securely logging into remote servers for administration
- Executing commands on remote systems securely

**Popular flags with examples:**
```bash
ssh user@example.com            # Connects to remote server as user
ssh -p 2222 user@example.com    # Connects to custom port 2222
ssh -i ~/.ssh/id_rsa user@example.com  # Uses specific private key
ssh user@example.com "ls -la"   # Executes command on remote server
ssh -L 8080:localhost:80 user@example.com  # Local port forwarding
ssh -R 8080:localhost:80 user@example.com  # Remote port forwarding
ssh -D 1080 user@example.com    # Creates SOCKS proxy
ssh -X user@example.com         # Enables X11 forwarding for GUI apps
ssh -v user@example.com         # Verbose mode for debugging
ssh -o StrictHostKeyChecking=no user@example.com  # Disables host key checking
```

---

### `scp`

**What it is:**
- Secure copy protocol for transferring files between hosts over SSH
- Encrypted file transfer using SSH protocol

**What it does:**
- Copies files securely between local and remote systems
- Preserves file permissions and timestamps

**When to use:**
- Securely transferring files to or from remote servers
- Copying files between remote systems

**Popular flags with examples:**
```bash
scp file.txt user@example.com:/path/  # Copies file to remote server
scp user@example.com:/path/file.txt . # Copies file from remote server
scp -r directory/ user@example.com:/path/  # Recursively copies directory
scp -P 2222 file.txt user@example.com:/path/  # Uses custom port
scp -i ~/.ssh/id_rsa file.txt user@example.com:/path/  # Uses specific key
scp file1.txt file2.txt user@example.com:/path/  # Copies multiple files
scp -p file.txt user@example.com:/path/  # Preserves modification times
scp -C file.txt user@example.com:/path/  # Enables compression
scp user1@host1:/path/file user2@host2:/path/  # Copies between remote hosts
```

---

### `rsync`

**What it is:**
- Fast and versatile file copying and synchronization tool
- Efficiently transfers and synchronizes files locally or over network

**What it does:**
- Copies files while transferring only differences (delta-transfer algorithm)
- Supports compression, preservation of permissions, and partial transfers

**When to use:**
- Synchronizing files and directories between systems
- Creating backups with incremental updates

**Popular flags with examples:**
```bash
rsync -av source/ destination/  # Archives with verbose output
rsync -avz source/ user@example.com:/destination/  # Syncs to remote with compression
rsync -avz --delete source/ destination/  # Deletes files in destination not in source
rsync -avz --exclude='*.log' source/ destination/  # Excludes .log files
rsync -avzP source/ destination/  # Shows progress during transfer
rsync -avz --dry-run source/ destination/  # Shows what would be transferred (test mode)
rsync -avz -e "ssh -p 2222" source/ user@example.com:/dest/  # Uses custom SSH port
rsync -avz --include='*.txt' --exclude='*' source/ dest/  # Includes only .txt files
rsync -avz --bwlimit=1000 source/ destination/  # Limits bandwidth to 1000 KB/s
rsync -avz --backup --backup-dir=/backups source/ dest/  # Creates backups
```

---

## Hardware & System Information

### `lscpu`

**What it is:**
- Displays detailed CPU architecture information
- Shows processor characteristics and configuration

**What it does:**
- Presents CPU information including cores, threads, architecture, and cache sizes
- Shows CPU frequency, virtualization support, and capabilities

**When to use:**
- Checking CPU specifications and capabilities
- Verifying processor configuration for software requirements

**Usage examples:**
```bash
lscpu                           # Shows detailed CPU information
lscpu | grep "Model name"       # Extracts CPU model
lscpu -e                        # Shows extended CPU information
lscpu -p                        # Shows parseable output
```

---

### `lspci`

**What it is:**
- Lists all PCI devices connected to the system
- Shows graphics cards, network adapters, and other PCI hardware

**What it does:**
- Displays PCI bus information including device IDs, vendors, and drivers
- Shows detailed information about PCI hardware components

**When to use:**
- Identifying installed hardware components and their specifications
- Finding device IDs for driver installation

**Popular flags with examples:**
```bash
lspci                           # Lists all PCI devices
lspci -v                        # Verbose output with detailed information
lspci -vv                       # Even more verbose output
lspci -k                        # Shows kernel drivers handling each device
lspci | grep -i vga             # Shows graphics card information
lspci | grep -i ethernet        # Shows network adapters
lspci -nn                       # Shows device IDs alongside names
lspci -t                        # Shows device tree
lspci -s 00:02.0                # Shows specific device by ID
```

---

### `lsusb`

**What it is:**
- Lists USB devices connected to the system
- Shows USB controllers, hubs, and connected peripherals

**What it does:**
- Displays USB device information including vendor IDs, product IDs, and descriptions
- Shows USB bus topology and device hierarchy

**When to use:**
- Identifying connected USB devices and their specifications
- Troubleshooting USB device recognition issues

**Popular flags with examples:**
```bash
lsusb                           # Lists all USB devices
lsusb -v                        # Verbose output with detailed information
lsusb -t                        # Shows USB device tree
lsusb -d vendor:product         # Shows specific device
lsusb | grep -i mouse           # Finds mouse devices
```

---

### `lsblk`

**What it is:**
- Lists information about block devices (covered in Disk Management section)
- Displays storage devices and their partitions

**What it does:**
- Shows all disk drives, partitions, mount points, and sizes
- Displays device hierarchy in tree format

**When to use:**
- Viewing available storage devices and their configuration
- Checking mount points and filesystem information

**Popular flags with examples:**
```bash
lsblk                           # Shows all block devices
lsblk -f                        # Shows filesystem information
lsblk -a                        # Shows all devices including empty ones
```

---

### `dmidecode`

**What it is:**
- Displays DMI/SMBIOS hardware information from system firmware
- Shows detailed information about computer hardware components

**What it does:**
- Retrieves hardware information from BIOS including manufacturer, model, serial numbers
- Shows memory, processor, baseboard, and BIOS details

**When to use:**
- Getting detailed hardware specifications without opening the computer
- Finding serial numbers, model information for warranty or inventory

**Popular flags with examples:**
```bash
sudo dmidecode                  # Shows all hardware information
sudo dmidecode -t system        # Shows system information
sudo dmidecode -t processor     # Shows CPU information
sudo dmidecode -t memory        # Shows RAM information
sudo dmidecode -t bios          # Shows BIOS information
sudo dmidecode -t baseboard     # Shows motherboard information
sudo dmidecode -s system-serial-number  # Shows system serial number
sudo dmidecode -s system-manufacturer  # Shows manufacturer name
```

---

### `hwinfo`

**What it is:**
- Comprehensive hardware information tool
- Probes and displays detailed hardware configuration

**What it does:**
- Shows extensive information about all detected hardware
- Provides more detailed output than lspci or lsusb

**When to use:**
- Getting comprehensive hardware inventory and specifications
- Troubleshooting hardware detection issues

**Popular flags with examples:**
```bash
sudo hwinfo                     # Shows all hardware information
sudo hwinfo --short             # Shows brief summary
sudo hwinfo --cpu               # Shows CPU information
sudo hwinfo --disk              # Shows disk information
sudo hwinfo --network           # Shows network hardware
sudo hwinfo --usb               # Shows USB devices
sudo hwinfo --gfxcard           # Shows graphics card information
```

**Note:** Install with `apt install hwinfo` or `yum install hwinfo`

---

### `uname`

**What it is:**
- Prints system information including kernel version and architecture
- Shows basic operating system identification

**What it does:**
- Displays kernel name, version, machine hardware, and operating system
- Provides quick system identification information

**When to use:**
- Checking kernel version and system architecture
- Verifying operating system information for compatibility

**Popular flags with examples:**
```bash
uname                           # Shows kernel name (Linux)
uname -a                        # Shows all system information
uname -r                        # Shows kernel release version
uname -m                        # Shows machine hardware architecture (x86_64, etc.)
uname -n                        # Shows network hostname
uname -s                        # Shows kernel name
uname -v                        # Shows kernel version with build date
uname -o                        # Shows operating system name
```

---

### `hostname`

**What it is:**
- Displays or sets the system's hostname
- Shows or modifies the computer's name on the network

**What it does:**
- Displays current hostname or sets new hostname temporarily
- Shows FQDN (Fully Qualified Domain Name) and IP address

**When to use:**
- Checking current system hostname
- Temporarily changing hostname for testing

**Popular flags with examples:**
```bash
hostname                        # Shows current hostname
hostname -f                     # Shows FQDN (fully qualified domain name)
hostname -i                     # Shows IP address
hostname -I                     # Shows all IP addresses
hostname -d                     # Shows DNS domain name
sudo hostname newhostname       # Sets hostname temporarily (until reboot)
hostnamectl                     # Shows detailed hostname information (systemd)
sudo hostnamectl set-hostname newhostname  # Sets hostname permanently (systemd)
```

---

### `uptime`

**What it is:**
- Shows how long the system has been running
- Displays system uptime, logged-in users, and load averages

**What it does:**
- Shows current time, how long system has been up, number of users, and load averages
- Provides quick system status snapshot

**When to use:**
- Checking system uptime and overall load
- Monitoring system stability and performance at a glance

**Usage examples:**
```bash
uptime                          # Shows uptime and load averages
uptime -p                       # Shows uptime in pretty format
uptime -s                       # Shows time when system started
```

---

### `free`

**What it is:**
- Displays memory usage information (RAM and swap)
- Shows available and used memory

**What it does:**
- Reports total, used, free, shared, and available memory
- Shows swap space usage

**When to use:**
- Checking available memory before running memory-intensive tasks
- Monitoring memory usage during troubleshooting

**Popular flags with examples:**
```bash
free                            # Shows memory in kilobytes
free -h                         # Shows memory in human-readable format (GB/MB)
free -m                         # Shows memory in megabytes
free -g                         # Shows memory in gigabytes
free -s 5                       # Updates display every 5 seconds
free -t                         # Shows totals line
free -w                         # Shows wide output with separate cache column
```

---

### `lshw`

**What it is:**
- Lists detailed hardware configuration information
- Comprehensive hardware information extraction tool

**What it does:**
- Extracts detailed information about hardware configuration
- Shows hardware capabilities, configuration, and driver information

**When to use:**
- Getting complete hardware inventory with detailed specifications
- Generating hardware reports for documentation

**Popular flags with examples:**
```bash
sudo lshw                       # Shows all hardware information
sudo lshw -short                # Shows brief summary
sudo lshw -html > hardware.html # Outputs in HTML format
sudo lshw -xml                  # Outputs in XML format
sudo lshw -class disk           # Shows only disk information
sudo lshw -class network        # Shows only network hardware
sudo lshw -class processor      # Shows only CPU information
sudo lshw -sanitize             # Removes sensitive information (serial numbers)
```

**Note:** Install with `apt install lshw` or `yum install lshw`

---

## Archive & Compression

### `tar`

**What it is:**
- Archive utility for creating and extracting tape archives
- Combines multiple files into single archive file, optionally compressed

**What it does:**
- Creates, extracts, and lists contents of tar archives
- Supports various compression methods (gzip, bzip2, xz)

**When to use:**
- Creating backups of directories and files
- Distributing multiple files as single package

**Popular flags with examples:**
```bash
tar -cvf archive.tar files/     # Creates tar archive from directory
tar -czvf archive.tar.gz files/ # Creates gzip compressed archive
tar -cjvf archive.tar.bz2 files/  # Creates bzip2 compressed archive
tar -cJvf archive.tar.xz files/   # Creates xz compressed archive
tar -xvf archive.tar            # Extracts tar archive
tar -xzvf archive.tar.gz        # Extracts gzip archive
tar -xjvf archive.tar.bz2       # Extracts bzip2 archive
tar -xJvf archive.tar.xz        # Extracts xz archive
tar -tvf archive.tar            # Lists contents of archive
tar -xvf archive.tar -C /destination/  # Extracts to specific directory
tar -xzvf archive.tar.gz file.txt  # Extracts specific file
tar -czvf archive.tar.gz --exclude='*.log' files/  # Excludes .log files
tar -czf - files/ | ssh user@example.com "cat > /path/archive.tar.gz"  # Creates and transfers
```

**Common flags:**
- `c` = create, `x` = extract, `t` = list
- `v` = verbose, `f` = file
- `z` = gzip, `j` = bzip2, `J` = xz

---

### `gzip`

**What it is:**
- Compresses files using gzip compression algorithm
- Reduces file size while maintaining file integrity

**What it does:**
- Compresses files creating .gz compressed versions
- Decompresses .gz files back to original format

**When to use:**
- Compressing individual files to save disk space
- Preparing files for transfer over network

**Popular flags with examples:**
```bash
gzip file.txt                   # Compresses file to file.txt.gz (deletes original)
gzip -k file.txt                # Compresses keeping original file
gzip -d file.txt.gz             # Decompresses file
gunzip file.txt.gz              # Decompresses file (same as gzip -d)
gzip -9 file.txt                # Maximum compression (level 9)
gzip -1 file.txt                # Fastest compression (level 1)
gzip -c file.txt > file.txt.gz  # Compresses to stdout, keeps original
gzip -r directory/              # Recursively compresses files in directory
gzip -l file.txt.gz             # Lists compression information
gzip -t file.txt.gz             # Tests integrity of compressed file
```

---

### `gunzip`

**What it is:**
- Decompresses gzip compressed files
- Equivalent to `gzip -d`

**What it does:**
- Extracts original files from .gz compressed archives
- Removes .gz extension after decompression

**When to use:**
- Extracting compressed files received or downloaded
- Restoring compressed backup files

**Usage examples:**
```bash
gunzip file.txt.gz              # Decompresses file
gunzip -k file.txt.gz           # Decompresses keeping .gz file
gunzip -c file.txt.gz > file.txt  # Decompresses to stdout
gunzip -t file.txt.gz           # Tests compressed file integrity
```

---

### `bzip2`

**What it is:**
- Compresses files using bzip2 algorithm (better compression than gzip)
- Creates .bz2 compressed files

**What it does:**
- Compresses files with higher compression ratio than gzip
- Decompresses .bz2 files back to original format

**When to use:**
- When maximum compression is more important than speed
- Compressing large files where space savings matter

**Popular flags with examples:**
```bash
bzip2 file.txt                  # Compresses file to file.txt.bz2
bzip2 -k file.txt               # Compresses keeping original
bzip2 -d file.txt.bz2           # Decompresses file
bunzip2 file.txt.bz2            # Decompresses file (same as bzip2 -d)
bzip2 -9 file.txt               # Maximum compression
bzip2 -1 file.txt               # Fastest compression
bzip2 -c file.txt > file.txt.bz2  # Compresses to stdout
bzip2 -t file.txt.bz2           # Tests file integrity
```

---

### `xz`

**What it is:**
- Compresses files using LZMA/LZMA2 algorithm (highest compression)
- Creates .xz compressed files with excellent compression ratios

**What it does:**
- Provides very high compression ratios, better than gzip or bzip2
- Decompresses .xz files back to original format

**When to use:**
- When maximum compression is critical (distributions, archives)
- Compressing large data sets where size reduction is paramount

**Popular flags with examples:**
```bash
xz file.txt                     # Compresses file to file.txt.xz
xz -k file.txt                  # Compresses keeping original
xz -d file.txt.xz               # Decompresses file
unxz file.txt.xz                # Decompresses file (same as xz -d)
xz -9 file.txt                  # Maximum compression (very slow)
xz -0 file.txt                  # Fastest compression
xz -c file.txt > file.txt.xz    # Compresses to stdout
xz -t file.txt.xz               # Tests file integrity
xz -l file.txt.xz               # Lists compression information
```

---

### `zip`

**What it is:**
- Creates compressed ZIP archives compatible with Windows
- Packages and compresses multiple files into .zip format

**What it does:**
- Creates ZIP archives containing multiple files and directories
- Compresses files while maintaining directory structure

**When to use:**
- Creating archives for sharing with Windows users
- Packaging files for email or web download

**Popular flags with examples:**
```bash
zip archive.zip file1.txt file2.txt  # Creates zip archive
zip -r archive.
