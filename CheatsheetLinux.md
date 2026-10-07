Bash Cheat Sheet

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#bash-cheat-sheet)

A cheat sheet for bash commands.

> **Note:** A comma between command options indicates alternative forms.  
> Example: `ls -a, --all` means `ls -a` or `ls --all`.

## Command History

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#command-history)

```shell
!!            # Run the last command

CTRL+r        # Search bash history (hit CTRL+r multiple times to find multiple occurrences)

touch foo.sh              # Create foo.sh
chmod +x !$                # !$ is the last argument of the last command, i.e. foo.sh
```

## Navigating Directories

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#navigating-directories)

```shell
pwd                       # Print current directory path
ls                        # List files and directories
ls -a, --all             # List files and directories including hidden
ls -l                     # List files and directories in long form
ls -l -h, --human-readable # Long format with human-readable sizes
ls -t                     # List directories by modification time, newest first
stat foo.txt              # List size, created and modified timestamps for a file
stat foo                  # List size, created and modified timestamps for a directory
tree                      # List directory and file tree
tree -a                   # List directory and file tree including hidden
tree -d                   # List directory tree
cd foo                    # Go to foo sub-directory
cd /                      # Go to root directory
cd                        # Go to home directory
cd ~                      # Go to home directory
cd ..                     # Go to parent directory
cd -                      # Go to previous directory
pushd foo                 # Go to foo sub-directory and add previous directory to stack
popd                      # Go back to directory in stack saved by `pushd`

```
Warning: Some Windows `cd` habits do not work the same way in Bash.

- `cd \` is not the Linux equivalent of Windows `cd \`.
  In Bash, `\` is an escape character. Use `cd /` for the root directory.

- `cd..` does not work in Bash because `cd` and `..` must be separate arguments.
  Use `cd ..`.

- `cd/` is not the usual Bash syntax.
  Use `cd /`.
`dir` also exists on many Linux systems, but `ls` is the commonly used command.

## Creating Directories

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#creating-directories)

```shell
mkdir foo                        # Create a directory
mkdir foo bar                    # Create multiple directories
mkdir -p, --parents foo/bar       # Create nested directory
mkdir -p, --parents {foo,bar}/baz # Create multiple nested directories

mktemp -d, --directory            # Create a temporary directory
```

## Moving Directories

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#moving-directories)

```shell
cp -R, --recursive foo bar                               # Copy directory
mv foo bar                                              # Move directory

rsync -r, --recursive -z, --compress -v, --verbose /foo/ /bar/ # Copy directory recursively
rsync -a, --archive -z, --compress -v, --verbose /foo/ /bar/   # Copy directory in archive mode and preserve attributes
rsync -avz /foo username@hostname:/bar                  # Copy local directory to remote directory
rsync -avz username@hostname:/foo /bar                  # Copy remote directory to local directory
```

## Deleting Directories

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#deleting-directories)

```shell
rmdir foo                        # Delete empty directory
rm -r, --recursive foo            # Delete directory including contents
rm -r, --recursive -f, --force foo # Delete directory including contents, ignore nonexistent files and never prompt
```

## Creating Files

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#creating-files)

```shell
touch foo.txt          # Create file or update existing files modified timestamp
touch foo.txt bar.txt  # Create multiple files
touch {foo,bar}.txt    # Create multiple files
touch test{1..3}       # Create test1, test2 and test3 files
touch test{a..c}       # Create testa, testb and testc files

mktemp                 # Create a temporary file
```

## Standard Output, Standard Error and Standard Input

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#standard-output-standard-error-and-standard-input)

```shell
echo "foo" > bar.txt       # Overwrite file with content
echo "foo" >> bar.txt      # Append to file with content

ls exists 1> stdout.txt    # Redirect the standard output to a file
ls noexist 2> stderror.txt # Redirect the standard error output to a file
ls > out.txt 2>&1          # Redirect standard output and standard error to a file
ls > /dev/null 2>&1         # Discard standard output and standard error

read foo                   # Read from standard input and write to the variable foo
```

## Moving Files

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#moving-files)

```shell
cp foo.txt bar.txt                                # Copy file
mv foo.txt bar.txt                                # Move file

