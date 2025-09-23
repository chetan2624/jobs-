# 🐧 Linux User and Group Management - Complete Practice Guide

> A comprehensive, hands-on guide to mastering user and group management in Linux with real-world examples and best practices.

## 📋 Table of Contents

1. [Understanding Users and Groups](#understanding-users-and-groups)
2. [User Management Commands](#user-management-commands)
3. [Group Management Commands](#group-management-commands)
4. [Information & Monitoring Commands](#information--monitoring-commands)
5. [File Permissions & User Context](#file-permissions--user-context)
6. [Real-World Scenarios](#real-world-scenarios)
7. [Best Practices & Security](#best-practices--security)
8. [Troubleshooting Common Issues](#troubleshooting-common-issues)
9. [Quick Reference Cheat Sheet](#quick-reference-cheat-sheet)

---

## 🎯 Understanding Users and Groups

### What is a User?
A **user** in Linux is an account that represents a person or service that can interact with the system. Each user has unique identifiers and permissions.

### What is a Group?
A **group** is a collection of users that share common permissions and access rights. Groups simplify permission management by allowing administrators to assign rights to multiple users simultaneously.

### Types of Users

| User Type | UID Range | Description | Example |
|-----------|-----------|-------------|---------|
| **Root User** | 0 | Superuser with unlimited privileges | `root` |
| **System Users** | 1-999 | Service accounts for system processes | `daemon`, `www-data` |
| **Regular Users** | 1000+ | Normal user accounts for people | `john`, `alice` |

### Important Files

| File | Purpose | Format |
|------|---------|--------|
| `/etc/passwd` | User account information | `username:x:UID:GID:comment:home:shell` |
| `/etc/shadow` | Encrypted passwords and aging info | `username:password:lastchange:min:max:warn:inactive:expire` |
| `/etc/group` | Group information | `groupname:x:GID:members` |
| `/etc/gshadow` | Group passwords and administrators | `groupname:password:administrators:members` |

### 💡 Key Points to Remember
- Every user has a unique **UID** (User ID)
- Every group has a unique **GID** (Group ID)  
- Users can belong to multiple groups
- The first group a user belongs to is their **primary group**
- Additional groups are **secondary groups**

---

## 👤 User Management Commands

### 1. `useradd` - Creating Users

#### Basic Syntax
```bash
useradd [options] username
```

#### Essential Options

| Option | Description | Example |
|--------|-------------|---------|
| `-m` | Create home directory | `useradd -m john` |
| `-s` | Set default shell | `useradd -s /bin/bash john` |
| `-g` | Set primary group | `useradd -g developers john` |
| `-G` | Add to secondary groups | `useradd -G sudo,admin john` |
| `-d` | Custom home directory | `useradd -d /custom/home john` |
| `-u` | Specify UID | `useradd -u 1500 john` |
| `-c` | Add comment/description | `useradd -c "John Doe" john` |

#### Practical Examples

```bash
# Create a basic user with home directory
useradd -m alice

# Create a user with specific shell and groups
useradd -m -s /bin/bash -G sudo,developers -c "Alice Smith" alice

# Create a system user (no home directory)
useradd -r -s /bin/false webserver

# Create user with custom home directory
useradd -m -d /opt/projects/bob -s /bin/bash bob
```

#### 🔥 Use Cases
- **New employee onboarding**: Create user accounts for new team members
- **Service accounts**: Create system users for applications
- **Project-specific users**: Create users for specific projects or clients
- **Temporary accounts**: Create accounts for contractors or temporary workers

#### ⚠️ Points to Remember
- Always use `-m` to create home directory for regular users
- System users should use `-r` option
- Choose appropriate shells (`/bin/bash` for interactive, `/bin/false` for system users)
- Set strong password policies

---

### 2. `usermod` - Modifying Users

#### Basic Syntax
```bash
usermod [options] username
```

#### Essential Options

| Option | Description | Example |
|--------|-------------|---------|
| `-aG` | Add to groups (append) | `usermod -aG docker alice` |
| `-G` | Set groups (replace) | `usermod -G sudo,admin alice` |
| `-s` | Change shell | `usermod -s /bin/zsh alice` |
| `-d` | Change home directory | `usermod -d /new/home alice` |
| `-l` | Change username | `usermod -l newalice alice` |
| `-L` | Lock account | `usermod -L alice` |
| `-U` | Unlock account | `usermod -U alice` |

#### Practical Examples

```bash
# Add user to docker group
usermod -aG docker alice

# Change user's shell to zsh
usermod -s /bin/zsh alice

# Lock a user account
usermod -L alice

# Change username
usermod -l alice_new alice

# Move home directory
usermod -d /new/home/alice -m alice
```

#### 🔥 Use Cases
- **Role changes**: Add/remove users from groups when roles change
- **Security**: Lock compromised accounts temporarily
- **Shell preferences**: Change user shells based on preferences
- **Home directory management**: Move user data to different locations

---

### 3. `userdel` - Deleting Users

#### Basic Syntax
```bash
userdel [options] username
```

#### Essential Options

| Option | Description | Example |
|--------|-------------|---------|
| `-r` | Remove home directory and mail spool | `userdel -r alice` |
| `-f` | Force deletion (even if logged in) | `userdel -f alice` |

#### Practical Examples

```bash
# Delete user but keep home directory
userdel alice

# Delete user and remove all files
userdel -r alice

# Force delete logged-in user
userdel -f -r alice
```

#### ⚠️ Points to Remember
- Always backup important data before deletion
- Check if user owns critical files elsewhere in the system
- Use `-r` carefully as it permanently deletes data
- Consider disabling instead of deleting for audit trails

---

### 4. `passwd` - Password Management

#### Basic Syntax
```bash
passwd [options] [username]
```

#### Essential Options

| Option | Description | Example |
|--------|-------------|---------|
| `-l` | Lock password | `passwd -l alice` |
| `-u` | Unlock password | `passwd -u alice` |
| `-d` | Delete password | `passwd -d alice` |
| `-e` | Expire password | `passwd -e alice` |
| `-S` | Show password status | `passwd -S alice` |

#### Practical Examples

```bash
# Change your own password
passwd

# Change another user's password (as root)
passwd alice

# Lock a user's password
passwd -l alice

# Force password change on next login
passwd -e alice

# Check password status
passwd -S alice
```

#### 🔥 Use Cases
- **Security incidents**: Lock compromised accounts
- **New users**: Force password change on first login  
- **Password policies**: Implement password aging
- **Account maintenance**: Regular password updates

---

## 👥 Group Management Commands

### 1. `groupadd` - Creating Groups

#### Basic Syntax
```bash
groupadd [options] groupname
```

#### Essential Options

| Option | Description | Example |
|--------|-------------|---------|
| `-g` | Specify GID | `groupadd -g 2000 developers` |
| `-r` | Create system group | `groupadd -r webadmin` |

#### Practical Examples

```bash
# Create a basic group
groupadd developers

# Create group with specific GID
groupadd -g 1500 projectx

# Create system group
groupadd -r backup-service
```

---

### 2. `groupmod` - Modifying Groups

#### Basic Syntax
```bash
groupmod [options] groupname
```

#### Essential Options

| Option | Description | Example |
|--------|-------------|---------|
| `-n` | Change group name | `groupmod -n newdevs developers` |
| `-g` | Change GID | `groupmod -g 1600 developers` |

#### Practical Examples

```bash
# Rename a group
groupmod -n backend-team developers

# Change group GID
groupmod -g 1800 developers
```

---

### 3. `groupdel` - Deleting Groups

#### Basic Syntax
```bash
groupdel groupname
```

#### Practical Examples

```bash
# Delete a group
groupdel developers

# Note: Cannot delete a group if it's a primary group for any user
```

#### ⚠️ Points to Remember
- Cannot delete a group if it's the primary group of any user
- Files owned by the group will become orphaned
- Check group membership before deletion

---

### 4. `gpasswd` - Group Password Management

#### Basic Syntax
```bash
gpasswd [options] groupname
```

#### Essential Options

| Option | Description | Example |
|--------|-------------|---------|
| `-a` | Add user to group | `gpasswd -a alice developers` |
| `-d` | Remove user from group | `gpasswd -d alice developers` |
| `-A` | Set group administrators | `gpasswd -A alice,bob developers` |

#### Practical Examples

```bash
# Add user to group
gpasswd -a alice developers

# Remove user from group
gpasswd -d alice developers

# Set group administrators
gpasswd -A alice developers
```

---

## 📊 Information & Monitoring Commands

### 1. User Information Commands

#### `id` - Display User and Group IDs

```bash
# Show current user's ID info
id

# Show specific user's ID info
id alice

# Show only UID
id -u alice

# Show only primary GID
id -g alice

# Show all group IDs
id -G alice
```

#### `whoami` - Current User

```bash
# Display current username
whoami
```

#### `who` - Logged in Users

```bash
# Show all logged in users
who

# Show detailed information
who -a

# Show boot time
who -b
```

#### `w` - Detailed User Activity

```bash
# Show detailed user activity
w

# Show for specific user
w alice
```

### 2. Group Information Commands

#### `groups` - Show User Groups

```bash
# Show groups for current user
groups

# Show groups for specific user
groups alice
```

#### `getent` - Get Database Entries

```bash
# Show all users
getent passwd

# Show specific user
getent passwd alice

# Show all groups
getent group

# Show specific group
getent group developers
```

### 3. File Inspection Commands

```bash
# View user accounts
cat /etc/passwd
tail -n 5 /etc/passwd

# View group information
cat /etc/group
grep developers /etc/group

# View password info (root only)
sudo cat /etc/shadow
```

---

## 🔐 File Permissions & User Context

### Understanding File Ownership

Every file and directory has:
- **Owner** (user): The user who owns the file
- **Group**: The group that owns the file  
- **Others**: Everyone else

### Changing Ownership

#### `chown` - Change Owner

```bash
# Change owner only
chown alice file.txt

# Change owner and group
chown alice:developers file.txt

# Change recursively
chown -R alice:developers /project/

# Change only group
chown :developers file.txt
```

#### `chgrp` - Change Group

```bash
# Change group ownership
chgrp developers file.txt

# Change recursively  
chgrp -R developers /project/
```

### Running Commands as Different Users

#### `su` - Switch User

```bash
# Switch to root
su -

# Switch to specific user
su - alice

# Run single command as user
su -c "ls /home" alice
```

#### `sudo` - Execute as Another User

```bash
# Run command as root
sudo ls /root

# Run command as specific user
sudo -u alice ls /home/alice

# Switch to root shell
sudo -i

# Run command with group
sudo -g developers touch /shared/file.txt
```

---

## 🌟 Real-World Scenarios

### Scenario 1: Onboarding New Developer

```bash
# Step 1: Create user with appropriate groups
sudo useradd -m -s /bin/bash -G developers,docker -c "John Smith" john

# Step 2: Set temporary password
sudo passwd john

# Step 3: Force password change on first login
sudo passwd -e john

# Step 4: Create project directory with proper ownership
sudo mkdir /projects/webapp
sudo chown john:developers /projects/webapp
sudo chmod 775 /projects/webapp

# Step 5: Add SSH key (if applicable)
sudo mkdir /home/john/.ssh
sudo chown john:john /home/john/.ssh
sudo chmod 700 /home/john/.ssh
```

### Scenario 2: Setting up Web Server User

```bash
# Create system user for web server
sudo useradd -r -s /bin/false -d /var/www -c "Web Server" www-data

# Create web directory
sudo mkdir -p /var/www/html
sudo chown www-data:www-data /var/www/html
sudo chmod 755 /var/www/html

# Add developers to www-data group for deployment
sudo usermod -aG www-data alice
sudo usermod -aG www-data bob
```

### Scenario 3: Project Team Setup

```bash
# Create project group
sudo groupadd project-alpha

# Create shared directory
sudo mkdir /shared/project-alpha
sudo chown :project-alpha /shared/project-alpha
sudo chmod 775 /shared/project-alpha
sudo chmod g+s /shared/project-alpha  # Set group sticky bit

# Add team members
sudo usermod -aG project-alpha alice
sudo usermod -aG project-alpha bob
sudo usermod -aG project-alpha charlie
```

### Scenario 4: Security Incident Response

```bash
# Immediately lock compromised account
sudo passwd -l compromised_user

# Check login history
last compromised_user
lastlog -u compromised_user

# Find files owned by user
find / -user compromised_user -type f 2>/dev/null

# Backup user data
sudo tar -czf /backup/compromised_user_backup.tar.gz /home/compromised_user

# Remove user but keep home directory for investigation
sudo userdel compromised_user
```

---

## 🛡️ Best Practices & Security

### User Account Security

#### Password Policies
```bash
# View current password aging policy
chage -l alice

# Set password expiration
sudo chage -M 90 alice

# Set minimum password age
sudo chage -m 7 alice

# Force password change on next login
sudo chage -d 0 alice
```

#### Account Lockout
```bash
# Lock account
sudo passwd -l alice
sudo usermod -L alice

# Unlock account
sudo passwd -u alice
sudo usermod -U alice

# Set account expiration
sudo chage -E 2024-12-31 alice
```

### Group Management Best Practices

1. **Principle of Least Privilege**: Only grant necessary permissions
2. **Regular Audits**: Review group memberships regularly
3. **Naming Conventions**: Use clear, descriptive group names
4. **Documentation**: Maintain records of group purposes

### Security Checklist

- [ ] Regular password updates
- [ ] Monitor failed login attempts
- [ ] Review sudo access regularly  
- [ ] Audit file permissions
- [ ] Remove unused accounts
- [ ] Use strong password policies
- [ ] Implement account lockout policies
- [ ] Monitor user activity logs

---

## 🔧 Troubleshooting Common Issues

### Issue 1: User Cannot Login

**Symptoms**: User account exists but login fails

**Diagnosis**:
```bash
# Check if account is locked
sudo passwd -S username

# Check password expiration
sudo chage -l username

# Check shell validity
grep username /etc/passwd
```

**Solutions**:
```bash
# Unlock account
sudo passwd -u username

# Reset password expiration
sudo chage -d 0 username

# Fix shell
sudo usermod -s /bin/bash username
```

### Issue 2: Permission Denied Errors

**Symptoms**: User cannot access files/directories

**Diagnosis**:
```bash
# Check user groups
groups username
id username

# Check file permissions
ls -la problematic_file
```

**Solutions**:
```bash
# Add to appropriate group
sudo usermod -aG required_group username

# Fix file ownership
sudo chown username:group file
sudo chmod 664 file
```

### Issue 3: Group Deletion Fails

**Symptoms**: Cannot delete group

**Diagnosis**:
```bash
# Check if group is primary for any user
grep ":group_gid:" /etc/passwd

# Check group membership
getent group groupname
```

**Solutions**:
```bash
# Change primary group for users
sudo usermod -g new_primary_group username

# Then delete the group
sudo groupdel old_group
```

---

## 📚 Quick Reference Cheat Sheet

### User Commands Quick Reference

| Command | Purpose | Quick Example |
|---------|---------|---------------|
| `useradd -m user` | Create user with home | `useradd -m alice` |
| `usermod -aG group user` | Add to group | `usermod -aG sudo alice` |
| `userdel -r user` | Delete user & home | `userdel -r alice` |
| `passwd user` | Change password | `passwd alice` |
| `su - user` | Switch user | `su - alice` |
| `sudo -u user cmd` | Run as user | `sudo -u alice ls` |

### Group Commands Quick Reference

| Command | Purpose | Quick Example |
|---------|---------|---------------|
| `groupadd group` | Create group | `groupadd developers` |
| `groupmod -n new old` | Rename group | `groupmod -n devs developers` |
| `groupdel group` | Delete group | `groupdel developers` |
| `gpasswd -a user group` | Add to group | `gpasswd -a alice devs` |
| `gpasswd -d user group` | Remove from group | `gpasswd -d alice devs` |

### Information Commands Quick Reference

| Command | Purpose | Output |
|---------|---------|---------|
| `whoami` | Current user | `alice` |
| `id` | User/group IDs | `uid=1000(alice) gid=1000(alice)...` |
| `groups` | User groups | `alice sudo developers` |
| `who` | Logged in users | `alice pts/0 2024-01-15 10:30` |
| `last` | Login history | Recent login records |

### File Permission Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `chown user:group file` | Change ownership | `chown alice:devs file.txt` |
| `chgrp group file` | Change group | `chgrp devs file.txt` |
| `chmod 755 file` | Change permissions | `chmod 755 script.sh` |

---

## 🎯 Practice Exercises

### Exercise 1: Basic User Management
1. Create a user named `testuser` with a home directory
2. Set the password for `testuser`
3. Add `testuser` to the `sudo` group
4. Verify the user can run sudo commands
5. Delete the user and home directory

### Exercise 2: Group Project Setup
1. Create a group called `webdev`
2. Create three users: `frontend`, `backend`, `designer`
3. Add all users to the `webdev` group
4. Create a shared directory `/projects/website`
5. Set appropriate ownership and permissions

### Exercise 3: Security Scenario
1. Create a user account for a temporary contractor
2. Set the account to expire in 30 days
3. Force password change on first login
4. After project completion, lock the account
5. Clean up by removing the account

---

## 📖 Additional Resources

### Log Files to Monitor
- `/var/log/auth.log` - Authentication attempts
- `/var/log/secure` - Security-related events (RedHat/CentOS)
- `/var/log/lastlog` - Last login information
- `/var/log/faillog` - Failed login attempts

### Useful Tools
- `htop` - Interactive process viewer with user info
- `finger` - Display user information
- `pinky` - Lightweight finger alternative
- `users` - Show currently logged in users

### Configuration Files
- `/etc/login.defs` - Default values for user accounts
- `/etc/default/useradd` - Default values for useradd
- `/etc/skel/` - Skeleton directory for new users
- `/etc/sudoers` - Sudo configuration

---

*Remember: With great power comes great responsibility. Always test commands in a safe environment before applying them to production systems!*

**Last Updated**: September 2025
