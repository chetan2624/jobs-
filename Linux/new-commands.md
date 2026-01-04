# 🐧 Complete Linux Commands Reference Guide

## 📚 Table of Contents

- [File & Directory Management](#file--directory-management)
- [File Viewing & Text Display](#file-viewing--text-display)
- [Text Processing & Filters](#text-processing--filters)
- [File Permissions & Ownership](#file-permissions--ownership)
- [User & Group Management](#user--group-management)
- [Process & Job Management](#process--job-management)
- [Package Management](#package-management)

---

## File & Directory Management

### `ls`

**What it is:**
- A fundamental Linux utility that lists directory contents
- Displays files and folders with their properties and attributes

**What it does:**
- Shows all files and directories with detailed information like permissions, ownership, size, and dates
- Helps verify file existence and understand filesystem structure before operations

**When to use:**
- Navigating directories and verifying file presence before operations
- Organizing files and viewing hidden configuration files

**Popular flags with examples:**
```bash
ls -l           # Long format with permissions, owner, size, and date
ls -a           # Shows all files including hidden files (starting with .)
ls -lh          # Human-readable file sizes (KB, MB, GB)
ls -R           # Recursively lists all subdirectories
ls -lt          # Sorts by modification time, newest first
ls -ltr         # Sorts by modification time, oldest first
ls -d */        # Lists only directories, not files
```

---

### `pwd`

**What it is:**
- Prints the absolute path of your current working directory
- Shows your exact location in the filesystem hierarchy from root

**What it does:**
- Displays complete path of current location to prevent navigation confusion
- Provides full path for use in commands or sharing with colleagues

**When to use:**
- Verifying location before running potentially dangerous commands
- Storing current directory path in variables for scripts

**Popular flags with examples:**
```bash
pwd             # Displays the current working directory path
pwd -P          # Shows physical path, resolving symbolic links
pwd -L          # Shows logical path, including symbolic links (default)
```

---

### `cd`

**What it is:**
- Changes your current working directory to navigate the filesystem
- Moves between different folders using absolute or relative paths

**What it does:**
- Changes your current location to a specified path for file operations
- Supports shortcuts for quick navigation like home directory and parent folders

**When to use:**
- Navigating to project directories or different system locations
- Moving between directories during file organization and troubleshooting

**Popular flags with examples:**
```bash
cd /etc                    # Changes to /etc directory using absolute path
cd ..                      # Moves up one directory level
cd ~                       # Goes to your home directory
cd -                       # Returns to previous directory
cd                         # With no arguments, goes to home directory
cd ~/Documents/projects    # Navigates using path relative to home
cd ../../share            # Moves up two levels then into share directory
```

---

### `mkdir`

**What it is:**
- Creates new directories in the Linux filesystem
- Establishes folder structures for organizing files

**What it does:**
- Creates one or more directories with specified names and permissions
- Can create nested directory structures in a single command

**When to use:**
- Starting new projects and creating organized folder structures
- Setting up application directories and logging folders

**Popular flags with examples:**
```bash
mkdir myproject                    # Creates single directory
mkdir -p app/logs/data            # Creates nested directories including parents
mkdir dir1 dir2 dir3              # Creates multiple directories at once
mkdir -m 755 public_folder        # Creates directory with specific permissions
mkdir -p ~/projects/{frontend,backend,database}  # Creates multiple subdirectories
mkdir -v newdir                   # Creates with verbose output
```

---

### `rmdir`

**What it is:**
- Removes empty directories from the filesystem
- Safer alternative to rm -r as it only works on empty directories

**What it does:**
- Deletes directories that contain no files or subdirectories
- Provides error messages when attempting to delete non-empty directories

**When to use:**
- Cleaning up empty directory structures after moving files
- Removing unused folders during system maintenance

**Popular flags with examples:**
```bash
rmdir emptydir                # Removes single empty directory
rmdir dir1 dir2 dir3         # Removes multiple empty directories
rmdir -p parent/child/grandchild  # Removes nested empty directories
rmdir -v olddir              # Removes with verbose output
rmdir --ignore-fail-on-non-empty testdir  # Doesn't show errors if not empty
```

---

### `cp`

**What it is:**
- Duplicates files and directories from one location to another
- Creates exact copies while preserving the original files

**What it does:**
- Creates identical copies of files or directory structures in different locations
- Can preserve file attributes like timestamps, permissions, and ownership

**When to use:**
- Creating backups before editing critical configuration files
- Copying configuration templates and duplicating project structures

**Popular flags with examples:**
```bash
cp file1.txt file2.txt           # Copies file1 to file2 in same directory
cp -r sourcedir targetdir        # Recursively copies entire directory
cp -i file1.txt /backup/         # Interactive mode, prompts before overwriting
cp -p original.conf backup.conf  # Preserves file attributes
cp -u *.txt /backup/             # Copies only newer files (update mode)
cp -v docs/* /share/             # Verbose output showing each file
cp -a /data /backup/             # Archive mode: preserves all attributes
```

---

### `mv`

**What it is:**
- Relocates files and directories to different locations or renames them
- Changes file path or name while maintaining the original data

**What it does:**
- Moves files from source to destination or renames them in same location
- Works instantly within same filesystem without copying data

**When to use:**
- Organizing downloaded files into appropriate category folders
- Renaming files with timestamps or moving files during project refactoring

**Popular flags with examples:**
```bash
mv oldname.txt newname.txt      # Renames file in same directory
mv file.txt /tmp/               # Moves file to /tmp directory
mv -i file.txt /destination/    # Interactive prompt before overwriting
mv -n file.txt /backup/         # Moves only if destination doesn't exist
mv *.log /archive/              # Moves all .log files to archive
mv -v source.txt target.txt     # Verbose output showing what was moved
mv -u newer.txt /destination/   # Moves only if source is newer
```

---

### `rm`

**What it is:**
- Permanently deletes files and directories from the filesystem
- Removes data without sending to trash or recycle bin

**What it does:**
- Deletes specified files or directories immediately and permanently
- Can remove entire directory trees with all contents in single command

**When to use:**
- Cleaning up temporary build artifacts and old log files
- Removing test data and freeing disk space during maintenance

**Popular flags with examples:**
```bash
rm file.txt                     # Deletes single file permanently
rm -r directory/                # Recursively removes directory and contents
rm -i important.txt             # Prompts for confirmation before deleting
rm -f locked.txt                # Forces deletion without prompts
rm -rf oldproject/              # Recursively and forcefully deletes (DANGEROUS!)
rm -v file1.txt file2.txt       # Verbose output showing what was removed
rm *.tmp                        # Deletes all files matching pattern
rm -i *.log                     # Deletes log files with confirmation for each
```

⚠️ **CRITICAL WARNING:**
The `rm -rf` command is extremely dangerous. A simple typo (like `rm -rf / data` instead of `rm -rf /data`) can delete your entire system. Always double-check paths before executing.

---

### `touch`

**What it is:**
- Creates empty files instantly or updates file timestamps
- Quick way to create file placeholders or refresh file dates

**What it does:**
- Creates new empty files if they don't exist or updates modification timestamps
- Can set specific timestamps on files for testing or organizational purposes

**When to use:**
- Creating placeholder files during project setup
- Updating timestamps to trigger timestamp-based processes or builds

**Popular flags with examples:**
```bash
touch newfile.txt               # Creates empty file or updates timestamp
touch file1.txt file2.txt file3.txt  # Creates multiple files at once
touch -c existing.txt           # Updates timestamp only if file exists
touch -d "2023-12-25 10:00:00" oldfile.txt  # Sets specific date and time
touch -r reference.txt target.txt  # Sets timestamp to match reference file
touch -a file.txt               # Updates only access time
touch -m file.txt               # Updates only modification time
touch -t 202312251000.00 file.txt  # Sets timestamp using YYYYMMDDhhmm.ss
```

---

### `find`

**What it is:**
- Powerful tool that searches for files and directories based on various criteria
- Recursively searches through directory trees with complex filtering options

**What it does:**
- Locates files matching specified criteria like name, size, permissions, or modification time
- Can perform actions on found files like deleting, changing permissions, or executing commands

**When to use:**
- Searching for configuration files whose exact location you've forgotten
- Finding files with specific permissions during security audits or cleaning up large files

**Popular flags with examples:**
```bash
find /path -name "*.txt"        # Finds all .txt files in specified path
find . -type f                  # Finds all regular files
find . -type d                  # Finds all directories
find /var/log -mtime -7         # Finds files modified in last 7 days
find . -size +100M              # Finds files larger than 100 megabytes
find /home -user john           # Finds all files owned by user 'john'
find . -perm 777                # Finds files with specific permissions
find . -name "*.tmp" -delete    # Finds and deletes all .tmp files
find . -name "*.log" -exec rm {} \;  # Finds .log files and executes rm
find /data -type f -empty       # Finds all empty files
find . -iname "readme*"         # Case-insensitive search
```

---

### `locate`

**What it is:**
- Quickly searches for files by name using a pre-built database
- Provides near-instantaneous results compared to find command

**What it does:**
- Searches a database containing paths to all files on system for fast results
- Performs pattern matching on file paths without real-time filesystem traversal

**When to use:**
- Quickly finding system command or library file locations
- Rapid file discovery across large filesystems where find would take too long

**Popular flags with examples:**
```bash
locate filename.txt             # Quickly finds all paths containing filename
locate -i readme                # Case-insensitive search
locate -c "*.conf"              # Counts configuration files without listing
locate -b '\mysql'              # Searches only basename (filename) not full path
locate -e existing.txt          # Shows only files that currently exist
locate -r "\.log$"              # Uses regex to find files ending with .log
locate --limit 10 "*.txt"       # Limits output to first 10 results
sudo updatedb                   # Updates locate database with recent changes
```

**Note:** The locate database is typically updated once daily, so very recent files might not appear until after `updatedb` runs.

---

### `which`

**What it is:**
- Shows the full path of executable commands in your PATH
- Reveals which version of a command will execute when you type it

**What it does:**
- Searches through PATH directories to find executable program locations
- Returns the first matching executable found in PATH search order

**When to use:**
- Verifying which version of a program will run when multiple versions exist
- Finding correct full paths to commands before hardcoding them in scripts

**Popular flags with examples:**
```bash
which python                    # Shows path to python executable
which -a python                 # Shows all matching executables in PATH
which bash gcc make             # Shows paths for multiple commands
which nonexistent               # Returns nothing if command not found
which ls                        # Shows path to ls command (usually /usr/bin/ls)
```

---

### `whereis`

**What it is:**
- Locates binary, source code, and manual pages for commands
- Provides broader view than which by showing related documentation and source

**What it does:**
- Searches standard system directories for executable, source, and manual page locations
- Helps find not only where program runs from but also its documentation location

**When to use:**
- Locating manual pages to read documentation directly
- Finding source code locations during development or system understanding

**Popular flags with examples:**
```bash
whereis ls                      # Shows binary, source, and man page locations
whereis -b gcc                  # Shows only binary location
whereis -m python               # Shows only manual page locations
whereis -s apache2              # Shows only source code locations
whereis bash ssh grep           # Finds locations for multiple commands
whereis -u *                    # Shows commands with unusual entries
```

---

### `stat`

**What it is:**
- Displays detailed status information about files and filesystems
- Shows comprehensive metadata beyond what ls provides

**What it does:**
- Reveals complete file metadata including timestamps, inode number, permissions, and size
- Provides filesystem information when used with -f flag showing partition details

**When to use:**
- Debugging permission issues by examining detailed permission bits and ownership
- Checking precise timestamps during forensic analysis or security audits

**Popular flags with examples:**
```bash
stat file.txt                   # Shows complete file information
stat -f /home                   # Shows filesystem information for partition
stat -c "%n %s" file.txt        # Custom format: shows filename and size only
stat -c "%y" file.txt           # Shows only modification time
stat -c "%a" file.txt           # Shows numeric permissions (e.g., 644, 755)
stat --format="%U %G" file.txt  # Shows owner username and group name
stat -L symlink.txt             # Follows symbolic links and shows target info
```

---

### `file`

**What it is:**
- Determines and displays file type by examining contents
- Identifies formats regardless of file extensions or their absence

**What it does:**
- Analyzes file content using magic number databases to identify actual file types
- Detects various formats including text, binary executables, images, and archives

**When to use:**
- Verifying actual file type before opening files with suspicious extensions
- Identifying recovered files that have lost their original names and extensions

**Popular flags with examples:**
```bash
file document.pdf               # Shows PDF document with version info
file unknown_file               # Identifies file type without extension
file -b image.jpg               # Brief mode: shows type without filename
file -i file.txt                # Shows MIME type (e.g., text/plain)
file *                          # Checks types of all files in current directory
file -z compressed.gz           # Looks inside compressed files
file -L symlink                 # Follows symbolic links and shows target type
```

---

### `tree`

**What it is:**
- Displays directory structures in hierarchical tree format
- Visually represents folder organization with indentation and connecting lines

**What it does:**
- Creates visual representation of directory hierarchies showing parent-child relationships
- Recursively traverses subdirectories with filtering and summary statistics

**When to use:**
- Documenting project structures for README files or technical documentation
- Visualizing current structure before planning refactoring or reorganization

**Popular flags with examples:**
```bash
tree                            # Shows tree structure of current directory
tree -L 2                       # Limits tree depth to 2 levels
tree -d                         # Shows only directories, not files
tree -f                         # Shows full path prefix for each file
tree -a                         # Shows hidden files (starting with dot)
tree -h                         # Shows file sizes in human-readable format
tree -p                         # Shows file permissions alongside entries
tree -u                         # Shows file owner information
tree --dirsfirst               # Lists directories before files
tree -I 'node_modules|*.log'   # Excludes matching patterns
tree -o output.txt             # Saves tree output to file
```

**Note:** `tree` might not be installed by default. Install with `apt install tree` or `yum install tree`.

---

### `ln`

**What it is:**
- Creates links between files (hard links or symbolic links)
- Allows files to appear in multiple locations without duplication

**What it does:**
- Creates hard links pointing to same data on disk or symbolic links acting as shortcuts
- Enables efficient file organization without duplicating actual data

**When to use:**
- Creating symbolic links so multiple applications reference one master configuration
- Using hard links for space-efficient backups where unchanged files share storage

**Popular flags with examples:**
```bash
ln file.txt hardlink.txt        # Creates hard link to file.txt
ln -s /path/to/file.txt symlink.txt  # Creates symbolic link
ln -s /opt/app/config /etc/app-config  # Creates symlink for easier access
ln -sf newfile.txt symlink.txt  # Creates/replaces symlink forcefully
ln -s /usr/bin/python3.9 /usr/local/bin/python  # Version-specific symlink
ln file1 file2 file3 backup/    # Creates hard links to multiple files
ln -s "$(pwd)/script.sh" ~/bin/script  # Creates symlink using absolute path
```

**Key difference:** Hard links can't cross filesystems or link directories, while symbolic links can point anywhere but break if target moves.

---

## File Viewing & Text Display

### `cat`

**What it is:**
- Displays entire contents of files to terminal
- Concatenates multiple files together or creates new files

**What it does:**
- Outputs complete contents of files to standard output for quick viewing
- Combines multiple files in sequence into continuous output stream

**When to use:**
- Checking short configuration files without opening an editor
- Concatenating multiple log files to search through all at once

**Popular flags with examples:**
```bash
cat file.txt                    # Displays entire file contents
cat file1.txt file2.txt         # Displays both files concatenated
cat -n file.txt                 # Shows file with line numbers
cat -b file.txt                 # Numbers only non-empty lines
cat -s file.txt                 # Squeezes multiple blank lines into single line
cat -A file.txt                 # Shows all non-printing characters
cat > newfile.txt               # Creates new file from keyboard input (Ctrl+D to save)
cat file1.txt file2.txt > combined.txt  # Concatenates and saves to new file
cat file.txt | grep "error"     # Pipes contents to grep for searching
```

---

### `less`

**What it is:**
- Interactive pager displaying file contents one screen at a time
- Provides smooth scrolling and search capabilities for large files

**What it does:**
- Opens files in interactive viewer with forward and backward scrolling
- Allows quick navigation using keyboard shortcuts and regex search

**When to use:**
- Examining large log files that would overflow terminal with cat
- Reading documentation interactively with search and navigation features

**Popular flags and navigation:**
```bash
less file.txt                   # Opens file for interactive viewing
less +F file.txt                # Starts in follow mode (like tail -f)
less -N file.txt                # Shows line numbers
less -S file.txt                # Disables line wrapping
less +100 file.txt              # Opens at line 100

# Inside less, navigation commands:
Space or f                      # Forward one screen
b                              # Backward one screen
/pattern                       # Search forward for pattern
?pattern                       # Search backward for pattern
n                              # Repeat last search forward
N                              # Repeat last search backward
g                              # Go to beginning of file
G                              # Go to end of file
q                              # Quit less
h                              # Help screen
```

---

### `more`

**What it is:**
- Basic pager displaying file contents one screen at a time
- Simple forward navigation tool largely superseded by less

**What it does:**
- Displays file contents in pages that fit terminal screen with percentage indicator
- Provides basic forward navigation without backward scrolling

**When to use:**
- Quick viewing on minimal systems where less might not be available
- Simple sequential reading where backward scrolling isn't needed

**Popular flags and navigation:**
```bash
more file.txt                   # Displays file one screen at a time
more +10 file.txt               # Starts display at line 10
more -d file.txt                # Displays helpful navigation prompts
more -c file.txt                # Clears screen before displaying each page

# Inside more, navigation commands:
Space                          # Display next screen
Enter                          # Display next line
q                              # Quit more
/pattern                       # Search forward for pattern
n                              # Repeat last search
=                              # Show current line number
h                              # Help screen
```

**Note:** In modern Linux, `less` is recommended over `more` for better features.

---

### `head`

**What it is:**
- Displays the beginning portion of files (first 10 lines by default)
- Perfect for quickly previewing file contents

**What it does:**
- Outputs first lines of files to terminal without viewing entire contents
- Can display specific number of lines or bytes from file beginning

**When to use:**
- Checking CSV file headers before processing data
- Examining log file beginnings to see format or start time

**Popular flags with examples:**
```bash
head file.txt                   # Shows first 10 lines (default)
head -n 5 file.txt             # Shows first 5 lines only
head -n 20 file.txt            # Shows first 20 lines
head -c 100 file.txt           # Shows first 100 bytes
head -n -10 file.txt           # Shows all lines except last 10
head file1.txt file2.txt       # Shows first lines of each file with filenames
head -q file1.txt file2.txt    # Shows first lines without filenames (quiet)
head -v file.txt               # Always prints filename even for single file
ls -lt | head -n 5             # Shows 5 most recently modified files
```

---

### `tail`

**What it is:**
- Displays the ending portion of files (last 10 lines by default)
- Can follow files in real-time as new content is added

**What it does:**
- Outputs final lines of files for checking recent entries
- Follows files continuously displaying new lines as they're written (with -f)

**When to use:**
- Monitoring live log files to watch errors as they occur
- Checking job completion status by viewing last lines of logs

**Popular flags with examples:**
```bash
tail file.txt                   # Shows last 10 lines (default)
tail -n 20 file.txt            # Shows last 20 lines
tail -n 5 file.txt             # Shows last 5 lines only
tail -c 100 file.txt           # Shows last 100 bytes
tail -f /var/log/syslog        # Follows file, displaying new lines (Ctrl+C to stop)
tail -F /var/log/app.log       # Follows file and handles log rotation
tail -n +50 file.txt           # Shows all lines starting from line 50 to end
tail -f -n 0 file.txt          # Follows showing only new lines added after start
tail -f file1.log file2.log    # Follows multiple files simultaneously
tail --pid=1234 -f app.log     # Follows file until process 1234 terminates
```

---

### `wc`

**What it is:**
- Counts lines, words, and characters/bytes in files
- Provides quick statistics about file content size

**What it does:**
- Reports number of newlines, words, and bytes/characters in files
- Can count specific metrics independently (lines only, words only, etc.)

**When to use:**
- Verifying row counts in CSV files before analysis
- Counting error messages by piping grep output through wc

**Popular flags with examples:**
```bash
wc file.txt                     # Shows lines, words, and bytes
wc -l file.txt                  # Counts only lines
wc -w file.txt                  # Counts only words
wc -c file.txt                  # Counts only bytes
wc -m file.txt                  # Counts only characters (handles multibyte)
wc -L file.txt                  # Shows length of longest line
wc *.txt                        # Counts statistics for all .txt files
cat file.txt | wc -l            # Counts lines in command output
grep "error" app.log | wc -l    # Counts how many lines contain "error"
ls | wc -l                      # Counts number of files in directory
```

---

### `nl`

**What it is:**
- Adds line numbers to file contents when displaying them
- Offers sophisticated numbering options beyond cat -n

**What it does:**
- Numbers lines with customizable formats for numbers and separators
- Can selectively number certain lines based on content patterns

**When to use:**
- Adding line numbers for code review discussions
- Numbering document lines for citation and review purposes

**Popular flags with examples:**
```bash
nl file.txt                     # Numbers all non-empty lines
nl -ba file.txt                 # Numbers all lines including blank lines
nl -bt file.txt                 # Numbers only non-empty lines (default)
nl -s ": " file.txt            # Sets separator between number and content
nl -w 4 file.txt               # Sets width of line number field to 4
nl -v 100 file.txt             # Starts numbering at 100 instead of 1
nl -n rz file.txt              # Right-justifies with leading zeros
nl -n ln file.txt              # Left-justifies numbers
nl -n rn file.txt              # Right-justifies without zeros (default)
nl -i 5 file.txt               # Increments line numbers by 5
```

---

### `od`

**What it is:**
- Displays files in various numeric formats (octal, hex, decimal, ASCII)
- Reveals exact binary content including non-printable characters

**What it does:**
- Dumps file contents in multiple representation formats showing actual bytes
- Can display specific portions of files at defined offsets and lengths

**When to use:**
- Troubleshooting file encoding problems and detecting incorrect character encodings
- Inspecting binary executable files during reverse engineering

**Popular flags with examples:**
```bash
od file.bin                     # Displays file in octal format (default)
od -x file.bin                  # Displays file in hexadecimal format
od -d file.bin                  # Displays file in decimal format
od -c file.txt                  # Displays with characters and escape sequences
od -A x -t x1z file.bin        # Hex dump with ASCII interpretation
od -N 100 file.bin             # Displays only first 100 bytes
od -j 50 file.bin              # Skips first 50 bytes, then displays
od -w16 file.bin               # Sets output width to 16 bytes per line
od -An -tx1 file.bin           # Displays hex without offset addresses
```

---

### `xxd`

**What it is:**
- Creates hexadecimal dumps showing hex values and ASCII representation
- Can reverse the process to convert hex dumps back to binary files

**What it does:**
- Displays file contents in hex format with corresponding ASCII side by side
- Converts files to/from various formats including C-style array declarations

**When to use:**
- Analyzing binary file formats to understand headers and data sections
- Inspecting executable files during malware analysis or security research

**Popular flags with examples:**
```bash
xxd file.bin                    # Creates hex dump with ASCII representation
xxd -l 100 file.bin            # Dumps only first 100 bytes
xxd -s 50 file.bin             # Skips first 50 bytes, then dumps
xxd -c 16 file.bin             # Displays 16 bytes per line
xxd -b file.bin                # Shows binary representation instead of hex
xxd -i file.bin                # Outputs as C include file (array)
xxd -r hexdump.txt binary.out  # Reverses hex dump back to binary file
xxd -p file.bin                # Plain hex output without formatting
xxd -u file.bin                # Uses uppercase hex digits
```

---

### `strings`

**What it is:**
- Extracts and displays readable text strings from binary files
- Filters out non-printable characters to reveal embedded text

**What it does:**
- Scans binary files for sequences of printable characters above minimum length
- Helps identify file contents and embedded configuration data

**When to use:**
- Analyzing unknown binary files to identify program or purpose
- Searching executables for hardcoded passwords or sensitive data

**Popular flags with examples:**
```bash
strings file.bin                # Extracts all printable strings of 4+ characters
strings -n 8 program           # Shows only strings of 8 or more characters
strings -a executable          # Scans entire file including data sections
strings -t x file.bin          # Prints offset in hexadecimal before each string
strings -t d file.bin          # Prints offset in decimal before each string
strings -o file.bin            # Same as -t d, shows decimal offset
strings executable | grep password  # Searches for specific text in binary
strings -e l program           # Scans for little-endian encoded strings
strings -e b program           # Scans for big-endian encoded strings
```

---

## Text Processing & Filters

### `grep`

**What it is:**
- Searches for patterns in files using regular expressions
- One of the most powerful text filtering tools in Linux

**What it does:**
- Searches through text line by line, displaying only matching lines
- Supports recursive searches, case-insensitive matching, and context display

**When to use:**
- Searching log files for specific error codes or exceptions
- Finding all instances where a function or variable is used across source files

**Popular flags with examples:**
```bash
grep "error" file.txt           # Searches for lines containing "error"
grep -i "warning" file.txt      # Case-insensitive search
grep -r "TODO" /project/        # Recursively searches all files in directory
grep -n "function" code.py      # Shows line numbers with matches
grep -v "debug" app.log         # Inverse match: shows lines NOT containing "debug"
grep -c "error" log.txt         # Counts how many lines match
grep -l "config" *.conf         # Lists only filenames containing match
grep -w "test" file.txt         # Matches whole word "test" only
grep -A 3 "error" log.txt       # Shows 3 lines after each match
grep -B 2 "error" log.txt       # Shows 2 lines before each match
grep -C 2 "error" log.txt       # Shows 2 lines before AND after each match
grep -E "error|warning" log.txt # Extended regex: matches multiple patterns
grep -o "[0-9]+" file.txt       # Shows only matching portion, not entire line
```

---

### `egrep`

**What it is:**
- Equivalent to grep -E, enabling extended regular expressions by default
- Allows advanced regex features without escaping special characters

**What it does:**
- Interprets regex metacharacters like +, ?, |, and parentheses without backslashes
- Supports alternation, optional elements, and specific repetition counts

**When to use:**
- Searching for multiple related patterns simultaneously using alternation
- Matching email addresses, IP addresses, or URLs using extended regex

**Popular flags with examples:**
```bash
egrep "error|warning|critical" log.txt  # Matches any of these words
egrep "[0-9]{3}-[0-9]{4}" contacts.txt  # Matches phone pattern like 555-1234
egrep "start(s|ed|ing)?" text.txt       # Matches start, starts, started, starting
egrep "^(foo|bar)" file.txt             # Matches lines starting with foo or bar
egrep "[a-z]+@[a-z]+\.[a-z]+" emails.txt  # Matches basic email patterns
egrep "\b[A-Z]{2,}\b" file.txt          # Matches words with 2+ uppercase letters
egrep -o "http(s)?://[^ ]+" page.html   # Extracts URLs from HTML
```

**Note:** In modern Linux, `grep -E` is preferred over `egrep`, as egrep is deprecated though still widely available.

---

### `fgrep`

**What it is:**
- Equivalent to grep -F, treating search patterns as fixed strings
- Provides faster searches when you don't need pattern matching

**What it does:**
- Interprets search patterns literally without regex metacharacter interpretation
- Performs faster than grep because it doesn't parse regex patterns

**When to use:**
- Searching code for strings containing regex special characters you want to match literally
- Searching for exact error messages or identifiers with special characters

**Popular flags with examples:**
```bash
fgrep "error[123]" file.txt     # Searches for literal string "error[123]"
fgrep "192.168.1.1" config.txt  # Matches IP literally (dots not wildcards)
fgrep "$HOME" script.sh         # Searches for literal "$HOME" string
fgrep -f patterns.txt data.txt  # Searches for all patterns from file
fgrep "user@domain.com" logs.txt  # Matches email literally
fgrep "*important*" notes.txt   # Searches for text with literal asteris
ks
```

**Note:** Like egrep, `fgrep` is deprecated in favor of `grep -F`, though both work identically.

---

### `sed`

**What it is:**
- Powerful non-interactive text editor processing files line by line
- Performs operations like substitutions, deletions, and insertions based on patterns

**What it does:**
- Performs find-and-replace using patterns and regular expressions
- Can delete specific lines, insert content, and perform complex text manipulations

**When to use:**
- Programmatically replacing settings like IP addresses or port numbers across files
- Removing unwanted characters and normalizing formatting in data files

**Popular flags with examples:**
```bash
sed 's/old/new/' file.txt       # Replaces first occurrence on each line
sed 's/old/new/g' file.txt      # Replaces all occurrences (global)
sed -i 's/old/new/g' file.txt   # Edits file in-place (modifies actual file)
sed -i.bak 's/old/new/g' file.txt  # Edits in-place with backup
sed 's/old/new/2' file.txt      # Replaces only second occurrence
sed 's/old/new/gi' file.txt     # Case-insensitive global replacement
sed -n '10,20p' file.txt        # Prints only lines 10 through 20
sed '5d' file.txt               # Deletes line 5
sed '/pattern/d' file.txt       # Deletes all lines matching pattern
sed '/^$/d' file.txt            # Deletes all empty lines
sed '3i\New line' file.txt      # Inserts "New line" before line 3
sed '3a\New line' file.txt      # Appends "New line" after line 3
sed 's/\([0-9]\+\)/[\1]/g' file.txt  # Wraps numbers in brackets
```

---

### `awk`

**What it is:**
- Powerful pattern scanning and text processing language
- Treats files as collections of records (lines) and fields (columns)

**What it does:**
- Processes text field by field and line by line with automatic column splitting
- Provides complete programming language with variables, conditionals, loops, and functions

**When to use:**
- Extracting specific columns and calculating sums or averages from data files
- Parsing command output like ps or df to extract metrics

**Popular flags with examples:**
```bash
awk '{print $1}' file.txt       # Prints first column/field
awk '{print $1, $3}' file.txt   # Prints first and third columns
awk -F: '{print $1}' /etc/passwd  # Uses colon as field separator
awk '/error/ {print $0}' log.txt  # Prints entire line if contains "error"
awk '$3 > 100' data.txt         # Prints lines where third field > 100
awk '{sum += $2} END {print sum}' file.txt  # Sums values in second column
awk 'NR==5' file.txt            # Prints only line 5
awk 'NF > 0' file.txt           # Prints non-empty lines
awk '{print NR, $0}' file.txt   # Prints line number followed by entire line
awk -F, '{print $1 "," $3}' data.csv  # Reorders CSV columns
awk '/start/,/end/' file.txt    # Prints lines between patterns
awk '{print $1 "\t" $2}' file.txt  # Prints columns separated by tab
```

---

### `cut`

**What it is:**
- Extracts specific sections from each line of files
- Selects particular columns, character ranges, or fields based on delimiters

**What it does:**
- Cuts out and displays specific columns or fields from text
- Can extract characters by position, bytes, or fields by delimiter

**When to use:**
- Extracting specific columns like names or emails from CSV files
- Parsing system files like /etc/passwd by splitting on delimiters

**Popular flags with examples:**
```bash
cut -d: -f1 /etc/passwd         # Extracts first field using colon delimiter
cut -d, -f1,3 data.csv          # Extracts first and third fields from CSV
cut -c1-10 file.txt             # Extracts characters 1 through 10
cut -c5- file.txt               # Extracts from character 5 to end of line
cut -c1,5,9 file.txt            # Extracts specific character positions
cut -f2-5 data.txt              # Extracts fields 2 through 5 (default tab delimiter)
cut -d' ' -f2 file.txt          # Uses space as delimiter, extracts second field
cut --complement -f1 file.txt   # Extracts all fields except first
ls -l | cut -d' ' -f1,9         # Extracts permissions and filenames
cut -b1-20 file.txt             # Extracts first 20 bytes
```

---

### `sort`

**What it is:**
- Arranges lines of text files in order by various criteria
- Can merge pre-sorted files while maintaining order

**What it does:**
- Sorts lines alphabetically by default, but supports numeric and custom sorting
- Can sort very large files efficiently using external storage

**When to use:**
- Organizing data records by specific columns like dates or numerical IDs
- Sorting file contents before removing duplicates with uniq

**Popular flags with examples:**
```bash
sort file.txt                   # Sorts lines alphabetically
sort -n numbers.txt             # Sorts numerically (10 comes after 2)
sort -r file.txt                # Sorts in reverse order
sort -u file.txt                # Sorts and removes duplicate lines
sort -k2 file.txt               # Sorts by second field/column
sort -k2,2 -k1,1 file.txt       # Sorts by second field, then first field
sort -t: -k3 -n /etc/passwd     # Sorts by third field numerically with colon delimiter
sort -h sizes.txt               # Human-numeric sort (handles 1K, 2M, 3G)
sort -M months.txt              # Sorts by month names (JAN, FEB, etc.)
sort -f file.txt                # Case-insensitive sort
sort -b file.txt                # Ignores leading blank spaces
sort -o output.txt input.txt    # Sorts and writes to output file
```

---

### `uniq`

**What it is:**
- Filters out adjacent duplicate lines from sorted input
- Displays only unique lines or shows duplicate counts

**What it does:**
- Removes consecutive identical lines from input (only compares adjacent lines)
- Can count occurrences, display only duplicated lines, or show only unique lines

**When to use:**
- Counting how many times each error message appears in logs
- Removing duplicate entries from sorted lists of data

**Popular flags with examples:**
```bash
sort file.txt | uniq            # Removes duplicate lines from sorted input
sort file.txt | uniq -c         # Counts occurrences of each unique line
sort file.txt | uniq -d         # Shows only duplicated lines (appear more than once)
sort file.txt | uniq -u         # Shows only lines that appear exactly once
sort file.txt | uniq -i         # Case-insensitive comparison
sort file.txt | uniq -c | sort -rn  # Frequency count sorted by most common first
uniq -f 2 file.txt              # Ignores first 2 fields when comparing
uniq -s 5 file.txt              # Skips first 5 characters when comparing
uniq -w 10 file.txt             # Compares only first 10 characters
```

**Important:** `uniq` only removes adjacent duplicates, so you must sort first (`sort file | uniq`) to remove all duplicates.

---

### `tr`

**What it is:**
- Translates or deletes characters from standard input
- Performs character-by-character transformation as a filter in pipelines

**What it does:**
- Translates characters from one set to another, replacing each character
- Deletes specific characters or squeezes repeated characters into single instances

**When to use:**
- Converting lowercase letters to uppercase for case normalization
- Replacing spaces with underscores or converting DOS line endings to Unix format

**Popular flags with examples:**
```bash
tr 'a-z' 'A-Z' < file.txt       # Converts lowercase to uppercase
tr -d '0-9' < file.txt          # Deletes all digits from input
tr -s ' ' < file.txt            # Squeezes multiple spaces into single space
tr '\n' ',' < file.txt          # Replaces newlines with commas
tr -d '\r' < dosfile.txt        # Removes carriage returns (DOS to Unix)
tr -cd '[:print:]' < file.txt   # Keeps only printable characters
tr '[:lower:]' '[:upper:]'      # Converts to uppercase using character classes
tr -s '\n' < file.txt           # Removes blank lines by squeezing newlines
tr 'aeiou' '*' < file.txt       # Replaces vowels with asterisks
echo "hello world" | tr -d ' '  # Removes spaces: outputs "helloworld"
```

**Note:** `tr` reads from standard input only, so use `< file.txt` or pipe input to it.

---

### `paste`

**What it is:**
- Merges lines of files side by side
- Joins corresponding lines from multiple files with delimiters

**What it does:**
- Combines files horizontally by taking first line from each file and joining them
- Uses tab as default delimiter but can use any character to separate columns

**When to use:**
- Combining related datasets by merging first names with last names
- Joining identifiers with corresponding values to create analysis-ready datasets

**Popular flags with examples:**
```bash
paste file1.txt file2.txt       # Merges files side by side with tab separator
paste -d, file1.txt file2.txt   # Merges files with comma delimiter (CSV format)
paste -d'|' file1.txt file2.txt file3.txt  # Merges three files with pipe delimiter
paste -s file.txt               # Transposes: pastes all lines into single line
paste -s -d, file.txt           # Creates comma-separated list from lines
paste -d'\n' file1.txt file2.txt  # Interleaves lines from both files
paste - - < file.txt            # Pastes every two lines side by side
paste -d' ' names.txt ages.txt  # Creates space-separated records
```

---

### `join`

**What it is:**
- Merges lines from two sorted files based on a common field
- Similar to SQL joins but for text files

**What it does:**
- Joins lines from two files when they share a common key field
- Requires both input files to be sorted on the join field beforehand

**When to use:**
- Combining customer IDs from transactions with customer details to enrich data
- Merging test results with student information using student IDs

**Popular flags with examples:**
```bash
join file1.txt file2.txt        # Joins on first field (both must be sorted)
join -1 2 -2 1 file1.txt file2.txt  # Joins on 2nd field of file1, 1st of file2
join -t: file1.txt file2.txt    # Uses colon as field delimiter
join -a 1 file1.txt file2.txt   # Left outer join: includes unmatched from file1
join -a 2 file1.txt file2.txt   # Right outer join: includes unmatched from file2
join -v 1 file1.txt file2.txt   # Shows only lines from file1 without matches
join -v 2 file1.txt file2.txt   # Shows only lines from file2 without matches
join -o 1.1,2.2,1.3 file1.txt file2.txt  # Custom output format
join -i file1.txt file2.txt     # Case-insensitive join
```

---

### `diff`

**What it is:**
- Compares files line by line and displays differences
- Shows what changes would transform one file into the other

**What it does:**
- Analyzes two files and generates report showing additions, deletions, and modifications
- Can produce output in various formats including unified diffs and side-by-side

**When to use:**
- Reviewing code changes before committing to version control
- Comparing current server configurations with standard templates

**Popular flags with examples:**
```bash
diff file1.txt file2.txt        # Shows differences in default format
diff -u file1.txt file2.txt     # Unified diff format (used by git and patch)
diff -c file1.txt file2.txt     # Context diff format (shows surrounding lines)
diff -y file1.txt file2.txt     # Side-by-side comparison
diff -q file1.txt file2.txt     # Brief: only reports if files differ
diff -r dir1/ dir2/             # Recursively compares all files in directories
diff -i file1.txt file2.txt     # Case-insensitive comparison
diff -w file1.txt file2.txt     # Ignores whitespace differences
diff -b file1.txt file2.txt     # Ignores changes in whitespace amount
diff -B file1.txt file2.txt     # Ignores blank lines
diff --color file1.txt file2.txt  # Shows differences with color highlighting
```

---

### `comm`

**What it is:**
- Compares two sorted files line by line
- Produces three-column output showing unique and common lines

**What it does:**
- Displays comparison results in three columns: lines only in file1, only in file2, and in both
- Can suppress any combination of columns to show specific comparison results

**When to use:**
- Comparing user lists from different systems to identify differences
- Finding records that exist in one dataset but missing from another

**Popular flags with examples:**
```bash
comm file1.txt file2.txt        # Shows all three columns
comm -12 file1.txt file2.txt    # Shows only lines common to both files
comm -13 file1.txt file2.txt    # Shows only lines unique to file2
comm -23 file1.txt file2.txt    # Shows only lines unique to file1
comm -3 file1.txt file2.txt     # Shows only unique lines (suppress common)
```

**Important:** Both files must be sorted before using comm. Run `sort file1.txt > file1_sorted.txt` first if needed.

---

## File Permissions & Ownership

### `chmod`

**What it is:**
- Modifies file and directory permissions
- Controls who can read, write, or execute files using symbolic or numeric notation

**What it does:**
- Changes permission bits determining user, group, and other access rights
- Supports both absolute permission setting and relative changes

**When to use:**
- Making shell scripts executable or securing configuration files
- Setting appropriate permissions on upload directories for web applications

**Popular flags with examples:**
```bash
chmod +x script.sh              # Makes file executable for all users
chmod u+x script.sh             # Makes executable only for owner
chmod 755 script.sh             # Sets rwxr-xr-x permissions
chmod 644 file.txt              # Sets rw-r--r-- permissions
chmod -R 755 /var/www/html      # Recursively sets permissions on directory
chmod u+w,g-w,o-r file.txt      # Adds write for owner, removes write for group
chmod a+r file.txt              # Adds read permission for all
chmod 600 ~/.ssh/id_rsa         # Secures SSH private key (only owner read/write)
chmod g+s directory/            # Sets setgid bit: files inherit directory's group
chmod +t /tmp                   # Sets sticky bit: only owners can delete files
chmod 4755 program              # Sets setuid bit: program runs with owner's privileges
```

**Permission numbers:** 4=read, 2=write, 1=execute. Add them: 7=rwx, 6=rw-, 5=r-x, 4=r--, 0=---

---

### `chown`

**What it is:**
- Modifies file ownership (user owner and optionally group owner)
- Typically requires root privileges to execute

**What it does:**
- Transfers ownership of files from one user to another
- Can simultaneously change both user and group ownership

**When to use:**
- Transferring ownership to regular users after creating files as root
- Changing ownership of application files to web server user for deployments

**Popular flags with examples:**
```bash
sudo chown john file.txt        # Changes owner to user 'john'
sudo chown john:developers file.txt  # Changes owner and group
sudo chown :developers file.txt # Changes only group, keeps current owner
sudo chown -R www-data /var/www # Recursively changes ownership
sudo chown --reference=ref.txt target.txt  # Sets ownership to match reference
sudo chown -v john file.txt     # Verbose: shows what changes were made
sudo chown 1000:1000 file.txt   # Changes using numeric user and group IDs
```

---

### `chgrp`

**What it is:**
- Modifies the group ownership of files and directories
- Changes which group has group-level permissions

**What it does:**
- Changes the group associated with files for different user access
- Works similarly to chown but only for group ownership

**When to use:**
- Assigning shared project directories to project-specific groups
- Organizing files by purpose using groups for appropriate access control

**Popular flags with examples:**
```bash
chgrp developers file.txt       # Changes group to 'developers'
chgrp -R webadmin /var/www      # Recursively changes group
chgrp --reference=ref.txt target.txt  # Sets group to match reference file
chgrp -v developers file.txt    # Verbose: shows what changes were made
chgrp -c developers *.txt       # Shows only files that were actually changed
```

---

### `umask`

**What it is:**
- Sets the default permission mask for newly created files and directories
- Subtracts permissions from base defaults (666 for files, 777 for directories)

**What it does:**
- Defines which permission bits are turned off by default when creating files
- Provides security mechanism ensuring new files don't have overly permissive settings

**When to use:**
- Setting restrictive umasks in multi-user environments to keep files private
- Configuring umasks for web servers to allow appropriate read access

**Popular usage with examples:**
```bash
umask                           # Displays current umask value
umask 022                       # New files: 644 (rw-r--r--), new dirs: 755
umask 077                       # Restrictive: files 600, dirs 700
umask 002                       # Group-friendly: files 664, dirs 775
umask -S                        # Shows umask in symbolic notation
umask 0027                      # New files: 640, new dirs: 750
```

**How it works:** Subtract umask from base permissions. Files base: 666, Dirs base: 777. Umask 022: Files get 644, Dirs get 755.

---

### `getfacl`

**What it is:**
- Displays the access control list (ACL) of files and directories
- Shows extended permissions beyond standard user/group/other model

**What it does:**
- Retrieves and displays detailed ACL information including specific user and group permissions
- Shows both standard permissions and extended ACL entries with default ACLs

**When to use:**
- Troubleshooting access issues by examining detailed permission structures
- Verifying that sensitive files don't have unintended ACL entries

**Popular flags with examples:**
```bash
getfacl file.txt                # Shows ACL for file
getfacl -R /project/            # Recursively shows ACLs for directory
getfacl --omit-header file.txt  # Shows ACL without comment header
getfacl -t file.txt             # Shows ACL in tab-delimited format
getfacl -c file.txt             # Omits comment lines starting with #
getfacl -d directory/           # Shows default ACLs for new files in directory
getfacl -a file.txt             # Shows only access ACL, not default ACLs
```

---

### `setfacl`

**What it is:**
- Modifies access control lists on files and directories
- Grants specific permissions to individual users or groups beyond basic permissions

**What it does:**
- Adds, modifies, or removes extended permissions for specific named users and groups
- Sets default ACLs on directories that automatically apply to new files

**When to use:**
- Granting specific user read access without changing group membership
- Creating default ACLs ensuring new files inherit correct permissions

**Popular flags with examples:**
```bash
setfacl -m u:john:rw file.txt   # Grants user 'john' read+write permission
setfacl -m g:developers:rx dir/ # Grants group 'developers' read+execute
setfacl -m u:alice:rwx,g:team:rx file.txt  # Sets multiple ACL entries
setfacl -x u:john file.txt      # Removes ACL entry for user 'john'
setfacl -b file.txt             # Removes all extended ACL entries
setfacl -R -m u:bob:rw /project/  # Recursively grants permissions
setfacl -d -m g:developers:rwx dir/  # Sets default ACL for new files
setfacl --set u::rwx,g::rx,o::- file.txt  # Sets complete ACL (replaces existing)
setfacl -k dir/                 # Removes default ACLs from directory
```

---

### `lsattr`

**What it is:**
- Lists extended file attributes on Linux filesystems
- Shows special flags like immutable or append-only

**What it does:**
- Lists filesystem-level attributes that control file behavior beyond permissions
- Shows attributes specific to ext2/ext3/ext4 filesystems

**When to use:**
- Discovering why files cannot be deleted even as root
- Verifying that critical system files have protective attributes set

**Popular flags with examples:**
```bash
lsattr file.txt                 # Shows attributes for file
lsattr -a /etc                  # Shows attributes including hidden files
lsattr -d directory/            # Shows attributes of directory itself
lsattr -R /project/             # Recursively shows attributes for all files
lsattr -l file.txt              # Uses long format showing full attribute names
lsattr -v file.txt              # Shows file version/generation number
```

**Common attribute letters:** i=immutable, a=append-only, d=no-dump, s=secure-deletion, u=undeletable, c=compressed

---

### `chattr`

**What it is:**
- Modifies extended file attributes on Linux filesystems
- Sets special flags that control file behavior beyond permissions

**What it does:**
- Sets or removes filesystem attributes enforcing additional restrictions or behaviors
- Can make files completely immutable or restrict to append-only mode

**When to use:**
- Protecting critical configuration files from accidental modification
- Setting append-only mode on log files to prevent tampering

**Popular flags with examples:**
```bash
sudo chattr +i file.txt         # Makes file immutable (cannot be modified/deleted)
sudo chattr -i file.txt         # Removes immutable attribute
sudo chattr +a logfile.log      # Sets append-only: file can only grow
sudo chattr -a logfile.log      # Removes append-only attribute
sudo chattr +d file.txt         # Excludes file from dump (backup) operations
sudo chattr +s file.txt         # Enables secure deletion (overwrites with zeros)
sudo chattr +u file.txt         # Allows undelete: contents saved when deleted
sudo chattr -R +i /etc/config/  # Recursively makes all files immutable
sudo chattr +c file.txt         # Marks file for filesystem compression
```

**Note:** Requires root privileges. Immutable files cannot be modified even by root until attribute is removed.

---

## User & Group Management

### `useradd`

**What it is:**
- Creates new user accounts on the system
- Sets up user directories, assigns user IDs, and establishes initial configurations

**What it does:**
- Creates user accounts with unique UIDs, home directories, and default shell configurations
- Sets up initial user environment by copying skeleton files

**When to use:**
- Onboarding new employees by creating user accounts
- Creating service accounts for applications with specific UIDs

**Popular flags with examples:**
```bash
sudo useradd john               # Creates basic user account
sudo useradd -m john            # Creates user with home directory
sudo useradd -m -s /bin/bash john  # Creates user with specific shell
sudo useradd -m -d /custom/home/john john  # Custom home directory path
sudo useradd -m -G developers,admins john  # Adds to additional groups
sudo useradd -m -u 1500 john    # Creates user with specific UID
sudo useradd -m -e 2025-12-31 john  # Creates with account expiration date
sudo useradd -m -s /sbin/nologin serviceacct  # Service account that cannot login
sudo useradd -r mysqluser       # Creates system account (UID below 1000)
sudo useradd -m -k /etc/skel.custom john  # Uses custom skeleton directory
```

---

### `usermod`

**What it is:**
- Modifies existing user account properties
- Changes usernames, home directories, shells, and group memberships

**What it does:**
- Updates user account information without recreating the account
- Can lock/unlock accounts, change expiration dates, and modify group memberships

**When to use:**
- Modifying group memberships when users change departments
- Locking user accounts during security incidents

**Popular flags with examples:**
```bash
sudo usermod -aG developers john  # Adds user to group (keeps existing groups)
sudo usermod -G developers john   # Sets supplementary groups (removes others)
sudo usermod -g admins john       # Changes primary group
sudo usermod -d /new/home/john -m john  # Changes home directory and moves files
sudo usermod -s /bin/zsh john     # Changes login shell
sudo usermod -l johnsmith john    # Changes username
sudo usermod -L john              # Locks account (prevents login)
sudo usermod -U john              # Unlocks account
sudo usermod -e 2025-12-31 john   # Sets account expiration date
sudo usermod -u 2000 john         # Changes UID to 2000
```

---

### `userdel`

**What it is:**
- Removes user accounts from the system
- Optionally deletes user's home directory and mail spool

**What it does:**
- Removes user entries from system password and shadow files
- Can delete user's home directory and all associated files

**When to use:**
- Removing accounts when employees leave the organization
- Deleting old service accounts or test accounts during security cleanups

**Popular flags with examples:**
```bash
sudo userdel john               # Removes user account but leaves home directory
sudo userdel -r john            # Removes user and deletes home directory
sudo userdel -f john            # Forces deletion even if user is logged in
sudo userdel -r -f john         # Forcefully removes user and home directory
```

**Warning:** Files owned by deleted user outside home directory will have numeric UID. Use `find / -user UID -exec chown newowner {} \;` to reassign.

---

### `passwd`

**What it is:**
- Changes user passwords and manages password aging
- Primary mechanism for password management on Linux systems

**What it does:**
- Changes passwords with verification before accepting new password
- Configures password aging policies including expiration and warning periods

**When to use:**
- Resetting forgotten user passwords
- Setting password expiration policies for security compliance

**Popular flags with examples:**
```bash
passwd                          # Changes password for current user
sudo passwd john                # Changes password for user 'john'
sudo passwd -l john             # Locks account by disabling password
sudo passwd -u john             # Unlocks account, re-enabling password
sudo passwd -d john             # Deletes password (passwordless login - dangerous!)
sudo passwd -e john             # Expires password, forcing change on next login
sudo passwd -n 7 john           # Sets minimum days between password changes
sudo passwd -x 90 john          # Sets maximum password age to 90 days
sudo passwd -w 14 john          # Sets warning period to 14 days before expiration
sudo passwd -i 30 john          # Sets account lock after 30 days past expiration
sudo passwd -S john             # Shows password status information
```

---

### `groupadd`

**What it is:**
- Creates new groups on the system
- Establishes group identities with unique group IDs (GIDs)

**What it does:**
- Creates groups with unique GIDs for permission management
- Can create system groups or regular groups for user organization

**When to use:**
- Creating project-specific groups for team file access
- Establishing dedicated groups for web servers or databases

**Popular flags with examples:**
```bash
sudo groupadd developers        # Creates new group
sudo groupadd -g 3000 developers  # Creates group with specific GID
sudo groupadd -r mysqlgrp       # Creates system group (GID < 1000)
sudo groupadd -f developers     # Succeeds even if group already exists
sudo groupadd -o -g 1000 duplicate  # Creates group with duplicate GID
```

---

### `groupdel`

**What it is:**
- Removes groups from the system
- Deletes group entries from system group database

**What it does:**
- Deletes group entries from /etc/group and /etc/gshadow files
- Cannot delete groups that are any user's primary group

**When to use:**
- Removing project-specific groups after projects complete
- Deleting old service groups during system cleanup

**Popular flags with examples:**
```bash
sudo groupdel developers        # Deletes the group
sudo groupdel oldproject        # Removes unused project group
```

**Note:** Cannot delete if it's the primary group of any user. First change users' primary groups with `usermod -g`.

---

### `id`

**What it is:**
- Displays user and group identity information
- Shows user ID (UID), group ID (GID), and all supplementary groups

**What it does:**
- Shows numeric and text identifiers for user and all groups
- Can display information for any user on the system

**When to use:**
- Verifying which groups a user actually belongs to during troubleshooting
- Checking all group memberships during access control auditing

**Popular flags with examples:**
```bash
id                              # Shows UID, GID, and groups for current user
id john                         # Shows identity information for user 'john'
id -u                           # Shows only user ID (UID) number
id -g                           # Shows only primary group ID (GID)
id -G                           # Shows all group IDs user belongs to
id -un                          # Shows username (text, not number)
id -gn                          # Shows primary group name (text)
id -Gn                          # Shows all group names user belongs to
id -r                           # Shows real ID instead of effective ID
```

---

### `whoami`

**What it is:**
- Displays the username of the current effective user
- Shows who the system thinks you are for permission purposes

**What it does:**
- Prints username associated with current effective user ID
- Provides quick way to verify current user context

**When to use:**
- Verifying which user identity you're operating as after using sudo or su
- Checking if scripts are running as root or specific user

**Usage examples:**
```bash
whoami                          # Shows current effective username
sudo whoami                     # Shows 'root' when using sudo
su - john -c "whoami"           # Shows 'john' when running as switched user
```

---

### `who`

**What it is:**
- Displays information about users currently logged into the system
- Shows usernames, terminals, login times, and remote hosts

**What it does:**
- Lists all active login sessions including physical terminals and remote connections
- Shows where users logged in from and how long they've been logged in

**When to use:**
- Checking who is logged in before system maintenance
- Identifying active sessions during security audits

**Popular flags with examples:**
```bash
who                             # Shows all logged-in users
who -a                          # Shows all available information
who -b                          # Shows time of last system boot
who -H                          # Includes column headers
who -q                          # Quick mode: shows only usernames and count
who am i                        # Shows information about your own session
who -u                          # Shows idle time for each user
who -m                          # Shows only entry for current terminal
```

---

### `w`

**What it is:**
- Displays currently logged-in users and their activities
- Combines who, uptime, and current activity into one view

**What it does:**
- Shows system load averages, uptime, logged-in users, and their running processes
- Displays per-user statistics including CPU usage, idle time, and current commands

**When to use:**
- Checking what users are doing when system performance is slow
- Verifying logged-in users before performing maintenance

**Popular flags with examples:**
```bash
