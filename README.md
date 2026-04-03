# Understanding Red Hat Enterprise Linux
### Basics
The big thing about RHEL is support. The Red Hat company can provide support for paying companies.
This support is obtained via support licenses.
RHEL started in 1993 and is the most successful enterprise linux version available.
A new version of RHEL comes out every 3 years or so.
You can obtain support for up to 10 years
You can have free access to RHEL (unsupported) for up to 16 instances of RHEL (using a Developer account)
### The RHEL family
`Fedora` - latest and greatest, but less stable
`CentOS` Stream - a little more stable than fedora, but more cutting edge than RHEL
`RHEL` is derived from CentOS
`Oracle` and `Amazon linux` are derived from RHEL
### Custom Partitioning
Linux servers need 3 different areas for storing data
-  small partition for linux kernel
-  UEFI bootlogger files
- `/root` - OS essential files
# Using the Command Line
`bash` is often the default shell
`terminal` is an environment which runs the shell
`who` command can be used to display who is logged in
`cockpit` is a useful interface, but you should not rely too heavily on it
# Essential tools
### man
best source to get extensive info on your system (especially if you don't have internet access)
These are formatted into sections
- section 1 - executable programs/commands
- section 5 - file formats and conventions
- section 8 - system admin commands (sudo)
You can use `/text_here` to search for a string in the man page
You can also use uppercase `G` to show examples
`man` is indexed from `mandb`
You can populate mandb using `sudo mandb`
### editors
Linux configuration happens by modifying text files
- plain ASCII is used often
- YAML is often used for Ansible
- JSON is often good for data storage formats
- XML is an alternative to both of the above
`nano` and `vim` are very common text editors
`nano` is very easy to use
`vim` is more complex and very powerful
# Understanding the Bash Shell
### Using I/O Redirection and Piping
Redirection uses `STDIN`, `STDERR`, and `STDOUT` to work with command output.

| Command              | Where to redirect output                                |
| -------------------- | ------------------------------------------------------- |
| `>`                  | Redirects output to a file                              |
| `>>`                 | Appends output to a file                                |
| `2> /dev/null`       | Send errors to /dev/null                                |
| `ps aux \| grep ssh` | Sending output to then be processed by  another command |
### History
Bash registers command history
The `command` history will print out recently used commands
The `HISTSIZE` and `HISTFILESIZE` are variables to define how long it should be (1000 is default)
`history -w` syncs history memory to the history file
### Shell Expansion
Globbing expands filename based on wildcards
- `ls *`
- `ls a?*`
- `ls [a-e]*`

Other types of expansion also exist
- brace expansion: `touch file{1..9}`
- Tilde expansion: `cd ~`
- Command substitution: `ls -l $(which ls)`
- variable substitution: `echo $PATH`
### Escaping Special Characters
Escaping is taking away the special meaning of certain characters that otherwise do something.

| Item                           | Description                                                                            |
| ------------------------------ | -------------------------------------------------------------------------------------- |
| Double quotes<br>`"text here"` | Suppresses globbing and shell expansion, but allows commands and variable substitution |
| Single Quotes<br>`'text here'` | Suppresses special meaning of everything.                                              |
| Backslash<br>`\`               | Protects the following character from expansion                                        |
### Variables
These are items to be referenced and re-used
The command `env` will show the current variables
Users can define new variables using `[export] key=value` (mostly useful for scripting)
```bash
[bradley@cachyos ~]$ color=blue
[bradley@cachyos ~]$ echo $color
blue
```
`$PATH` is an example of a variable
### Using alias
alias can be used to define custom commands
```bash
[bradley@cachyos ~]$ alias print="ls -la"
[bradley@cachyos ~]$ print
total 40
drwx--x---+ 1 bradley bradley  482 Mar 15 16:37 .
drwxr-xr-x  1 root    root      14 Mar  2 12:20 ..
-rw-------  1 bradley bradley   84 Mar 10 19:07 .bash_history
-rw-r--r--  1 bradley bradley   21 Mar  2 12:20 .bash_logout
-rw-r--r--  1 bradley bradley   57 Mar  2 12:20 .bash_profile
-rw-r--r--  1 bradley bradley  172 Mar  2 12:20 .bashrc
drwx------  1 bradley bradley 1300 Mar 17 13:13 .cache
drwxr-xr-x  1 bradley bradley 1606 Mar 17 15:32 .config
drwxr-xr-x  1 bradley bradley  430 Mar 10 15:55 Desktop
drwxr-xr-x  1 bradley bradley  160 Mar 11 15:48 Documents
drwxr-xr-x  1 bradley bradley  572 Mar 17 12:30 Downloads

```
you can remove an alias with `unalias`
custom aliases are stored in bash startup files
### Tuning the bash environment
The following is to make variables and alias persistent
`/etc/profile` is the generic bash file that are processed in a login shell
`/etc/bashrc` is processed while opening a subshell
`~/.bash_profile` is the *user specific* way to customize
`~/.bashrc` same thing
`~/.bash_logout` commands you want to execute when you log out
# Basic System Admin tasks
## File Management
### Directory Structure
The directory hierarchy is highly standardized
The command `man hier` will print out descriptions

| Directory | Purpose                                                             |
| --------- | ------------------------------------------------------------------- |
| `/`       | Root directory of the filesystem                                    |
| `/bin`    | Essential user command binaries (e.g., `ls`, `cp`)                  |
| `/boot`   | Boot loader files and kernel images                                 |
| `/dev`    | Device files representing hardware and virtual devices              |
| `/etc`    | Configuration files                                                 |
| `/home`   | Home directories for regular users                                  |
| `/lib`    | Shared libraries and kernel modules                                 |
| `/media`  | Mount points for removable media (USB, CD-ROM, etc.)                |
| `/mnt`    | Temporary mount points for filesystems                              |
| `/opt`    | Optional or third-party software packages                           |
| `/proc`   | Virtual filesystem providing process and kernel information         |
| `/root`   | Home directory of the root (superuser)                              |
| `/run`    | Runtime variable data (since last boot, e.g., PID files, sockets)   |
| `/sbin`   | Essential system binaries (administrative commands)                 |
| `/srv`    | Data for services (e.g., web, FTP)                                  |
| `/sys`    | Virtual filesystem exposing kernel objects                          |
| `/tmp`    | Temporary files (automatically cleared, often within 30 days)       |
| `/usr`    | User commands, applications, libraries, documentation               |
| `/var`    | Variable data that persists (logs, spool files, caches, mail, etc.) |
### File Management Commands

| Command | Description               |
| ------- | ------------------------- |
| `ls`    | list files                |
| `mkdir` | make a directory          |
| `cp`    | copy files                |
| `mv`    | move files                |
| `rmdir` | remove an empty directory |
### Finding Files

| Command  | Description                                                                                                                              |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `which`  | looks for binaries in $PATH                                                                                                              |
| `locate` | uses the updatedb database                                                                                                               |
| `find`   | most powerful command, but requires some more input<br>Example: <br># Find files by extension:<br>`find path/to/directory -name '*.ext'` |
### Mounting Filesystems
To access a device like a partition, it must be connected to a directory otherwise known as *mounting* a device.
The Linux file-system often uses multiple mounts

To manually mount a device, you can use the following command.
```bash
mount /dev/devicename /directory
```
Example: 
```bash
mount /dev/sdb1 /mnt
```
The `mount` command gives an overview of all mounts
It's a little too much info for most occasions
```bash
[bradley@cachyos ~]$ mount  
/dev/nvme0n1p2 on / type btrfs (rw,noatime,compress=zstd:1,ssd,discard=async,space_cache=v2,subvolid=256,subvol=/@)  
devtmpfs on /dev type devtmpfs (rw,nosuid,size=15779964k,nr_inodes=3944991,mode=755,inode64,huge=within_size)  
tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev,inode64,huge=within_size,usrquota)  
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=600,ptmxmode=000)  
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
# more below ...
```
The `findmnt` command shows all currently mounted devices and their place in the file-system.
```bash
[bradley@cachyos ~]$ findmnt
TARGET                       SOURCE               FSTYPE      OPTIONS
/                            /dev/nvme0n1p2[/@]   btrfs       rw,noatime,compress=zstd:1,ssd,discard=async,space_cache=v2,subvolid=256,subvol=/@
├─/dev                       devtmpfs             devtmpfs    rw,nosuid,size=15779964k,nr_inodes=3944991,mode=755,inode64,huge=within_size
│ ├─/dev/hugepages           hugetlbfs            hugetlbfs   rw,nosuid,nodev,relatime,pagesize=2M
│ ├─/dev/mqueue              mqueue               mqueue      rw,nosuid,nodev,noexec,relatime
│ ├─/dev/shm                 tmpfs                tmpfs       rw,nosuid,nodev,inode64,huge=within_size,usrquota
│ └─/dev/pts                 devpts               devpts      rw,nosuid,noexec,relatime,gid=5,mode=600,ptmxmode=000
├─/sys                       sysfs                sysfs       rw,nosuid,nodev,noexec,relatime
│ ├─/sys/kernel/tracing      tracefs              tracefs     rw,nosuid,nodev,noexec,relatime
│ ├─/sys/kernel/debug        debugfs              debugfs     rw,nosuid,nodev,noexec,relatime
│ ├─/sys/kernel/config       configfs             configfs    rw,nosuid,nodev,noexec,relatime
# more below ...
```
The `lsblk` command gives a clean overview of real devices and their current mount in the directory structure
```bash
student@rhcsa1:~$ lsblk
NAME          MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sr0            11:0    1 1024M  0 rom  
vda           252:0    0   40G  0 disk 
├─vda1        252:1    0    1M  0 part 
├─vda2        252:2    0    1G  0 part /boot
└─vda3        252:3    0   39G  0 part 
  ├─rhel-root 253:0    0   35G  0 lvm  /
  └─rhel-swap 253:1    0    4G  0 lvm  [SWAP]

