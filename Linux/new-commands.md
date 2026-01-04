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