rsync -z, --compress -v, --verbose /foo.txt /bar    # Copy file quickly if not changed
rsync -z, --compress -v, --verbose /foo.txt /bar.txt # Copy and rename file quickly if not changed
```

## Deleting Files

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#deleting-files)

```shell
rm foo.txt            # Delete file
rm -f, --force foo.txt # Delete file, ignore nonexistent files and never prompt
```

## Reading Files

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#reading-files)

```shell
cat foo.txt            # Print all contents
less foo.txt           # Print some contents at a time (g - go to top of file, SHIFT+g, go to bottom of file, /foo to search for 'foo')
head foo.txt           # Print top 10 lines of file
tail foo.txt           # Print bottom 10 lines of file
xdg-open foo.txt       # Open file with the default desktop application
wc foo.txt             # List number of lines, words and bytes in the file
```

## File Permissions

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#file-permissions)

| #   | Permission | rwx | Binary |
| --- | --- | --- | --- |
| 7   | read, write and execute | rwx | 111 |
| 6   | read and write | rw- | 110 |
| 5   | read and execute | r-x | 101 |
| 4   | read only | r-- | 100 |
| 3   | write and execute | -wx | 011 |
| 2   | write only | -w- | 010 |
| 1   | execute only | --x | 001 |
| 0   | none | --- | 000 |

For a directory, execute means you can enter a directory.

| User | Group | Others | Description |
| --- | --- | --- | --- |
| 6   | 4   | 4   | User can read and write, everyone else can read (typical file permissions with umask 022) |
| 7   | 5   | 5   | User can read, write and execute, everyone else can read and execute (typical directory permissions with umask 022) |

- u - User
- g - Group
- o - Others
- a - All of the above

```shell
ls -l /foo.sh            # List file permissions
chmod 744 foo.sh         # Set permissions to rwxr--r--
chmod 644 foo.sh         # Set permissions to rw-r--r--
chmod u+x foo.sh         # Give the user execute permission
chmod g+x foo.sh         # Give the group execute permission
chmod u-x,g-x foo.sh     # Take away the user and group execute permission
chmod u+x,g+x,o+x foo.sh # Give everybody execute permission
chmod a+x foo.sh         # Give everybody execute permission
chmod +x foo.sh          # Add execute permission, affected by the current umask
```

## Finding Files

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#finding-files)

Find binary files for a command.

```shell
type wget                                  # Find the binary
which wget                                 # Find the binary
whereis wget                               # Find the binary, source, and manual page files
```

`locate` uses an index and is fast.

```shell
sudo updatedb                              # Update the locate index