```
### Links
This will show the inode number in a list format
```bash
ls -il
```
```bash
student@rhcsa1:/$ ln --help
Usage: ln [OPTION]... [-T] TARGET LINK_NAME
  or:  ln [OPTION]... TARGET
  or:  ln [OPTION]... TARGET... DIRECTORY
  or:  ln [OPTION]... -t DIRECTORY TARGET...
In the 1st form, create a link to TARGET with the name LINK_NAME.
In the 2nd form, create a link to TARGET in the current directory.
In the 3rd and 4th forms, create links to each TARGET in DIRECTORY.
Create hard links by default, symbolic links with --symbolic.

```
#### hard links
A name that is pointing directly to a inode
You can have several that point to the same inode
These must be on the same device
The target **cannot** be a directory
Example of use: 6 links pointing to the same script that needs to be changed often
#### symbolic links
These point to a name
Also called a soft link
If the target is removed, then this will point to nothing
The target **can** be a directory
The best practice is to point to an **absolute path** instead of a **relative path**
`bin`, `lib`, `lib64` are symlinks in the example below
```bash
student@rhcsa1:/$ ls -l /
total 24
dr-xr-xr-x.   2 root root    6 Apr  1  2025 afs
lrwxrwxrwx.   1 root root    7 Apr  1  2025 bin -> usr/bin
dr-xr-xr-x.   5 root root 4096 Mar 15 20:33 boot
drwxr-xr-x.  21 root root 3500 Mar 21 12:28 dev
drwxr-xr-x. 140 root root 8192 Mar 21 12:28 etc
drwxr-xr-x.   3 root root   21 Mar 15 20:28 home
lrwxrwxrwx.   1 root root    7 Apr  1  2025 lib -> usr/lib
lrwxrwxrwx.   1 root root    9 Apr  1  2025 lib64 -> usr/lib64
```
### Archiving Files
`tar` is the **Tape Archiver** and was created a long time ago
It doesn't compress data by default
```bash
student@rhcsa1:/$ tar --help
Usage: tar [OPTION...] [FILE]...
GNU 'tar' saves many files together into a single tape or disk archive, and can
restore individual files from the archive.

