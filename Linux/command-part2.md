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