locate foo.txt                             # Find a file
locate -i, --ignore-case foo.txt           # Find a file and ignore case
locate 'f*.txt'                            # Find a text file starting with 'f'
```

`find` doesn't use an index and is slow.

```shell
find /path -name foo.txt                   # Find a file
find /path -iname foo.txt                  # Find a file with case insensitive search
find /path -name "*.txt"                   # Find all text files
find /path -name foo.txt -delete           # Find a file and delete it
find /path -name "*.png" -exec pngquant {} \; # Find all .png files and execute pngquant on each one
find /path -type f -name foo.txt           # Find a file
find /path -type d -name foo               # Find a directory
find /path -type l -name foo.txt           # Find a symbolic link
find /path -type f -mtime +30              # Find files that haven't been modified in 30 days
find /path -type f -mtime +30 -delete      # Delete files that haven't been modified in 30 days
```

## Find in Files

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#find-in-files)

```shell
grep 'foo' /bar.txt                         # Search for 'foo' in file 'bar.txt'
grep -r, --recursive 'foo' /bar              # Search for 'foo' in directory 'bar'
grep -R, --dereference-recursive 'foo' /bar  # Search for 'foo' in directory 'bar' and follow symbolic links
grep -l, --files-with-matches 'foo' /bar     # Show only files that match
grep -L, --files-without-match 'foo' /bar    # Show only files that don't match
grep -i, --ignore-case 'Foo' /bar            # Case-insensitive search
grep -x, --line-regexp 'foo' /bar            # Match the entire line
grep -C 1, --context=1 'foo' /bar            # Add one line of context above and below each result
grep -v, --invert-match 'foo' /bar           # Show only lines that don't match
grep -c, --count 'foo' /bar                  # Count matching lines
grep -n, --line-number 'foo' /bar            # Add line numbers
grep --color=auto 'foo' /bar                 # Add colour to output
grep -R 'foo\|bar' /baz                     # Search for 'foo' or 'bar' using a basic regular expression
grep -R -E, --extended-regexp 'foo|bar' /baz # Use extended regular expressions
egrep -R 'foo|bar' /baz                     # Legacy alias; prefer grep -E
```

### Replace in Files

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#replace-in-files)

```shell
sed 's/fox/bear/g' foo.txt               # Replace fox with bear in foo.txt and output to console
sed 's/fox/bear/gi' foo.txt              # Replace fox (case insensitive) with bear in foo.txt and output to console
sed 's/red fox/blue bear/g' foo.txt      # Replace the exact text 'red fox' with 'blue bear'
sed 's/fox/bear/g' foo.txt > bar.txt     # Replace fox with bear in foo.txt and save in bar.txt
sed -i, --in-place 's/fox/bear/g' foo.txt # Replace fox with bear and overwrite foo.txt
```

## Symbolic Links

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#symbolic-links)

```shell
ln -s, --symbolic foo bar            # Create a link 'bar' to the 'foo' folder
ln -s, --symbolic -f, --force foo bar # Overwrite an existing symbolic link 'bar'
ls -l                               # Show where symbolic links are pointing
```

## Compressing Files

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#compressing-files)

### zip

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#zip)

Compresses one or more files into *.zip files.

```shell
zip foo.zip /bar.txt                # Compress bar.txt into foo.zip
zip foo.zip /bar.txt /baz.txt       # Compress bar.txt and baz.txt into foo.zip
zip foo.zip /{bar,baz}.txt          # Compress bar.txt and baz.txt into foo.zip
zip -r, --recurse-paths foo.zip /bar # Compress directory bar into foo.zip
```

### gzip

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#gzip)

Compresses a single file into *.gz files.

```shell
gzip /bar.txt                  # Compress bar.txt into bar.txt.gz and delete the original
gzip -k, --keep /bar.txt       # Compress bar.txt into bar.txt.gz and keep the original
gzip -c /bar.txt > foo.gz      # Compress bar.txt and write the result to foo.gz
```

### tar -c

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#tar--c)

Compresses (optionally) and combines one or more files into a single *.tar, *.tar.gz, *.tpz or *.tgz file.

```shell
tar -czf foo.tgz /bar.txt /baz.txt              # Create a gzip-compressed archive from two files
tar -czf foo.tgz /{bar,baz}.txt                  # Create a gzip-compressed archive using brace expansion
tar -czf foo.tgz /bar                            # Create a gzip-compressed archive from a directory
tar --create --gzip --file=foo.tgz /bar          # Same using long options
```

## Decompressing Files

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#decompressing-files)

### unzip

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#unzip)

```shell
unzip foo.zip          # Unzip foo.zip into current directory
```

### gunzip

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#gunzip)

```shell
gunzip foo.gz           # Unzip foo.gz into current directory and delete foo.gz
gunzip -k, --keep foo.gz # Unzip foo.gz into current directory
```

### tar -x

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#tar--x)

```shell
tar -xzf foo.tar.gz       # Extract a gzip-compressed tar archive
tar -xf foo.tar            # Extract an uncompressed tar archive
```

## Disk Usage

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#disk-usage)

```shell
df                     # List disks, size, used and available space
df -h, --human-readable # List disks, size, used and available space in a human readable format