Examples:
  tar -cf archive.tar foo bar  # Create archive.tar from files foo and bar.
  tar -tvf archive.tar         # List all files in archive.tar verbosely.
  tar -xf archive.tar          # Extract all files from archive.tar.
```
`gzip` (`-z` argument for tar) is the most common compression utility
## Managing text Files
### Common text tools
#### more
the original file pager
#### less
was developed to offer some more advanced features
you can use the arrows to go up and down
#### head
show first 10 lines
use `-n 20` to shows 20
#### tail 
shows last 10 lines
use `-n 20` to shows 20
use `-f` to follow (useful to follow new additions to a file in real time)
```bash
tail -f /var/log/messages
```
#### cat
dumps text file to screen
#### cut
filters output
Example
```bash
# Print the fifth character on each line:
cut [-c|--characters] 5
```
#### sort
sorts output
Example
```bash
# Sort a file in ascending order:
sort path/to/file
```
#### tr
Translate characters: run replacements based on single characters and character sets.
Example:
```bash
# Replace all occurrences of a character in a file, and print the result:
tr < path/to/file find_character replace_character

```
### grep
tool to find text in output and files
```bash
ps aux | grep firefox
```
Search for a pattern within files:  
```bash
grep "search_pattern" path/to/file1 path/to/file2 ...
```
### Regular Expressions (regex)
Always put regex between single quotes
Don't confuse these with globbing (shell wildcards)
For use with some of the following tools:
- grep
- vim
- awk
- sed
For more info
```bash
man regex
```
Example: For lines that start with L
```bash
grep '^l' filename
```
Example: For lines that end with anna
```bash
grep 'anna$'
```
### awk
powerful text processing utility that specialized in data extraction and reporting
Example: Print the fifth column (a.k.a. field) in a space-separated file:
```bash
awk '{print $5}' path/to/file
```
### sed
stream editor
Used to search and transform text
Can be used to delete specific lines
## The root account
user id 0
root operates in kernel space and has unlimited access to all parts of the system
root is often not active these days
### Switching user
the `su` command is used to switch to another user
`su -` opens a shell with the correct environment variables
### sudo
super user do
Allows you to temporarily invoke the powers of root
It's best to use `visudo` to manage the files
You can manage who can sudo in:
- `/etc/sudoers`
- `/etc/sudoers.d`
You can manage who has sudo rights with groups
`wheel` has full sudo rights
```bash
## Allows people in group wheel to run all commands
%wheel	ALL=(ALL)	ALL
```
This can be accomplished with the following command
```bash
sudo usermod -aG wheel username
```
You can allow users to run specific commands as admin with the following edit to the sudoers file
```bash
lisa ALL=/sbin/useradd, /usr/bin/passwd
```
You can even make it more specific
```bash
%users ALL=/bin/mount /dev/sdb, /bin/umount /dev/sdb
```
### ssh
`systemctl status sshd` to confirm if ssh is running
by default, root is not allowed to use ssh
#### scp
Uses ssh to copy files to/from a remote server
Example: copy /etc/hosts to /tmp/hosts on a remote server
```bash
scp /etc/hosts user@192.168.100.10:/tmp/hosts
```
## Managing users and groups
### User properties
These are managed in `/etc/passwd` and `/etc/shadow`

| Property         | Description                                                                                       |
| ---------------- | ------------------------------------------------------------------------------------------------- |
| `Name`           | Name of the account                                                                               |
| `Password`       | Secret used for authentication<br>`\etc\passwd` shows an `x` as it is in `\etc\shadow` these days |
| `UID`            | A unique ID for the user                                                                          |
| `GID`            | the ID of the primary group                                                                       |
| `GECOS`          | Additional non-mandatory info about user                                                          |
| `Home Directory` | the environment where users create personal files                                                 |
| `Shell`          | program that will be started after successful authentication                                      |
it looks something like this
```bash
bradley:x:1000:1000:bradley:/home/bradley:/bin/bash
```
System users (service accounts / UID less than 1000) should have `/sbin/nologin` as their shell
You can view the password hash for users in `\etc\shadow`.
The `!` means there is no password set
```bash
student@rhcsa1:~$ sudo tail /etc/shadow
gdm:!:20528::::::
gnome-initial-setup:!:20528::::::
sshd:!:20528::::::
chrony:!:20528::::::
dnsmasq:!:20528::::::
tcpdump:!:20528::::::
student:$y$j9T$2.quEwHS436X.5aaZ7VQw/$fC2TIa7Z2v5XItj/laEzosHjqBvfhRo27N.cVW8LeS4:20534:0:99999:7:::
linda:$y$j9T$JoCmVyfGWy/VUTpKW4vLO1$Lica/elWnPZgfuTdnwWSkc8HuNHPCT.SkT18MeACjm2:20534:0:99999:7:::
jack:!:20534:0:99999:7:::
bob:!:20534:0:99999:7:::
```
### Create and Manage Users

| Command   | Description           |
| --------- | --------------------- |
| `useradd` | create users          |
| `usermod` | modify users          |
| `userdel` | delete users          |
| `passwd`  | change user passwords |
### Default user settings

| Locations          | Description                                                                                                          |
| ------------------ | -------------------------------------------------------------------------------------------------------------------- |
| `/etc/logins.defs` | File to manage user defaults<br>- Password options really matter here<br>- Applies to new users (not existing users) |
| `/etc/skel`        | Files copied from here to user home upon creation                                                                    |
### Limiting user access

| Command                          | Description                                                                                               |
| -------------------------------- | --------------------------------------------------------------------------------------------------------- |
| `usermod -L username`            | Will lock a user<br>(If you check the password hash of a user, and it begins with `!`, then it is locked) |
| `usermod -U username`            | Will unlock a user                                                                                        |
| `usermod -e 2025-09-01 username` | user account will expire on September 1, 2025                                                             |
| `usermod -s /sbin/nologin myapp` | set the login shell as nologin for users that are not intended to login (application accounts)            |
### Managing Group Membership
`Primary group` membership is managed via `/etc/passwd`
```bash
student@rhcsa1:~$ tail --lines=5 /etc/passwd
tcpdump:x:72:72:tcpdump:/:/usr/sbin/nologin
student:x:1000:1000:student:/home/student:/bin/bash
linda:x:1001:1001::/home/linda:/bin/bash
jack:x:1002:1002::/home/jack:/bin/bash
bob:x:1003:1003::/home/bob:/bin/bash
```
The user's primary group will become the `group owner` if a user creates a file.
```bash
tudent@rhcsa1:~/labs$ touch test.txt
student@rhcsa1:~/labs$ ls -la
total 4
drwxr-xr-x.  2 student student   22 Mar 28 16:01 .
drwx------. 15 student student 4096 Mar 28 16:01 ..
-rw-r--r--.  1 student student    0 Mar 28 16:01 test.txt
```
Additional (`secondary`) groups can be given to a user. This is managed via `/etc/group`
```bash
student@rhcsa1:~/labs$ cat /etc/group
root:x:0:
...
wheel:x:10:student
student:x:1000:
linda:x:1001:
jack:x:1002:
bob:x:1003:
```
You can `temporarily` set primary group membership with `newgrp`
`id` can be used to show group membership
```bash
student@rhcsa1:~$ id
uid=1000(student) gid=1000(student) groups=1000(student),10(wheel) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```
### Creating and Managing Groups

| Command                  | Description           |
| ------------------------ | --------------------- |
| `groupadd`               | add to groups         |
| `groupdel`               | delete a group        |
| `groupmod`               | modify a group        |
| `usermod -aG group user` | Add a user to a group |
### Managing Password Properties
Password hashes are stored in `/etc/shadow`
- This is composed of `hashing algorithm` - `random salt` - `encrypted hash of user password`
Basic password requirements are set in `/etc/login.defs`

To change password settings for existing users, use `chage` or `passwd`
```bash
Usage: chage [options] LOGIN