du                     # List current directory, subdirectories and file sizes
du /foo/bar            # List specified directory, subdirectories and file sizes
du -h, --human-readable # List current directory, subdirectories and file sizes in a human readable format
du -d 1, --max-depth=1  # List sizes down to a maximum depth of 1
du -d 0, --max-depth=0  # List only the current directory size
```

## Memory Usage

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#memory-usage)

```shell
free                   # Show memory usage
free -h, --human        # Show human readable memory usage
free -h, --human --si   # Show human readable memory usage in power of 1000 instead of 1024
free -s, --seconds 5    # Show memory usage and update continuously every five seconds
```

## Logs & System Debugging

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#logs--system-debugging)

```shell
journalctl -u nginx          # Logs for a specific service
journalctl -xe               # Recent log entries with additional context
journalctl -f                # Follow logs in real time
journalctl -k                # Kernel logs from the systemd journal

dmesg | tail                 # Recent kernel messages
tail -f /var/log/syslog     # Live system logs on many Debian/Ubuntu systems
tail -f /var/log/messages   # Live system logs on many RHEL/Fedora/CentOS Stream systems
```

## Packages

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#packages)

```shell
sudo apt update              # Refresh repository index
apt search wget              # Search for a package
apt show wget                # List information about the wget package
apt list --all-versions wget # List all versions of the package
sudo apt install wget        # Install the latest version of the wget package
sudo apt install wget=1.2.3  # Install a specific version of the wget package
sudo apt remove wget         # Remove the wget package
sudo apt upgrade             # Upgrade all upgradable packages
```

## Shutdown and Reboot

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#shutdown-and-reboot)

```shell
shutdown                     # Shutdown in 1 minute
shutdown now "Cya later"     # Immediately shut down
shutdown +5 "Cya later"      # Shutdown in 5 minutes

shutdown --reboot            # Reboot in 1 minute
shutdown -r now "Cya later"  # Immediately reboot
shutdown -r +5 "Cya later"   # Reboot in 5 minutes

shutdown -c                  # Cancel a shutdown or reboot

reboot                       # Reboot now
reboot -f                    # Force a reboot
```

## Identifying Processes

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#identifying-processes)

```shell
top                    # List all processes interactively
htop                   # List all processes interactively
ps aux                 # List processes in BSD-style format
pidof foo              # Return the PID of all foo processes

CTRL+Z                 # Suspend the foreground process (SIGTSTP)
bg                     # Resume a suspended process and run in the background
fg                     # Bring the last background process to the foreground
fg %1                  # Bring job 1 to the foreground

sleep 30 &             # Sleep for 30 seconds and move the process into the background
jobs                   # List all background jobs
jobs -p                # List all background jobs with their PID

lsof                   # List all open files and the process using them
lsof -itcp:4000        # Return the process listening on port 4000
```

## Process Priority

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#process-priority)

Process priorities go from -20 (highest) to 19 (lowest).

```shell
nice -n 10 foo         # Start foo with nice value 10
sudo nice -n -20 foo   # Start foo with highest priority (requires privileges)
renice -n 19 -p PID    # Set the nice value of PID to 19
ps -o ni PID           # Show the nice value of PID
```

## Killing Processes

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#killing-processes)

```shell
CTRL+C                 # Send SIGINT to the foreground process (usually terminates it)
kill PID               # Shut down process by PID gracefully. Sends TERM signal.
kill -9 PID            # Force shut down of process by PID. Sends SIGKILL signal.
pkill foo              # Shut down process by name gracefully. Sends TERM signal.
pkill -9 foo           # force shut down process by name. Sends SIGKILL signal.
killall foo            # Kill all process with the specified name gracefully.
```

## Date & Time

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#date--time)

```shell
date                   # Print the date and time
date --iso-8601        # Print the ISO8601 date
date --iso-8601=ns     # Print the ISO8601 date and time

time tree              # Time how long the tree command takes to execute
```

## Scheduled Tasks

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#scheduled-tasks)

```
   *      *         *         *           *
Minute, Hour, Day of month, Month, Day of the week
```

```shell
crontab -l                 # List cron tab
crontab -e                 # Edit cron tab using the configured editor
crontab /path/crontab      # Load cron tab from a file
crontab -l > /path/crontab # Save cron tab to a file

* * * * * foo              # Run foo every minute
*/15 * * * * foo           # Run foo every 15 minutes
0 * * * * foo              # Run foo every hour
15 6 * * * foo             # Run foo daily at 6:15 AM
44 4 * * 5 foo             # Run foo every Friday at 4:44 AM
0 0 1 * * foo              # Run foo at midnight on the first of the month
0 0 1 1 * foo              # Run foo at midnight on the first of the year