Options:
  -d, --lastday LAST_DAY        set date of last password change to LAST_DAY
  -E, --expiredate EXPIRE_DATE  set account expiration date to EXPIRE_DATE
  -h, --help                    display this help message and exit
  -i, --iso8601                 use YYYY-MM-DD when printing dates
  -I, --inactive INACTIVE       set password inactive after expiration
                                to INACTIVE
  -l, --list                    show account aging information
  -m, --mindays MIN_DAYS        set minimum number of days before password
                                change to MIN_DAYS
  -M, --maxdays MAX_DAYS        set maximum number of days before password
                                change to MAX_DAYS
  -R, --root CHROOT_DIR         directory to chroot into
  -P, --prefix PREFIX_DIR       directory prefix
  -W, --warndays WARN_DAYS      set expiration warning days to WARN_DAYS
```
## Securing Files with Permissions
Every files has a set of user, group, and other users permissions
Using `ls -la` will show the permissions on files in the current directory.
```bash
root@rhcsa1:/etc/skel# ls -la newfile 
-rw-r--r--. 1 root root 0 Mar 28 16:18 newfile
```
### Changing File Ownership
The command `chown user[:group] file` can be used to change ownership of a file.
The `:group` setting is optional.
```bash
root@rhcsa1:/home/anna# ls -la newfile 
-rw-r--r--. 1 anna anna 0 Mar 28 16:18 newfile