at -l                      # List scheduled tasks
at -c 1                    # Show task with ID 1
at -r 1                    # Remove task with ID 1
at now + 2 minutes         # Open the at> prompt for a task that runs in 2 minutes
at 12:34 PM next month     # Open the at> prompt for 12:34 PM next month
at tomorrow                # Open the at> prompt for a task that runs tomorrow
```

## HTTP Requests

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#http-requests)

```shell
curl https://example.com                               # Return response body
curl -i, --include https://example.com                        # Include status line and HTTP headers
curl -L, --location https://example.com                       # Follow redirects
curl -o foo.txt, --output foo.txt https://example.com         # Save output using a chosen file name
curl -O, --remote-name https://example.com/file.txt           # Save using the remote file name
curl -H "User-Agent: Foo", --header "User-Agent: Foo" https://example.com # Add an HTTP header
curl -X POST, --request POST -H "Content-Type: application/json" -d '{"foo":"bar"}' https://example.com # POST JSON
curl --data-urlencode 'foo=bar' https://example.com           # POST URL-encoded form data

wget https://example.com/file.txt                             # Download a file to the current directory
wget -O foo.txt, --output-document=foo.txt https://example.com/file.txt # Save using a chosen file name
```

## Network Troubleshooting

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#network-troubleshooting)

```shell
ping example.com            # Send multiple ping requests using the ICMP protocol
ping -c 10 -i 5 example.com # Make 10 attempts, 5 seconds apart

ip addr                     # List IP addresses on the system
ip route show               # Show the routing table

netstat -i, --interfaces     # List network interfaces (legacy net-tools command)
netstat -l, --listening      # List listening sockets (legacy net-tools command)
ss -tuln                    # List listening TCP and UDP sockets (modern alternative)

traceroute example.com      # Show the network hops to a destination

mtr -w, --report-wide example.com                                    # Continually list all servers the network traffic goes through
mtr -r, --report -w, --report-wide -c, --report-cycles 100 example.com # Output a report that lists network traffic 100 times

nmap localhost              # Scan the 1000 most common ports on localhost
nmap localhost -p 1-65535   # Scan ports 1 through 65535 on localhost
nmap 192.168.4.3            # Scan the 1000 most common ports on a remote IP address
nmap -sn 192.168.1.0/24     # Host discovery without a port scan

nmtui                       # NetworkManager Text User Interface
```

## DNS

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#dns)

```shell
host example.com            # Show the IPv4 and IPv6 addresses

dig example.com             # Show complete DNS information

cat /etc/resolv.conf        # Show resolver configuration; often managed automatically
```

## Hardware

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#hardware)

```shell
lsusb                  # List USB devices
lspci                  # List PCI hardware
lshw                   # List all hardware
```

## Terminal Multiplexers

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#terminal-multiplexers)

Start multiple terminal sessions. Detached sessions survive terminal or SSH disconnects, but not a system reboot. `tmux` is more modern than `screen`.

```shell
tmux             # Start a new session (CTRL-b + d to detach)
tmux ls          # List all sessions
tmux attach -t 0 # Reattach to a session

screen           # Start a new session (CTRL-a + d to detach)
screen -ls       # List all sessions
screen -R 31166  # Reattach to a session

exit             # Exit a session
```

## Secure Shell Protocol (SSH)

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#secure-shell-protocol-ssh)

```shell
ssh hostname                 # Connect to hostname using your current user name over the default SSH port 22
ssh -i foo.pem hostname      # Connect to hostname using the identity file
ssh user@hostname            # Connect to hostname using the user over the default SSH port 22
ssh user@hostname -p 8765    # Connect to hostname using the user over a custom port
ssh ssh://user@hostname:8765 # Connect to hostname using the user over a custom port
```

Set default user and port in `~/.ssh/config`, so you can just enter the name next time:

```text
Host name
  HostName 127.0.0.1
  User foo
  Port 8765