root@rhcsa1:/home/anna# chown jack newfile 


root@rhcsa1:/home/anna# ls -la
-rw-r--r--. 1 jack anna   0 Mar 28 16:18 newfile
```
The command `chgrp {group} file` can be used to set group owner membership.
```bash
root@rhcsa1:/home/anna# ls -la newfile 
-rw-r--r--. 1 jack anna 0 Mar 28 16:18 newfile
 
root@rhcsa1:/home/anna# chgrp jack newfile 

root@rhcsa1:/home/anna# ls -la newfile
-rw-r--r--. 1 jack jack   0 Mar 28 16:18 newfile
```
### Understanding Basic Permissions

| Permission | Number | File                    | Directory                      |
| ---------- | ------ | ----------------------- | ------------------------------ |
| `Read`     | `4`    | Open a file             | list files                     |
| `Write`    | `2`    | Modify an existing file | adding files<br>deleting files |
| `Execute`  | `1`    | Run a file              | `cd` into a directory          |
### Managing Basic Permissions
`chmod` is used to manage permissions
This can be used with absolute (`chmod 750 file`) or relative (`chmod +x file`)
`chmod -R` can be used to change recursively
```bash
root@rhcsa1:/home/anna# ls -la taco/
total 0
drwxr-xr-x. 12 root root 187 Mar 29 17:50 .
drwx------.  4 anna anna 105 Mar 29 17:50 ..
drwxr-xr-x.  2 root root   6 Mar 29 17:50 seasoning1
drwxr-xr-x.  2 root root   6 Mar 29 17:50 seasoning2
drwxr-xr-x.  2 root root   6 Mar 29 17:50 seasoning3

root@rhcsa1:/home/anna# chmod -Rv 777 taco/*
mode of 'taco/seasoning1' changed from 0755 (rwxr-xr-x) to 0777 (rwxrwxrwx)
mode of 'taco/seasoning2' changed from 0755 (rwxr-xr-x) to 0777 (rwxrwxrwx)
mode of 'taco/seasoning3' changed from 0755 (rwxr-xr-x) to 0777 (rwxrwxrwx)
```
### Securing Files with Permissions
Default permissions are managed with `umask`.
It is basically a subtraction from the default permissions.
This is managed globally in `/etc/bashrc`
You can also manage per user in `~/.bashrc`

| Permissions     | Files | Directories |
| :-------------- | :---- | ----------- |
| **`Default`**   | `666` | `777`       |
| **`umask 022`** | `644` | `755`       |
| **`umask 027`** | `640` | `750`       |
### Configuring Directories for Shared Group Access
If members of the same group need to share files within a directory, special permissions are required.
The `SGID` (set group ID) permissions ensure that all files created in the shared group directory are group owned by the group owner of the directory.
The `sticky bit` permissions ensures that only the **owner of a file** or **directory that contains the file**, is able to delete the file.
The `SUID` (Set User ID) ensures that a program *is executed* with the permissions of the owner.

| Command                | Description                         |
| ---------------------- | ----------------------------------- |
| `chmod g+s /directory` | Will apply SGID to the directory    |
| `chmod +t /directory`  | Assigns sticky bit to the directory |
You can use the following command to find files that have `SUID` enabled
```bash
find / -perm /4000
```
```bash
/usr/lib/polkit-1/polkit-agent-helper-1
/usr/bin/umount
/usr/bin/chage
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/bin/passwd
/usr/bin/mount....
```
Using `ls -la` will show the `s` flag where the `x` would normally be for the user permission.
```bash
root@rhcsa1:/home/anna# ls -la /usr/bin/passwd 
-rwsr-xr-x. 1 root root 91416 May 25  2025 /usr/bin/passwd
```
Likewise, you can see that a directory has `SGID` set as it will also have an `s` where you would normally see an `x`
```bash
# Before setting SGID
root@rhcsa1:/rhcsa# ls -la
drwxr-xr-x.  2 root students   6 Mar 29 20:40 data
--------------------------------------------------------
# After setting SGID
root@rhcsa1:/rhcsa# chmod g+s data/
root@rhcsa1:/rhcsa# ls -la
drwxr-sr-x.  2 root students   6 Mar 29 20:40 data
```
## Managing Network Configuration
### Understanding NIC naming

| Command        | Description                |
| -------------- | -------------------------- |
| `ip link show` | See current devices        |
| `ip addr show` | Check device configuration |

Device names have *classically* followed the scheme of:
- `eth0`, `eth1`, etc...

Modern BIOS naming is based on hardware properties: 
- `em[-N]` and `eno[nn` for embedded NICs
- `p<slot>p<port>` for NICs on the PCI bus
### Hostnames
The following command is used to manage hostnames
```bash
hostnamectl hostname
```
```bash
student@rhcsa1:~$ hostnamectl
 Static hostname: rhcsa1.example.com
       Icon name: computer-vm
         Chassis: vm 🖴
      Machine ID: 494503dc20a34c06b5a0f373b6bb6b5d
         Boot ID: 98a03a5577304b8baf66e18aa2ac0df6
  Virtualization: kvm
Operating System: Red Hat Enterprise Linux 10.1 (Coughlan)
     CPE OS Name: cpe:/o:redhat:enterprise_linux:10.1
          Kernel: Linux 6.12.0-124.8.1.el10_1.x86_64
    Architecture: x86-64
 Hardware Vendor: QEMU
  Hardware Model: Standard PC _Q35 + ICH9, 2009_
Firmware Version: Arch Linux 1.17.0-2-2
   Firmware Date: Tue 2014-04-01
    Firmware Age: 11y 11month 4w 1d   
```
The hostname is written to `/etc/hostname`
```bash
student@rhcsa1:~$ cat /etc/hostname 
rhcsa1.example.com
```
You can resolve names locally via `/etc/hosts`
```bash
student@rhcsa1:~$ cat /etc/hosts 
# Loopback entries; do not change.
# For historical reasons, localhost precedes localhost.localdomain:
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
# See hosts(5) for proper format and other examples:
# 192.168.1.10 foo.example.org foo
# 192.168.1.13 bar.example.org bar
```
### DNS
You can configure the DNS server via `/etc/resolv.conf`
```bash
student@rhcsa1:~$ cat /etc/resolv.conf 
# Generated by NetworkManager
search example.com
nameserver 192.168.122.1
```
You can technically configure the order of name resolution via `/etc/nsswitch.conf`
### Analyzing Network Config

| Commands   | Description                          |
| ---------- | ------------------------------------ |
| `ip -c a`  | Prints useful networking information |
| `ip route` | Prints useful routing information    |
Example of `ip -c a`
```bash
student@rhcsa1:~$ ip -c a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:6b:42:a7 brd ff:ff:ff:ff:ff:ff
    altname enx5254006b42a7
    inet 192.168.122.185/24 brd 192.168.122.255 scope global dynamic noprefixroute enp1s0
       valid_lft 3562sec preferred_lft 3562sec
    inet6 fe80::5054:ff:fe6b:42a7/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```
Example of `ip route`
```bash
student@rhcsa1:~$ ip route
default via 192.168.122.1 dev enp1s0 proto dhcp src 192.168.122.185 metric 100 
192.168.122.0/24 dev enp1s0 proto kernel scope link src 192.168.122.185 metric 100 
```
### Understanding Network Manager
This is a `systemd` service that manages network configuration.
The configuration files are stored in `/etc/NetworkManager/system-connections`. (You probably don't need to look at these files)
There are a couple useful tools to interact with **NetworkManager**.

| Tool    | Description          |
| ------- | -------------------- |
| `nmcli` | command line utility |
| `nmtui` | GUI utility          |

Permission to modify settings with **NetworkManager** are applied via **dbus**.
Non-privileged users that are logged in on the console **can** change network settings.
Non-privileged users that are logged in on via ssh **can not** change network settings.
```bash
student@rhcsa1:/etc/NetworkManager/system-connections$ nmcli general permissions 
PERMISSION                                                        VALUE 
org.freedesktop.NetworkManager.checkpoint-rollback                auth  
org.freedesktop.NetworkManager.enable-disable-connectivity-check  yes   
org.freedesktop.NetworkManager.enable-disable-network             yes   
org.freedesktop.NetworkManager.enable-disable-statistics          yes   
...
```
#### nmcli
It's a little complicated and thus should probably not be used on the exam, but it is still a useful tool for real life scenarios.
```bash
tudent@rhcsa1:/etc/NetworkManager/system-connections$ nmcli con show
NAME    UUID                                  TYPE      DEVICE 
enp1s0  4b9583ae-b33f-3c43-8d3e-660605728af6  ethernet  enp1s0 
lo      985afa6d-42dc-4acd-9fb6-dc1f3a1f2f47  loopback  lo
```
```     
student@rhcsa1:/etc/NetworkManager/system-connections$ nmcli dev status 
DEVICE  TYPE      STATE                   CONNECTION 
enp1s0  ethernet  connected               enp1s0     
lo      loopback  connected (externally)  lo         

```
#### nmtui
A much easier interface for managing networking and is fairly straightforward.
You can launch by running `nmtui` on the terminal.
### Troubleshooting Networking

| Command       | Description                                                                  |
| ------------- | ---------------------------------------------------------------------------- |
| `ping`        | Sends packets to see if you can communicate with another hostname/ip address |
| `ping6`       | The same as ping, but for ipv6                                               |
| `ip route`    | Prints the routing table                                                     |
| `ip -6 route` | The same as above, but for ipv6                                              |
| `tracepath`   | Show the path to the target                                                  |
| `tracepath6`  | The same as above, but for ipv6                                              |
| `ss`          | analyze socket stats                                                         |
# Daily Administration Tasks
## Managing Software
In the early days, software was packaged as a `tarball`. The process to install was extracting the tarball and following the instructions.
The `Red Hat Package Manager` (RPM) introduced packages on Red Hat.
- These Packages contained metadata, with information about the dependencies.
- A database was also included to keep track of the installed packages - making updates much easier.
- Unfortunately, you still needed to resolve dependencies manually. 

The `Yellowdog Update Manager` (YUM) was later introduced and allowed packages to be installed from repositories - as well as *automagically* handling dependencies!

The `Dandified Yum` (DNF) was a internal improvement of YUM and used the same approach.

The new kid on the flock is `Flatpak`. It provides an isolated container for an application to run in. This is largely for desktop application use.
### RPM Packages
An RPM contains a compressed archive as well as package metadata. 
- The old method of installing these was with `rpm`, but it makes more sense to use `dnf` these days.
- The `rpm` command can be used to show installed packages as well as related data.
```bash
student@rhcsaserver:~$ rpm -qa
libgcc-14.3.1-2.1.el10.x86_64
fonts-filesystem-2.0.5-18.el10.noarch
google-noto-fonts-common-20240401-5.el10.noarch
google-noto-sans-vf-fonts-20240401-5.el10.noarch
redhat-text-vf-fonts-4.1.0-1.el10.noarch
...
```
You can use `rpm2cpio *.rpm` to extract/show contents of a package.
### Setting up Repository Access

> [!WARNING] 
> You may not have repo access by default on the exam and may need to set it up.

A repository is a collection of `RPM` package files with an index that contains the repo contents.
These are often offered through websites, but it is also possible to create local repos.

Normally, a RHEL system should be registered with `subscription-manager` to access cloud repositories managed by Red Hat.

To ensure packages have not been tampered with, `GPG` keys can be used. 
A repository GPG key is used to sign all packages before installing.
To make sure this is possible, you need to ensure the local GPG key is present.
For the exam, you can disable it by setting `gpgcheck=0` in the repo client file.

To access repos offered via subscription manager, use `dnf config-manager --enable name-of-repo`.
Third party repos can be added using a repo file in `/etc/yum.repos.d`... or using `dnf config-manager`
```bash
student@rhcsaserver:~$ cd /etc/yum
yum/         yum.repos.d/ 

student@rhcsaserver:~$ cd /etc/yum.repos.d/

student@rhcsaserver:/etc/yum.repos.d$ ls
redhat.repo

student@rhcsaserver:/etc/yum.repos.d$ cat redhat.repo 
#
# Certificate-Based Repositories
# Managed by (rhsm) subscription-manager
#
# *** This file is auto-generated.  Changes made here will be overwritten. ***
# *** Use "subscription-manager repo-override --help" if you wish to make changes. ***
#
# If this file is empty and this system is subscribed, consider
# running "dnf repolist" to refresh the available repositories.
#
```
You can also run the command `dnf config-manager --add-repo="file:///repo/AppStream"`
### Managing packages with dnf
`dnf` is fairly intuitive

| Command                 | Description                            |
| ----------------------- | -------------------------------------- |
| `dnf list appname`      | lists installed and available packages |
| `dnf search appname`    | searches for an app by name            |
| `dnf search all string` | searches description as well           |
| `dnf info package`      | info about a package                   |
### Using dnf Groups
A dnf group is a collection of packages

| Item                                | Description                                                                                                 |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| Regular group                       | Just a collection of packages                                                                               |
| Environment group                   | Used to install a specific usage pattern                                                                    |
| `dnf group list`                    | Command to print out a list of groups                                                                       |
| `dnf group list hidden`             | Command to show some extras                                                                                 |
| `dnf group info "<groupname>"`      | Command to see packages within a group<br>(They are marked as **mandatory**, **default**, and **optional**) |
| `dnf group install`                 | Command to install mandatory and default packages                                                           |
| `dnf group install --with-optional` | Self explanatory                                                                                            |
|                                     |                                                                                                             |
### Managing dnf Updates and History
All transactions the dnf performs are logged to `/var/log/dnf.rpm.log`.
The command `dnf history` can be used for a summary of all installation and removal transactions.
The command `dnf history undo n` can undo a specific transaction.
### Using subscription-manager
In order to properly use Red Hat Enterprise Linux, you'll need to register the system and attach a subscription. Before doing so, you'll have no repository access. If you can't register RHEL, the only option is to use self provided repositories.

To register, use `subscription-manager register`. After which, you'll be prompted to enter a username for which the subscription is connected to. To un-register, use `subscription-manager unregister`.

After registering, **entitlement certificates** are created.
- `/etc/pki/product` indicates the installed RHEL products.
- `/etc/pki/consumer` identifies the Red Hat account for registration.
Use the `rct` command to check current entitlements.
### Managing Software with Flatpak
You can install flatpak with the command `dnf install -y flatpak`.
After install, `flatpak --version` can be used to show the current install.
To get access to the default REHL flatpak **remote**, you must authenticate using `podman login registry.redhat.io`.
Another **remote** is flathub which is located at https://flathub.org, which does not require authentication to use.
The list of remotes is located in `/etc/flatpak/remotes.d`.
You an add more remotes with a command such as:
```bash
flatpak remote-add --if-not-exists fedora oci+https://registry/fedoraproject.org
```


| Command                             | Description                                                                  |
| ----------------------------------- | ---------------------------------------------------------------------------- |
| `flatpak search <string>`           | Search for a flatpak to find a matching app                                  |
| `flatpak install <app>`             | Install a specific app. If multiple match, you'll be prompted to choose one. |
| `flatpak install -u <app>`          | Install the app, but only for the current user.                              |
| `flatpak list`                      | Show apps currently installed.                                               |
| `flatpak info`                      | Get flatpak details                                                          |
| `flatpak update`                    | Update an app                                                                |
| `flatpak mask`                      | Keep an app at a specific version by preventing updates.                     |
| `flatpak mask --remove`             | Undo the above command.                                                      |
| `flatpak uninstall [--delete-data]` | Remove an app                                                                |
## Monitoring Activity