```

Then connect with:

```shell
ssh name
```

## Secure Copy

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#secure-copy)

```shell
scp foo.txt ubuntu@hostname:/home/ubuntu # Copy foo.txt into the specified remote directory
```

## Bash Profile

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#bash-profile)

- bash - `.bashrc`
- zsh - `.zshrc`

```shell
# Always run ls after cd
cd() { builtin cd "$@" && ls; }

# Prompt user before overwriting any files
alias cp='cp --interactive'
alias mv='mv --interactive'
alias rm='rm --interactive'

# Always show disk usage in a human readable format
alias df='df -h'
alias du='du -h'
```

## Bash Script

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#bash-script)

### Variables

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#variables)

```shell
#!/bin/bash

foo=123                # Initialize variable foo with 123
declare -i foo=123     # Initialize an integer foo with 123
declare -r foo=123     # Initialize readonly variable foo with 123
echo $foo              # Print variable foo
echo ${foo}_'bar'      # Print variable foo followed by _bar
echo ${foo:-'default'} # Print variable foo if it exists otherwise print default

export foo             # Make foo available to child processes
unset foo              # Make foo unavailable to child processes
```

### Environment Variables

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#environment-variables)

```shell
#!/bin/bash

env            # List all environment variables
echo $PATH     # Print PATH environment variable
export FOO=Bar # Set an environment variable
```

### Functions

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#functions)

```shell
#!/bin/bash

greet() {
  local world="World"
  echo "$1 $world"
}

greet "Hello"
greeting=$(greet "Hello")
```

### Exit Codes

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#exit-codes)

```shell
#!/bin/bash

exit 0   # Exit the script successfully
exit 1   # Exit the script unsuccessfully
echo $?  # Print the last exit code
```

### Conditional Statements

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#conditional-statements)

#### Boolean Operators

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#boolean-operators)

- `[[ $foo ]]` - True if the string in `foo` is non-empty
- `[[ ! $foo ]]` - Negates the test

#### Numeric Operators

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#numeric-operators)

- `-eq` - Equals
- `-ne` - Not equals
- `-gt` - Greater than
- `-ge` - Greater than or equal to
- `-lt` - Less than
- `-le` - Less than or equal to

#### File Operators

- `-e foo.txt` - File or directory exists
- `-f foo.txt` - Regular file exists
- `-d foo` - Directory exists

#### String Operators

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#string-operators)

- `=` - Equals
- `==` - Equals
- `-z` - Is null
- `-n` - Is not null
- `<` - Is less than in ASCII alphabetical order
- `>` - Is greater than in ASCII alphabetical order

#### If Statements

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#if-statements)

```shell
#!/bin/bash

if [[ $foo == 'bar' ]]; then
  echo 'one'
elif [[ $foo == 'baz' ]] || [[ $foo == 'bat' ]]; then
  echo 'two'
elif [[ $foo == 'ban' ]] && [[ $USER == 'rehan' ]]; then
  echo 'three'
else
  echo 'four'
fi
```

#### Inline If Statements

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#inline-if-statements)

```shell
#!/bin/bash

[[ $USER = 'rehan' ]] && echo 'yes' || echo 'no'
```

#### While Loops

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#while-loops)

```shell
#!/bin/bash

declare -i counter=10
while [[ $counter -gt 2 ]]; do
  echo "The counter is $counter"
  ((counter--))
done
```

#### For Loops

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#for-loops)

```shell
#!/bin/bash

for i in {0..10..2}; do
  echo "Index: $i"
done

for filename in file1 file2 file3; do
  echo "Content: " >> "$filename"
done

for filename in *; do
  echo "Content: " >> "$filename"
done
```

#### Case Statements

[](https://github.com/RehanSaeed/Bash-Cheat-Sheet#case-statements)

```shell
#!/bin/bash

echo "What's the weather like tomorrow?"
read weather

case $weather in
  sunny | warm)
    echo "Nice weather: $weather"
    ;;
  cloudy | cool)
    echo "Not bad weather: $weather"
    ;;
  rainy | cold)
    echo "Terrible weather: $weather"
    ;;
  *)
    echo "Don't understand"
    ;;
esac
```
