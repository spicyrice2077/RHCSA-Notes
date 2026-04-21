![image RHEL](https://github.com/spicyrice2077/RHCSA-Notes/blob/main/images/redhat.png)
# Understanding Red Hat Enterprise Linux
### Basics
The big thing about RHEL is support. The Red Hat company can provide support for paying companies.
This support is obtained via support licenses.
RHEL started in 1993 and is the most successful enterprise linux version available.
A new version of RHEL comes out every 3 years or so.
You can obtain support for up to 10 years
You can have free access to RHEL (unsupported) for up to 16 instances of RHEL (using a Developer account)
### The RHEL family
`Fedora` - latest and greatest, but less stable.

`CentOS` Stream - a little more stable than fedora, but more cutting edge than RHEL.

`RHEL` is derived from CentOS.

`Oracle` and `Amazon linux` are derived from RHEL.
### Custom Partitioning
Linux servers need 3 different areas for storing data
-  small partition for linux kernel
-  UEFI bootlogger files
- `/` - OS essential files
# Using the Command Line
`bash` is often the default shell.

`terminal` is an environment which runs the shell.

`who` command can be used to display who is logged in.

`cockpit` is a useful interface, but you should not rely too heavily on it.
# Essential tools
### man
best source to get extensive info on your system (especially if you don't have internet access).
These are formatted into sections.
- section 1 - executable programs/commands
- section 5 - file formats and conventions
- section 8 - system admin commands (sudo)
You can use `/text_here` to search for a string in the man page.
You can also use uppercase `G` to show examples.
`man` is indexed from `mandb`.
You can populate mandb using `sudo mandb`.
### editors
Linux configuration happens by modifying text files.
- plain ASCII is used often
- YAML is often used for Ansible
- JSON is often good for data storage formats
- XML is an alternative to both of the above.
`nano` and `vim` are very common text editors.
`nano` is very easy to use.
`vim` is more complex and very powerful.
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
Bash registers command history.

The `command` history will print out recently used commands.

The `HISTSIZE` and `HISTFILESIZE` are variables to define how long it should be (1000 is default).

`history -w` syncs history memory to the history file.
### Shell Expansion
Globbing expands filename based on wildcards.
- `ls *`
- `ls a?*`
- `ls [a-e]*`

Other types of expansion also exist.
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
These are items to be referenced and re-used.
The command `env` will show the current variables.
Users can define new variables using `[export] key=value` (mostly useful for scripting).
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
you can remove an alias with `unalias`.
custom aliases are stored in bash startup files.
### Tuning the bash environment
The following is to make variables and alias persistent.

`/etc/profile` is the generic bash file that are processed in a login shell.

`/etc/bashrc` is processed while opening a subshell.

`~/.bash_profile` is the *user specific* way to customize.

`~/.bashrc` same thing.

`~/.bash_logout` commands you want to execute when you log out.
# Basic System Admin tasks
## File Management
### Directory Structure
The directory hierarchy is highly standardized.
The command `man hier` will print out descriptions.

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
The Linux file-system often uses multiple mounts.

To manually mount a device, you can use the following command.
```bash
mount /dev/devicename /directory
```
Example: 
```bash
mount /dev/sdb1 /mnt
```
The `mount` command gives an overview of all mounts.
It's a little too much info for most occasions.
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
The `lsblk` command gives a clean overview of real devices and their current mount in the directory structure.
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
This will show the inode number in a list format.
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
A name that is pointing directly to a inode.
You can have several that point to the same inode.
These must be on the same device.
The target **cannot** be a directory.
Example of use: 6 links pointing to the same script that needs to be changed often.
#### symbolic links
These point to a name.
Also called a soft link.
If the target is removed, then this will point to nothing.
The target **can** be a directory.
The best practice is to point to an **absolute path** instead of a **relative path**.
`bin`, `lib`, `lib64` are symlinks in the example below.
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
`tar` is the **Tape Archiver** and was created a long time ago.
It doesn't compress data by default.
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
`gzip` (`-z` argument for tar) is the most common compression utility.
## Managing text Files
### Common text tools
#### more
the original file pager
#### less
was developed to offer some more advanced features.
you can use the arrows to go up and down.
#### head
show first 10 lines.
use `-n 20` to shows 20.
#### tail 
shows last 10 lines.
use `-n 20` to shows 20.
use `-f` to follow (useful to follow new additions to a file in real time).
```bash
tail -f /var/log/messages
```
#### cat
dumps text file to screen.
#### cut
filters output.
Example.
```bash
# Print the fifth character on each line:
cut [-c|--characters] 5
```
#### sort
sorts output.
Example.
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
tool to find text in output and files.
```bash
ps aux | grep firefox
```
Search for a pattern within files:  
```bash
grep "search_pattern" path/to/file1 path/to/file2 ...
```
### Regular Expressions (regex)
Always put regex between single quotes.
Don't confuse these with globbing (shell wildcards).
For use with some of the following tools:
- grep
- vim
- awk
- sed
For more info.
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
powerful text processing utility that specialized in data extraction and reporting.
Example: Print the fifth column (a.k.a. field) in a space-separated file:
```bash
awk '{print $5}' path/to/file
```
### sed
stream editor.
Used to search and transform text.
Can be used to delete specific lines.
## The root account
user id 0.
root operates in kernel space and has unlimited access to all parts of the system.
root is often not active these days.
### Switching user
the `su` command is used to switch to another user.
`su -` opens a shell with the correct environment variables.
### sudo
super user do
Allows you to temporarily invoke the powers of root.
It's best to use `visudo` to manage the files.
You can manage who can sudo in:
- `/etc/sudoers`
- `/etc/sudoers.d`
You can manage who has sudo rights with groups.
`wheel` has full sudo rights
```bash
## Allows people in group wheel to run all commands
%wheel	ALL=(ALL)	ALL
```
This can be accomplished with the following command.
```bash
sudo usermod -aG wheel username
```
You can allow users to run specific commands as admin with the following edit to the sudoers file.
```bash
lisa ALL=/sbin/useradd, /usr/bin/passwd
```
You can even make it more specific.
```bash
%users ALL=/bin/mount /dev/sdb, /bin/umount /dev/sdb
```
### ssh
`systemctl status sshd` to confirm if ssh is running.
by default, root is not allowed to use ssh.
#### scp
Uses ssh to copy files to/from a remote server.
Example: copy /etc/hosts to /tmp/hosts on a remote server.
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
System users (service accounts / UID less than 1000) should have `/sbin/nologin` as their shell.
You can view the password hash for users in `\etc\shadow`.
The `!` means there is no password set.
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
You can `temporarily` set primary group membership with `newgrp`.
`id` can be used to show group membership.
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
Password hashes are stored in `/etc/shadow`.
- This is composed of `hashing algorithm` - `random salt` - `encrypted hash of user password`.
Basic password requirements are set in `/etc/login.defs`.

To change password settings for existing users, use `chage` or `passwd`.
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
Every files has a set of user, group, and other users permissions.
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
`chmod` is used to manage permissions.
This can be used with absolute (`chmod 750 file`) or relative (`chmod +x file`).
`chmod -R` can be used to change recursively.
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
This is managed globally in `/etc/bashrc`.
You can also manage per user in `~/.bashrc`.

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
You can use the following command to find files that have `SUID` enabled.
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
A dnf group is a collection of packages.

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
### Exploring Jobs and Processes
All tasks are started as processes.
Processes have a `PID` for which they can be managed.
Some processes are starting multiple threads. Individual threads cannot be managed.
Threads started from a shell can be managed as jobs.
Shell jobs can be started in the foreground or background.
### Managing Shell Jobs
You run a command as a job in the background by doing the following
```bash
<command> &
```
You can also move a currently running job to the background with
```bash
Crtl + Z
```
and then
```bash
bg
```
### Understanding Process States

| Main Process States | Description                                                                                                                                            |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `Running (R)`       | A process in the running state is either:<br><br>- Currently executing on the CPU<br>- Ready to run and waiting for CPU time                           |
| `Sleeping (S)`      | A sleeping process is waiting for an event to complete, such as:<br><br>- Input/Output operation<br>- Signal receipt<br>- Resource availability        |
| `Stopped (T)`       | A stopped process has been paused by:<br><br>- A user signal (SIGSTOP)<br>- Debugging operations                                                       |
| `Zombie (Z)`        | A zombie process is:<br><br>- A terminated process<br>- Still has an entry in the process table<br>- Waiting for its parent to collect its exit status |
| `Dead (X)`          | A dead process is:<br><br>- Completely terminated<br>- Being removed from the process table                                                            |
#### Common Process State Transitions
`Created → Running`
- Process is loaded into memory
- Scheduled for execution

`Running → Sleeping`
- Process waits for resources
- Voluntarily yields CPU

`Sleeping → Running`
- Required resources become available
- Process is scheduled again

`Running → Stopped`
- Receives SIGSTOP signal
- User initiates debugging

`Running → Zombie`
- Process terminates
- Parent hasn’t collected exit status
### Process Information with `ps`
One of the most useful ps commands to get an overview of all running processes is:
```bash
# Anything in [] is a kernel thread.
student@rhcsaserver:~$ ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  4.8  0.7  50512 41996 ?        Ss   18:00   0:00 /usr/lib/systemd/systemd ...
root           2  0.0  0.0      0     0 ?        S    18:00   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        S    18:00   0:00 [pool_workqueue_release]
root           4  0.0  0.0      0     0 ?        I<   18:00   0:00 [kworker/R-rcu_gp]
root           5  0.0  0.0      0     0 ?        I<   18:00   0:00 [kworker/R-sync_wq]
root           6  0.0  0.0      0     0 ?        I<   18:00   0:00 [kworker/R-slub_flushwq]
root           7  0.0  0.0      0     0 ?        I<   18:00   0:00 [kworker/R-netns]
...
student     3344 11.2  3.7 3005300 214484 ?      Ssl  18:01   0:00 /usr/bin/ptyxis --gapplication-service
student     3351  0.0  0.1 377772  6888 ?        Ssl  18:01   0:00 /usr/libexec/ptyxis-agent --socket..
student     3369  0.0  0.0    976   588 ?        S    18:01   0:00 catatonit -P
student     3421  0.0  0.0 230168  5636 pts/0    Ss   18:01   0:00 /usr/bin/bash
student     3466  0.0  0.0 230704  4092 pts/0    R+   18:01   0:00 ps aux
```
You can show the process forest with some different ps options.
```bash
student@rhcsaserver:~$ ps -fax
    PID TTY      STAT   TIME COMMAND
      2 ?        S      0:00 [kthreadd]
      3 ?        S      0:00  \_ [pool_workqueue_release]
      4 ?        I<     0:00  \_ [kworker/R-rcu_gp]
      5 ?        I<     0:00  \_ [kworker/R-sync_wq]
      6 ?        I<     0:00  \_ [kworker/R-slub_flushwq]
      7 ?        I<     0:00  \_ [kworker/R-netns]
...
   3351 ?        Ssl    0:00  |   \_ /usr/libexec/ptyxis-agent --socket-fd=3 --rlimit-nofile=1024
   3421 pts/0    Ss     0:00  |       \_ /usr/bin/bash
   3657 pts/0    R+     0:00  |           \_ ps -fax
   3369 ?        S      0:00  \_ catatonit -P
...
```
### Monitoring Memory Usage
Linux places as many files as possible into cache, in order to guarantee fast access to the files.
Because of this, it appears that the memory is saturated (low memory shows available for use).
Swap is used as an overflow buffer of emulated RAM on disk.
The kernel will move inactive application memory to swap first.
Inactive cache memory will just be dropped.
The `free` command is useful to get an idea of available memory.
```bash
student@rhcsaserver:~$ free -h
               total        used        free      shared  buff/cache   available
Mem:           5.5Gi       1.4Gi       3.5Gi        34Mi       851Mi       4.0Gi
Swap:          4.0Gi          0B       4.0Gi
```
The following file also contains useful info about memory
```bash
student@rhcsaserver:~$ cat /proc/meminfo 
MemTotal:        5733216 kB
MemFree:         3640736 kB
MemAvailable:    4244440 kB
Buffers:            4300 kB
Cached:           834652 kB
SwapCached:            0 kB
Active:          1571968 kB
...
```
### System Activity with `top`
This is a useful dashboard to monitor system activity
```bash
top - 18:19:46 up 18 min,  2 users,  load average: 0.32, 0.07, 0.02
Tasks: 275 total,   1 running, 274 sleeping,   0 stopped,   0 zombie
%Cpu(s):  4.5 us,  1.7 sy,  0.0 ni, 93.9 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st 
MiB Mem :   5598.8 total,   3541.7 free,   1467.1 used,    852.3 buff/cache     
MiB Swap:   4096.0 total,   4096.0 free,      0.0 used.   4131.7 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                                                
   2573 student   20   0 4919460 366160 130116 S  10.3   6.4   0:14.27 gnome-shell                                            
   3344 student   20   0 3061608 381620 102784 S   8.3   6.7   0:15.29 ptyxis                                                 
   2336 root      20   0  157720   4792   4268 S   1.0   0.1   0:00.85 spice-vdagentd                                         
   2659 student   20   0  606732  11932   6684 S   0.3   0.2   0:00.24 ibus-daemon                                            
      1 root      20   0   50512  41996  10492 S   0.0   0.7   0:00.96 systemd                                                
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kthreadd                                               
      3 root      20   0       0      0      0 S   0.0   0.0   0:00.00 pool_workqueue_release                                 
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-rcu_gp                                       
      5 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-sync_wq                                      
```
## Managing Processes
### Terminating processes
Use `kill <PID>` to send signals such as `SIGTERM` or `SIGKILL`. 
`killall <process_name>` terminates all instances of a process.
### Pausing and resuming
`Ctrl+Z` suspends a foreground process. 
Use `fg` to resume in the foreground or `bg` to continue in the background.
### Renicing and prioritizing
`nice` launches a process with a specified CPU priority.
`renice` adjusts the priority of running processes to balance CPU allocation.
You can use `top` and then `r` to renice a PID to a new value.
Nice Values range from `-20` to `19`. The lower the number, the higher the priority.
### Manage Tuned Profiles
You can use the command `tuned` to easily manage system tuning.
```bash
student@rhcsaserver:~$ tuned-adm list 
Available profiles:
- accelerator-performance     - Throughput performance based tuning with disabled higher latency STOP states
- aws                         - Optimize for aws ec2 instances
- balanced                    - General non-specialized tuned profile
- balanced-battery            - Balanced profile biased towards power savings changes for battery
- desktop                     - Optimize for the desktop use-case
- hpc-compute                 - Optimize for HPC compute workloads
- intel-sst                   - Configure for Intel Speed Select Base Frequency
- latency-performance         - Optimize for deterministic performance at the cost of increased power consumption
- network-latency             - Optimize for deterministic performance at the cost of increased power consumption, focused on low latency network performance
- network-throughput          - Optimize for streaming network throughput, generally only necessary on older CPUs or 40G+ networks
- optimize-serial-console     - Optimize for serial console use.
- powersave                   - Optimize for low power consumption
- throughput-performance      - Broadly applicable tuning that provides excellent performance across a variety of common server workloads
- virtual-guest               - Optimize for running inside a virtual guest
- virtual-host                - Optimize for running KVM guests
Current active profile: virtual-guest
```
### Managing User Sessions and Processes
You can use `ps -u <username>` to show process
You can also use `pkill -u <username>` to remove processes owned by a specific user.
```bash
student@rhcsaserver:~$ ps -u student
    PID TTY          TIME CMD
   2442 ?        00:00:00 systemd
   2446 ?        00:00:00 (sd-pam)
   2464 ?        00:00:00 gnome-keyring-d
   2475 tty2     00:00:00 gdm-wayland-ses
   2481 ?        00:00:00 dbus-broker-lau
   2488 ?        00:00:00 dbus-broker
   2493 tty2     00:00:00 gnome-session-b
   2544 ?        00:00:00 gnome-session-c
   2548 ?        00:00:00 gnome-session-b
   2573 ?        00:00:25 gnome-shell
   2583 ?        00:00:00 gvfsd
   2589 ?        00:00:00 gvfsd-fuse
   2618 ?        00:00:00 at-spi-bus-laun
   2624 ?        00:00:00 dbus-broker-lau
   2625 ?        00:00:00 dbus-broker
   2626 ?        00:00:00 at-spi2-registr
```

Another useful tool is `loginctl`.
Some example commands:
```bash
student@rhcsaserver:~$ loginctl 
SESSION  UID USER    SEAT  LEADER CLASS         TTY   IDLE SINCE
      2 1000 student seat0 2422   user          tty2  no   -    
      3 1000 student -     2442   manager       -     no   -    
      4    0 root    -     4880   manager-early -     no   -    
     c2    0 root    -     4864   user-early    pts/1 no   -    

4 sessions listed.

student@rhcsaserver:~$ loginctl list-sessions 
SESSION  UID USER    SEAT  LEADER CLASS         TTY   IDLE SINCE
      2 1000 student seat0 2422   user          tty2  no   -    
      3 1000 student -     2442   manager       -     no   -    
      4    0 root    -     4880   manager-early -     no   -    
     c2    0 root    -     4864   user-early    pts/1 no   -    

4 sessions listed.

student@rhcsaserver:~$ loginctl list-users 
 UID USER    LINGER STATE 
   0 root    no     active
1000 student no     active

2 users listed.
```

```bash
loginctl [OPTIONS...] COMMAND ...

Send control commands to or query the login manager.

Session Commands:
  list-sessions            List sessions
  session-status [ID...]   Show session status
  show-session [ID...]     Show properties of sessions or the manager
  activate [ID]            Activate a session
  lock-session [ID...]     Screen lock one or more sessions
  unlock-session [ID...]   Screen unlock one or more sessions
  lock-sessions            Screen lock all current sessions
  unlock-sessions          Screen unlock all current sessions
  terminate-session ID...  Terminate one or more sessions
  kill-session ID...       Send signal to processes of a session
```
## Working with Systemd
### Understanding the Role of Systemd
It is the first process started after loading the kernel.
It is used for starting services (ssh, web server, etc...).
It also starts other units: timers, sockets, sshd, mounting items, etc...
The primary command used to manage Systemd is `systemctl`.
### Exploring Systemd Units
`Service` units are used to start processes (These are the most important). 

`Socket` units monitor activity on a port and start the corresponding service unit when needed.

`Timer` units are used to start services periodically.

`Path` units can start service units when activity is detected in the file system.

`Mount` units are used to mount file systems.

There are more units, but they are not as relevant for the RHCSA exam.

You can use `cat` in combination with `systemctl` to get info on a unit.
```bash
student@rhcsaserver:~$ systemctl cat sshd.service 
# /usr/lib/systemd/system/sshd.service
[Unit]
Description=OpenSSH server daemon
Documentation=man:sshd(8) man:sshd_config(5)
After=network.target sshd-keygen.target
Wants=sshd-keygen.target
# Migration for Fedora 38 change to remove group ownership for standard host keys
# See https://fedoraproject.org/wiki/Changes/SSHKeySignSuidBit
Wants=ssh-host-keys-migration.service

[Service]
Type=notify
EnvironmentFile=-/etc/sysconfig/sshd
ExecStart=/usr/sbin/sshd -D $OPTIONS
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target

```
You can list all unit files with the following command:
```bash
student@rhcsaserver:~$ systemctl list-unit-files 
UNIT FILE                                                                 STATE           PRESET  
proc-sys-fs-binfmt_misc.automount                                         static          -       
-.mount                                                                   generated       -       
boot.mount                                                                generated       -       
dev-hugepages.mount                                                       static          -       
dev-mqueue.mount                                                          static          -       
proc-sys-fs-binfmt_misc.mount                                             disabled        disabled
run-vmblock\x2dfuse.mount                                                 disabled        disabled
sys-fs-fuse-connections.mount                                             static          -       
...
```
### Managing Systemd Services
After installing **RHEL**, many units are enabled by default.
The following command can be used to check the status of units.
`systemctl status <unit name>`
```bash
student@rhcsaserver:~$ systemctl status sshd
● sshd.service - OpenSSH server daemon
     Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; preset: enabled)
     Active: active (running) since Sat 2026-04-04 14:43:51 CDT; 9min ago
 Invocation: f631687479974716bec5ff037da02d60
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 1269 (sshd)
      Tasks: 1 (limit: 35471)
     Memory: 2.3M (peak: 2.5M)
        CPU: 5ms
     CGroup: /system.slice/sshd.service
             └─1269 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

Apr 04 14:43:51 rhcsaserver.example.com systemd[1]: Starting sshd.service - OpenSSH server daemon...
Apr 04 14:43:51 rhcsaserver.example.com sshd[1269]: Server listening on 0.0.0.0 port 22.
Apr 04 14:43:51 rhcsaserver.example.com systemd[1]: Started sshd.service - OpenSSH server daemon.
Apr 04 14:43:51 rhcsaserver.example.com sshd[1269]: Server listening on :: port 22.

```
Common commands to be used:

| Command                         | Description                                           |
| ------------------------------- | ----------------------------------------------------- |
| `systemctl status <unit name>`  | Get the current status                                |
| `systemctl start <unit name>`   | Start the unit                                        |
| `systemctl enable <unit name>`  | Make sure the unit auto starts at system boot         |
| `systemctl stop <unit name>`    | Stop the unit                                         |
| `systemctl restart <unit name>` | Restart the unit                                      |
| `systemctl disable <unit name>` | Make sure the unit does not auto start at system boot |
### Modifying Systemd Unit Configuration
Default system-provided systemd unit files are in `/usr/lib/systemd/system`.
You should not edit these.
```bash
student@rhcsaserver:/usr/lib/systemd/system$ ls -la
total 1780
drwxr-xr-x. 35 root root 20480 Mar 15 20:27  .
drwxr-xr-x. 18 root root  4096 Mar 15 20:27  ..
-rw-r--r--.  1 root root  1964 Apr 24  2025  accounts-daemon.service
-rw-r--r--.  1 root root   480 Jun 30  2025  alsa-restore.service
-rw-r--r--.  1 root root   465 Jun 30  2025  alsa-state.service
-rw-r--r--.  1 root root   275 Oct 28  2024  arp-ethers.service
-rw-r--r--.  1 root root   274 Jun 29  2025  atd.service
-rw-r--r--.  1 root root  1966 Apr 10  2025  auditd.service
...
```
Custom units are in `/etc/systemd/system`.
```bash
student@rhcsaserver:/etc/systemd/system$ ls -la
total 16
drwxr-xr-x. 14 root root 4096 Mar 15 20:28  .
drwxr-xr-x.  5 root root   47 Mar 15 20:26  ..
drwxr-xr-x.  2 root root   31 Mar 15 20:26  bluetooth.target.wants
lrwxrwxrwx.  1 root root   37 Mar 15 20:26  ctrl-alt-del.target -> /usr/lib/systemd/system/reboot.target
lrwxrwxrwx.  1 root root   41 Mar 15 20:26  dbus-org.bluez.service -> /usr/lib/systemd/system/bluetooth.service
lrwxrwxrwx.  1 root root   41 Mar 15 20:27  dbus-org.fedoraproject.FirewallD1.service -> /usr/lib/systemd/system/firewalld.service
lrwxrwxrwx.  1 root root   44 Mar 15 20:26  dbus-org.freedesktop.Avahi.service -> /usr/lib/systemd/system/avahi-daemon.service
lrwxrwxrwx.  1 root root   44 Mar 15 20:26  dbus-org.freedesktop.ModemManager1.service -> /usr/lib/systemd/system/ModemManager.service
...
```
Run-time automatically generated unit files are in `/run/systemd`.
```bash
student@rhcsaserver:/run/systemd$ ls -la
total 0
drwxr-xr-x. 20 root root  580 Apr  4 14:54 .
drwxr-xr-x. 48 root root 1240 Apr  4 14:43 ..
drwxr-xr-x.  2 root root   40 Apr  4 14:43 ask-password
srw-------.  1 root root    0 Apr  4 14:43 coredump
drwxr-xr-x.  6 root root  260 Apr  4 14:43 generator
dr-xr-xr-x.  3 root root  160 Apr  4 14:43 inaccessible
drwxr-xr-x.  2 root root   40 Apr  4 14:43 incoming
drwxr-xr-x.  2 root root  320 Apr  4 14:44 inhibit
srw-------.  1 root root    0 Apr  4 14:43 io.systemd.BootControl
srw-rw-rw-.  1 root root    0 Apr  4 14:43 io.systemd.Credentials
srw-rw-rw-.  1 root root    0 Apr  4 14:43 io.systemd.Hostname
srw-rw-rw-.  1 root root    0 Apr  4 14:43 io.systemd.ManagedOOM
srw-------.  1 root root    0 Apr  4 14:43 io.systemd.sysext
...
```
For custom configs, use `systemctl edit unit.service` to edit unit files. 
You can use `systemctl show` for available parameters.
```bash
student@rhcsaserver:/run/systemd$ systemctl show sshd.service 
Type=notify
ExitType=main
Restart=on-failure
RestartMode=normal
NotifyAccess=main
RestartUSec=42s
RestartSteps=0
RestartMaxDelayUSec=infinity
...
```
### Managing Unit Dependencies
The following command is a good start to view dependencies.
```bash
student@rhcsaserver:/run/systemd$ systemctl list-dependencies 
default.target
● ├─accounts-daemon.service
● ├─gdm.service
○ ├─nvmefc-boot-connections.service
● ├─rtkit-daemon.service
● ├─switcheroo-control.service
○ ├─systemd-update-utmp-runlevel.service
● ├─tuned-ppd.service
● ├─udisks2.service
● ├─upower.service
● └─multi-user.target
●   ├─atd.service
○   ├─audit-rules.service
●   ├─auditd.service
●   ├─avahi-daemon.service
...
```
If you add the following to a unit config, then you can add dependencies.
```bash
student@rhcsaserver:/run/systemd$ systemctl edit sshd.service 
-----------------------------------------------------------------
### Editing /etc/systemd/system/sshd.service.d/override.conf
### Anything between here and the comment below will become the contents of the drop-in file
[Unit]
Requires=vsftpd.service

### Edits below this comment will be discarded
....
```
What the above does is essentially make any services under "Requires" auto-start when the dependent service starts up.
### Masking Services
Some units cannot work simultaneously on the same system. (Example: `nginx` and `http`)
You can prevent admins from accidentally starting a conflicting unit by using `systemctl mask`. 
This essentially just links a unit to `/dev/null` so that it can't start. The comman `systemctl unmask` will undo that change.
```bash
student@rhcsaserver:/run/systemd$ systemctl mask sshd.service 
Created symlink '/etc/systemd/system/sshd.service' → '/dev/null'.
--------------------------
student@rhcsaserver:/run/systemd$ systemctl start sshd.service
Failed to start sshd.service: Unit sshd.service is masked.
--------------------------
student@rhcsaserver:/run/systemd$ systemctl status sshd.service 
● sshd.service
     Loaded: masked (Reason: Unit sshd.service is masked.)
.....
```
### Using Systemd to Run Anything
You can basically just make a custom service by copying an existing service from `/usr/lib/systemd/system/*.service` to `/etc/systemd/system/*.service` and then edit the contents to run your custom commands.
```bash
student@rhcsaserver:/run/systemd$ sudo cp -v /usr/lib/systemd/system/upower.service /etc/systemd/system/custom.service
'/usr/lib/systemd/system/upower.service' -> '/etc/systemd/system/custom.service'
```
Edit the new file to reflect what you would like it to do.
```bash
GNU nano 8.1                                  /etc/systemd/system/custom.service                                  Modified  
[Unit]
Description=Sleepy boi                 
After=network-online.target

[Service]
Type=simple
ExecStart=/usr/bin/sleep 300  

[Install]
WantedBy=multi-user.target
```
Reload systemd for it to get the new config.
```bash
student@rhcsaserver:/run/systemd$ sudo systemctl daemon-reload 
```
Start the service.
```bash
student@rhcsaserver:/run/systemd$ systemctl status custom.service 
○ custom.service - Sleepy boi
     Loaded: loaded (/etc/systemd/system/custom.service; disabled; preset: disabled)
     Active: inactive (dead)
--------------------------------
student@rhcsaserver:/run/systemd$ systemctl start custom.service 
--------------------------------
student@rhcsaserver:/run/systemd$ systemctl status custom.service 
● custom.service - Sleepy boi
     Loaded: loaded (/etc/systemd/system/custom.service; disabled; preset: disabled)
     Active: active (running) since Sat 2026-04-04 19:22:59 CDT; 1s ago
 Invocation: 6f95e89e480f4b35ab61c107f04f4f5d
   Main PID: 6937 (sleep)
      Tasks: 1 (limit: 35471)
     Memory: 252K (peak: 1.2M)
        CPU: 2ms
     CGroup: /system.slice/custom.service
             └─6937 /usr/bin/sleep 300

Apr 04 19:22:59 rhcsaserver.example.com systemd[1]: Started custom.service - Sleepy boi.
```
## Scheduling Tasks
### Systemd Timer
Since **RHEL 9**, this is the default way of scheduling recurring services.
Systemd provides `unit.timer` files which are used along side `unit.service` files to schedule the service.
In the `.timer` file, the `OnCalendar` option specifies when the service should be started. 
Systemd timers are often installed from RPM packages.

You can use **systemctl** with the appropriate arguments to list the available timers.
```bash
student@rhcsaserver:~$ systemctl list-units -t timer 
  UNIT                         LOAD   ACTIVE SUB     DESCRIPTION                                 
  dnf-makecache.timer          loaded active waiting dnf makecache --timer
  fstrim.timer                 loaded active waiting Discard unused filesystem blocks once a week
  fwupd-refresh.timer          loaded active waiting Refresh fwupd metadata regularly
  logrotate.timer              loaded active waiting Daily rotation of log files
  plocate-updatedb.timer       loaded active waiting Update the plocate database daily
  raid-check.timer             loaded active waiting Weekly RAID setup health check
  systemd-tmpfiles-clean.timer loaded active waiting Daily Cleanup of Temporary Directories

Legend: LOAD   → Reflects whether the unit definition was properly loaded.
        ACTIVE → The high-level unit activation state, i.e. generalization of SUB.
        SUB    → The low-level unit activation state, values depend on unit type.

7 loaded units listed. Pass --all to see loaded but inactive units, too.
To show all installed unit files use 'systemctl list-unit-files'.
```
You can also list the particular associated files with the following command.
```bash
student@rhcsaserver:~$ systemctl list-unit-files logrotate*
UNIT FILE         STATE   PRESET 
logrotate.service static  -      
logrotate.timer   enabled enabled

2 unit files listed.
```
We can user `systemctl cat` on a timer to view the details of the timer.
```bash
student@rhcsaserver:~$ systemctl cat logrotate.timer 
# /usr/lib/systemd/system/logrotate.timer
[Unit]
Description=Daily rotation of log files
Documentation=man:logrotate(8) man:logrotate.conf(5)

[Timer]
OnCalendar=daily
RandomizedDelaySec=1h
Persistent=true

[Install]
WantedBy=timers.target
```
The **OnCalendar** option can be validated with the following command.
```bash
student@rhcsaserver:~$ systemd-analyze calendar daily 8:00
  Original form: daily
Normalized form: *-*-* 00:00:00
    Next elapse: Mon 2026-04-06 00:00:00 CDT
       (in UTC): Mon 2026-04-06 05:00:00 UTC
       From now: 4h 11min left

  Original form: 8:00
Normalized form: *-*-* 08:00:00
    Next elapse: Mon 2026-04-06 08:00:00 CDT
       (in UTC): Mon 2026-04-06 13:00:00 UTC
       From now: 12h left
```
If you need to edit an existing timer, you can use the following.
```bash
student@rhcsaserver:~$ sudo systemctl edit raid-check.timer 
-----------------------------
### Editing /etc/systemd/system/raid-check.timer.d/override.conf
### Anything between here and the comment below will become the contents of the drop-in file

### Edits below this comment will be discarded
### /usr/lib/systemd/system/raid-check.timer
# [Unit]
# Description=Weekly RAID setup health check
# 
# [Timer]
# OnCalendar=Sun *-*-* 01:00:00
# Persistent=true
# AccuracySec=24h
# 
# [Install]
# WantedBy=timers.target
```
### cron jobs
The is the OG for scheduling and has been around for a while.

The `crond` process checks its config every minute in the `/etc/crontab` file.

For drop in cron files, they should be placed in `/etc/cron.d/` or one of the following.

For `cron.d`, the files need the correct syntax... for the `hourly`, `daily`, `etc...` you can just put a script as long as it's executable.
```bash
student@rhcsaserver:/etc$ cd cron.
cron.d/       cron.daily/   cron.hourly/  cron.monthly/ cron.weekly/  
```
To create a user cronjob, use `crontab -e` and make sure it has the correct syntax.

Cron time specifications are specified as `minute, hour, day of the month, month, day of the week`.
Example: `0 * * dec 1-5` will run a cron job `every Monday - Friday on minute zero in the Month of December`.
```bash
student@rhcsaserver:/etc$ cat /etc/crontab 
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
```

**anacron** is the service behind **cron** that ensures jobs are executed on a regular basis.

It takes care of all the jobs and the configuration is in `/etc/anacron`, but really shouldn't be edited.
### at
Before `at` can do its thing, the `atd` service must be running.
You can use `at <time>` to schedule a job.
- Type the specifications and then use `Ctrl+D` to close the shell.
```bash
student@rhcsaserver:/etc$ at 9:00
warning: commands will be executed using /bin/sh
at Mon Apr  6 09:00:00 2026
at> touch /home/student/at.txt
at> echo "This is a test" > /home/student/at.txt
at> <EOT>
job 1 at Mon Apr  6 09:00:00 2026
```
You can use `atq` to list jobs and then `atrm` to remove any you don't want.
```bash
student@rhcsaserver:/etc$ atq
1	Mon Apr  6 09:00:00 2026 a student
student@rhcsaserver:/etc$ atrm 1
student@rhcsaserver:/etc$ atq
student@rhcsaserver:/etc$ 
```
### Managing temporary files
Temporary files have **historically** been placed in `/tmp`. Without management, this directory could become quite bloated.

Nowadays, we use `systemd-tmpfiles` to manage temp files and directories.

- This will create/delete temp files automatically according to the files in the following locations.
	- `/usr/lib/tmpfiles.d/`
	- `/etc/tmpfiles.d`
	- `/run/tmpfiles.d`
Using `man tmpfiles.d` is a good idea to refresh your memory on who to manage these.

Example:
If were were to create this file:
```bash
sudo nano /etc/tmpfiles.d/my_temp_dir.conf
```
And apply the following as contents:
```bash
d /run/my_temp_dir 0755 labex labex 1m
```
It would do the following:
Explanation of the line:
- `d`: Specifies that this entry defines a directory.
- `/run/my_temp_dir`: The path to the directory.
- `0755`: The permissions for the directory.
- `labex labex`: The owner and group for the directory.
- `1m`: The age after which files in this directory should be deleted (1 minute).

## Configuring Logging
### systemd-journald
This is the primary method for logging on RHEL 10.

This receives logs from several locations and by default, **is not persistent**.
- kernel
- boot procedures
- syslog events
- standard output & errors from daemons

A common way to use this is to run `systemctl status name.unit` - which prints out the latest log for the specific service.
```bash
student@rhcsaserver:~$ systemctl status sshd.service 
● sshd.service - OpenSSH server daemon
     Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; preset: enabled)
     Active: active (running) since Mon 2026-04-06 16:28:18 CDT; 4h 51min ago
 Invocation: 6bf94a05ab7441d4a7cc841b3ec9a465
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 1295 (sshd)
      Tasks: 1 (limit: 35471)
     Memory: 2.3M (peak: 2.6M)
        CPU: 5ms
     CGroup: /system.slice/sshd.service
             └─1295 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

Apr 06 16:28:18 rhcsaserver.example.com systemd[1]: Starting sshd.service - OpenSSH server daemon...
Apr 06 16:28:18 rhcsaserver.example.com sshd[1295]: Server listening on 0.0.0.0 port 22.
Apr 06 16:28:18 rhcsaserver.example.com sshd[1295]: Server listening on :: port 22.
Apr 06 16:28:18 rhcsaserver.example.com systemd[1]: Started sshd.service - OpenSSH server daemon.
```
You can also run `journalctl` which prints the entire journal.

You can also use different options. A few examples are:

| Command                      | Description                                                       |
| ---------------------------- | ----------------------------------------------------------------- |
| `journalctl -p err`          | Only shows messages with priority error and higher                |
| `journalctl -f`              | Prints the last 10 lines and shows new messages as they are added |
| `journalctl -u sshd.service` | Shows messages for `sshd.service` only                            |
### Preserving the systemd Journal

> [!Important]
> This is likely to be an exam task

By default, log persistence is handled by rsyslog. All messages generated by the systemd journal are forwarded to rsyslog for storage.

If you want the journal itself to retain logs persistently, modify the configuration file at `/usr/lib/systemd/journal.conf`. Change the `Storage` setting from its default value of `auto` to `persistent`.

### rsyslogd
The `rsyslogd` service must be running for log collection and forwarding to function properly.

The main configuration file is located at `/etc/rsyslog.conf`, with additional drop-in configurations supported in `/etc/rsyslog.d/`.

Each rsyslog rule consists of three components:
- **Facility** – identifies the source or category of the log (e.g., `auth`, `cron`, `daemon`)
- **Severity** – defines the level of importance to capture (e.g., `info`, `warning`, `err`)
- **Destination** – specifies where the logs are written (e.g., a file, remote server, or pipe)
- - Rules follow the format:  
    `facility.severity destination`
- Multiple facilities can be combined with commas (e.g., `auth,authpriv.*`)
- Exclude with `none`:  
`auth.info;authpriv.none /var/log/auth.log`
### logrotate
- Managed by a **systemd timer** (`logrotate.timer`)
- Runs automatically (typically daily)
- Main config: `/etc/logrotate.conf`
- Per-service configs: `/etc/logrotate.d/`
# Managing Storage
## Working with Partitions and Mounts
### UEFI Disk Layout

| Component              | Location / Size              | Key Purpose                                      |
|----------------------|-----------------------------|--------------------------------------------------|
| Reserved Space       | First **1 MiB**              | Alignment + metadata                             |
| Protective MBR       | First **512 bytes (sector 0)** | Prevents legacy tools from overwriting GPT        |
| GPT Header           | Sector 1                     | Defines GPT structure                            |
| GPT Partition Table  | Next **32 sectors (16 KiB)** | Stores partition entries                         |
| Partition Entries    | **128 entries × 128 bytes**  | Defines partitions                               |
| EFI System Partition | Separate partition           | Contains bootloaders for UEFI                    |

- Systems **must have partitions**, including an **EFI System Partition (ESP)** for UEFI boot  
- **Partitions are required even when using LVM**  
- A **filesystem must be created on a partition** before use  
- Common practice: use **multiple partitions** to separate data (e.g., `/`, `/home`, `/var`)  
### Listing Block Devices
Use `lsblk` to get an overview of currently existing devices.
- `/dev/nvmexny` is used for VNME devices
- `/dev/sdx` is common for SCSI and SATA devices
- `/dev/vdx` is common for KVM virtual machines
```bash
student@rhcsaserver:/etc$ lsblk 
NAME          MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sr0            11:0    1 1024M  0 rom  
vda           252:0    0   40G  0 disk 
├─vda1        252:1    0    1M  0 part 
├─vda2        252:2    0    1G  0 part /boot
└─vda3        252:3    0   39G  0 part 
  ├─rhel-root 253:0    0   35G  0 lvm  /
  └─rhel-swap 253:1    0    4G  0 lvm  [SWAP]
```
#### Partition Numbering
On **sd** and **vd** devices, partitions are numbered in order:
- `/dev/sda2` is the second partition on the first hard disk.
- `/dev/sdb1` is the first partition on the second hard disk.
On **nvme** devices, partition names are appended to the device name:
- `/dev/nvme0n1p1` is the first partition on the first hard disk
- `/dev/nvme0n3p2` is the second partition on the third hard disk
### Understanding GPT and MBR Partitions

| Feature           | MBR               | GPT                    |
| ----------------- | ----------------- | ---------------------- |
| Year Introduced   | 1981              | Modern (replaces MBR)  |
| Disk Size Limit   | ~2 TiB            | Very large (>> 2 TiB)  |
| Max Partitions    | 4 primary         | 128                    |
| Partition Storage | 64 bytes (in MBR) | 16 KiB partition table |
| Boot Method       | Legacy BIOS       | UEFI                   |
| Modern Usage      | `Deprecated`      | Standard               |
### Creating Partitions
The two common tools used to manage partitions are `fdisk` and `parted`.
- `fdisk` → typically used with MBR
- `parted` → preferred for GPT/UEFI systems

#### Partition Types

| Type   | Purpose                          |
|--------|----------------------------------|
| linux  | Standard Linux filesystem         |
| swap   | Swap space                        |
| uefi   | EFI System Partition (boot)       |
| lvm    | Used for LVM physical volumes     |

- Partition type = identifier for how a partition is intended to be used  
- On modern systems, type is **less critical**, but still useful for organization  
- In `fdisk`, use:
  - `t` → change partition type  
  - `l` → list available types  

After creating a partition, verify your work:

```bash
lsblk
```bash
student@rhcsaserver:~$ lsblk 
NAME          MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sr0            11:0    1 1024M  0 rom  
vda           252:0    0   40G  0 disk 
├─vda1        252:1    0    1M  0 part 
├─vda2        252:2    0    1G  0 part /boot
└─vda3        252:3    0   39G  0 part 
  ├─rhel-root 253:0    0   35G  0 lvm  /
  └─rhel-swap 253:1    0    4G  0 lvm  [SWAP]
vdb           252:16   0   20G  0 disk 
└─vdb1        252:17   0   20G  0 part 
```
### Creating and Mounting File Systems
#### Common File Systems
- **XFS** (RHEL default)
  - Fast and scalable  
  - Can grow, but **cannot shrink**  
- **Ext4**
  - Older default (RHEL 6), still supported  
  - Backward compatible with Ext2  
  - Can **grow and shrink**  
- **vfat**
  - Supports multiple operating systems  
  - Commonly used for **EFI System Partition (ESP)** and shared media  
#### Creating and Mounting a File System
A partition is not very useful without a file system. 
You can use some of the following commands to create one on a parition.
`mkfs.xfs` - creates an xfs file system.
`mkfs.ext4` - creates an ext4 file system.
`mkfs.[Tab][Tab]` to show a list of available file systems.
```bash
student@rhcsaserver:~$ mkfs[TAB][TAB]
mkfs         mkfs.erofs   mkfs.ext2    mkfs.ext4    mkfs.minix   mkfs.vfat    
mkfs.cramfs  mkfs.exfat   mkfs.ext3    mkfs.fat     mkfs.msdos   mkfs.xfs
```
Example:
```bash
student@rhcsaserver:~$ sudo mkfs.ext4 /dev/vdb1

mke2fs 1.47.1 (20-May-2024)
Discarding device blocks: done                            
Creating filesystem with 5242364 4k blocks and 1310720 inodes
Filesystem UUID: 4ed83bcc-c612-42ba-adfd-131eaff7c57d
Superblock backups stored on blocks: 
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
	4096000

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done   
```

After creating the file system, you can mount it by doing the following.
```bash
student@rhcsaserver:~$ sudo mount /dev/vdb1 /mnt/

student@rhcsaserver:~$ lsblk
NAME          MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sr0            11:0    1 1024M  0 rom  
vda           252:0    0   40G  0 disk 
├─vda1        252:1    0    1M  0 part 
├─vda2        252:2    0    1G  0 part /boot
└─vda3        252:3    0   39G  0 part 
  ├─rhel-root 253:0    0   35G  0 lvm  /
  └─rhel-swap 253:1    0    4G  0 lvm  [SWAP]
vdb           252:16   0   20G  0 disk 
└─vdb1        252:17   0   20G  0 part /mnt

```
### Mounting Partitions through `/etc/fstab`
For a more persistent mount, it is add the device into `/etc/fstab`.
```bash
student@rhcsaserver:~$ cat /etc/fstab 

#
# /etc/fstab
# Created by anaconda on Mon Mar 16 01:26:19 2026
#
# Accessible filesystems, by reference, are maintained under '/dev/disk/'.
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info.
#
# After editing this file, run 'systemctl daemon-reload' to update systemd
# units generated from this file.
#
UUID=47b2eee4-5f8d-42d6-9746-01a3681f5c0f /                       xfs     defaults        0 0
UUID=90c4ef48-c9a8-461b-81d5-9089dbb3b7ea /boot                   xfs     defaults        0 0
UUID=99e1d6b4-4ae6-47ba-87ca-2c59073eed00 none                    swap    defaults        0 0
```
You can update the changes by running `systemctl daemon-reload`.
> [!WARNING]
> If you have an error in `/etc/fstab`, your system will drop a troubleshooting shell while booting.

Use `mount -a` to mount all that hasn't been mounted yet.
Alternatively, use `findmnt --verify` to verify synstax.
```bash
student@rhcsaserver:~$ sudo findmnt --verify
none
   [W] target specified more than once
none
   [E] unsupported source tag: UID=99e1d6asdfasdf-87ca-2c59073eed00
   [W] cannot detect on-disk filesystem type (No such file or directory)
```

```bash
student@rhcsaserver:~$ sudo findmnt --verify
Success, no errors or warnings detected
```
### Using UUID and Labels
Sometimes, block device names may change, therefor, it may make sense to use a different solution.
- **UUID**: This is automatically generated for each device that contains a file system
- **Label**: This can be set when the file system is created

You can use `lsblk --fs` to include the **UUID** & **Label**.
```bash
student@rhcsaserver:~$ lsblk --fs 
NAME          FSTYPE      FSVER    LABEL UUID                                   FSAVAIL FSUSE% MOUNTPOINTS
sr0                                                                                            
vda                                                                                            
├─vda1                                                                                         
├─vda2        xfs                        90c4ef48-c9a8-461b-81d5-9089dbb3b7ea    607.7M    37% /boot
└─vda3        LVM2_member LVM2 001       TtWr5a-jyuY-2xy7-peM0-XQKW-oLFU-EF3y0r                
  ├─rhel-root xfs                        47b2eee4-5f8d-42d6-9746-01a3681f5c0f     30.1G    14% /
  └─rhel-swap swap        1              99e1d6b4-4ae6-47ba-87ca-2c59073eed00                  [SWAP]
vdb                                                                                            
└─vdb1        ext4        1.0            4ed83bcc-c612-42ba-adfd-131eaff7c57d     18.5G     0% /mnt
```
### Creating a Swap Partition
Swap is **disk space used as an extension of RAM**
Swap can be created on any block device.

You can do so by setting a partition type to `linux-swap`
```bash
student@rhcsaserver:~$ sudo fdisk /dev/vdb

Welcome to fdisk (util-linux 2.40.2).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): p
Disk /dev/vdb: 20 GiB, 21474836480 bytes, 41943040 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 6984BD61-ED82-4015-9EFD-54B939C2D9B1

Device     Start      End  Sectors Size Type
/dev/vdb1   2048 41940991 41938944  20G Linux swap
```
After creating the swap partition, use `mkswap` to create the swap file-system
```bash
student@rhcsaserver:~$ sudo mkswap /dev/vdb1 
mkswap: /dev/vdb1: warning: wiping old ext4 signature.
Setting up swapspace version 1, size = 20 GiB (21472735232 bytes)
no label, UUID=52f6d4f2-50b7-421a-8ed1-574f671a3d25
```
Then, add to `/etc/fstab` by UUID
```bash
# /etc/fstab
# Created by anaconda on Mon Mar 16 01:26:19 2026
#
# Accessible filesystems, by reference, are maintained under '/dev/disk/'.
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info.
#
# After editing this file, run 'systemctl daemon-reload' to update systemd
# units generated from this file.
#
UUID=47b2eee4-5f8d-42d6-9746-01a3681f5c0f /                       xfs     defaults        0 0
UUID=90c4ef48-c9a8-461b-81d5-9089dbb3b7ea /boot                   xfs     defaults        0 0
UUID=99e1d6b4-4ae6-47ba-87ca-2c59073eed00 none                    swap    defaults        0 0
UUID=52f6d4f2-50b7-421a-8ed1-574f671a3d25 none                    swap    defaults        0 0
```
Turn it on
```bash
student@rhcsaserver:~$ sudo swapon -a
student@rhcsaserver:~$ swapon 
NAME      TYPE      SIZE USED PRIO
/dev/dm-1 partition   4G   0B   -2
/dev/vdb1 partition  20G   0B   -3
```
## Managing Advanced Storage
### LVM Overview

```
Physical Volume → Volume Group → Logical Volume → mkfs → mount
```
- You take **raw disks (`Physical Volume / PVs`)**
    - Disk or partition initialized for LVM
    - Example: `/dev/sdb1`
- Combine them into a **storage pool (`Volume Group / VG`)**
	- Pool of storage made from one or more PVs
    - Example: `vg_data`
- Carve out **usable volumes (`Logical Volume / LV`)**
	- Virtual block device carved from VG
    - Example: `lv_data`
- Then:
    - Format → `mkfs`
    - Mount → `/etc/fstab`
### Creating an LVM Volume
```bash
# Create Physical Volume
pvcreate /dev/<partition>

# Create Volume Group
vgcreate <vg_name> /dev/<partition>

# Create Logical Volume
lvcreate --namen <lv_name> --size <size> <vg_name>

# Format Logical Volume
mkfs.xfs /dev/<vg_name>/<lv_name>

# Mount Logical Volume
mount /dev/<vg_name>/<lv_name> <mount_point>
```
### Device Mapper (High-Level Notes)
- Kernel subsystem that creates virtual block devices
- Acts as a layer between physical storage and higher-level tools
- Used by LVM, encryption (dm-crypt), and RAID

`Physical Disk → LVM → Device Mapper → Block Device → Filesystem`
- LVM defines structure (`VG`/`LV`)
- Device Mapper creates the actual usable device

Device Mapper is the back end system that creates block devices; LVM uses it, but you should interact with the persistent LVM paths instead of `dm-*` names.
### Resizing LVM Logical Volumes
Use `vgs` to verify the volume group has unused disk space.
If the volume group does not have enough free space, add one or more physical volumes to it.
```bash
pvcreate /dev/<disk>
vgextend <vg_name> /dev/<disk>
```
#### Extend by size
```bash
lvextend --size +5G /dev/<vg>/<lv>
```
#### Extend using all free space
```bash
lvextend --extents +100%FREE /dev/<vg>/<lv>
```
#### Extend + resize file-system
```bash
lvextend --resizefs --size +5G /dev/<vg>/<lv>
```
```bash
lvextend --resizefs --extents +100%FREE /dev/<vg>/<lv>
```
#### If you forget to use `resizefs`...
You can use `resize2fs` to adjust the size on a `Ext` file-system
You can also use `xfs_growfs` to increase the size on a `XFS` file-system.

> [!WARNING]
> `Ext4` can be extended and shrunk
> 
> `XFS` can be extended, but not shrunk
### Reducing Volume Groups
If a volume group (`VG`) contains multiple physical volumes (`PV`), you can remove a `PV` **only if all data (extents) on that `PV` can be moved elsewhere within the `VG`**.
- Data in LVM is stored as **extents** across all PVs in the VG
- You **cannot remove a PV that still contains allocated extents**
- So the process is:
    1. **Move extents off the `PV`**
    2. **Remove the `PV` from the `VG`**
#### 1) Check current layout
```bash
pvs  
vgs  
lvs
```
Look for:
- Which PV you want to remove
- How much data is on it
- Whether other PVs have enough free space
#### 2) Move data off the PV
```bash
pvmove /dev/<pv_to_remove>
```
- Moves all allocated extents from that PV → to other PVs in the VG
- Can also be more controlled:
```bash
pvmove /dev/<pv_to_remove> /dev/<target_pv>
```
#### 3) Verify the PV is empty
```bash
pvs
```
- The PV should show **0 allocated extents**
#### 4) Remove the PV from the VG
```bash
vgreduce <vg_name> /dev/<pv_to_remove>
```
# Advanced System Administration Tasks
## Managing the Boot Procedure
### Exploring the Boot Procedure
**UEFI / BIOS**
  - Initializes hardware → loads bootloader

**GRUB**
  - Selects and loads kernel + initramfs

**Kernel**
  - Initializes hardware → starts initramfs

**initramfs**
  - Loads drivers → mounts real root filesystem

**systemd (PID 1)**
  - Starts targets and services

**Boot targets**
  - multi-user.target (CLI) or graphical.target (GUI)

**User space**
  - Login prompt → shell or desktop
### Modifying Grub2 Runtime Parameters
While booting, press `e` to edit the runtime boot options to the end of the line that starts with `linux`.
- `systemd.unit=emergency.target`
- `systemd.unit=rescue.target`
Press `c` to enter command mode (not very necessary for RHCSA).
- You can type `help` for an overview of options
- You can do `exit` to get out
### Changing Grub2 Persistent Parameters
```bash
student@rhcsaserver:~$ cat /etc/default/grub
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="crashkernel=2G-64G:256M,64G-:512M resume=UUID=99e1d6b4-4ae6-47ba-87ca-2c59073eed00 rd.lvm.lv=rhel/root rd.lvm.lv=rhel/swap rhgb quiet"
GRUB_DISABLE_RECOVERY="true"

# This line means that configs are loaded from kernel specific .conf files
GRUB_ENABLE_BLSCFG=true
```
If the above line is present (Default in RHEL 9 and 10), then Boot entries are stored in:
```bash
/boot/loader/entries/*.conf
```
#### grubby
Instead of manually editing the above .conf files, you can use grubby.
Set Persistent Kernel Parameters
```bash
grubby --update-kernel=ALL --args="param"
```
Remove Parameter
```bash
grubby --update-kernel=ALL --remove-args="param"
```
### Managing Systemd Targets
A Systemd target is a group of unit files.
Some targets are **isolatable**, which means they define the final state of a system.

| Target              | Description                              |
| ------------------- | ---------------------------------------- |
| `emergency.target`  | Minimal system (no services, root shell) |
| `rescue.target`     | Single-user mode (basic services)        |
| `multi-user.target` | Functional system without a GUI          |
| `graphical.target`  | Functional system with a GUI             |
- When you enable a unit:
    `systemctl enable <unit>`
- It creates a symbolic link in a target directory, usually:
    `/etc/systemd/system/<target>.wants/`
- This means:  
    → The unit will start **when that target is reached**
### Setting Targets
#### Get current default target
```bash
root@rhcsaserver:~# systemctl get-default 
graphical.target
```
#### Set default target
```bash
systemctl set-default multi-user.target
```
#### Switch target (current session)
```bash
systemctl isolate multi-user.target
```
#### Booting into a target
On the Grub 2 boot menu, add `systemd.unit=xxx.target` at the end of the line that begins with "`linux`" to boot into a specific target.
## Recovering a Lost Root User Password
> [!IMPORTANT]
>

Enter the grub menu while booting, press `e` to edit the kernel, and then add `init=/bin/bash` to the end of the line that starts with `"linux"`.
Once in the minimal environment, enter the following command to get into a read + write mode.
```bash
mount -o remount,rw /
```
Then, use `passwd` to change the root password.
```bash
passwd root
```
After doing so, create the file `.autorelable` in the root directory to handle the `SELinux` stuff.
```bash
touch /.autorelable
```
## Basic Bash Scripting
Be sure to provide the type of script at the top of the file
```bash
#!/bin/bash
Script contents...
```
You can print text by using `echo`
```bash
#!/bin/bash
echo "This is example text"
```
You can provide variables as well
```bash
#!/bin/bash

name=bradley

echo "Hello $name"

#Output
# Hello bradley
```
You can provide a positional argument with `$1` or `$2` and so on...
```bash
#!/bin/bash

echo "First arg: $1"
echo "Second arg: $2"
```
You can also add the output of commands as a variable.
```bash
#!/bin/bash
date=$(date)
user=$(whoami)
dir=$(pwd)

echo "Hello $user, you're currently in $dir and today's date is $date."
```
The `test` command can be used to test many things

Need to add some more notes here...
# Managing and Securing Network Services
## SSH
### SSH Key Based Login
#### Create a key pair
```bash
student@rhcsaserver:~$ ssh-keygen
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/student/.ssh/id_ed25519): testkey1
Enter passphrase for "testkey1" (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in testkey1
Your public key has been saved in testkey1.pub
The key fingerprint is:
SHA256:OQ9eUrnZFd8FrMHC0LUG0sC140hHlGSvH9rIvSuLQMo student@rhcsaserver.example.com
The key's randomart image is:
+--[ED25519 256]--+
|       .+XB+..o..|
|        .+*=o..oo|
|        . *.+o. o|
|       . * B..   |
|      . S * o    |
|   . o . B * .   |
|    E . . = +    |
|       . ..  .   |
|        . .oo.   |
+----[SHA256]-----+

########################################################################
student@rhcsaserver:~$ cat testkey1
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACCpS/OleiupQDsa3RL4ocRMP6SIB50srVdRbN0I8fhrcwAAAKiy7Wpksu1q
ZAAAAAtzc2gtZWQyNTUxOQAAACCpS/OleiupQDsa3RL4ocRMP6SIB50srVdRbN0I8fhrcw
AAAEAQli7nuodu9aOE2URsj4ufkJpQ6QBPu+b4rZDeXtYFgKlL86V6K6lAOxrdEvihxEw/
pIgHnSytV1Fs3Qjx+GtzAAAAH3N0dWRlbnRAcmhjc2FzZXJ2ZXIuZXhhbXBsZS5jb20BAg
MEBQY=
-----END OPENSSH PRIVATE KEY-----

########################################################################
student@rhcsaserver:~$ cat testkey1.pub 
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIKlL86V6K6lAOxrdEvihxEw/pIgHnSytV1Fs3Qjx+Gtz student@rhcsaserver.example.com
```
#### Copy the public key to the target
```bash
student@rhcsa-server-01:~$ sudo ssh-copy-id -i ./testkey1.pub student@192.168.100.44
[sudo] password for student: 
/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "./testkey1.pub"
The authenticity of host '192.168.100.44 (192.168.100.44)' can't be established.
ED25519 key fingerprint is SHA256:meZUZYXrx5Lr4gA0Z+nn+mFj3AUp8frQ64AhOUAP6qs.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
student@192.168.100.44's password: 

Number of key(s) added: 1

Now try logging into the machine, with: "ssh -i ./testkey1 'student@192.168.100.44'"
and check to make sure that only the key(s) you wanted were added.
```
You can then authenticate to the server with the following command:
```bash
ssh user@server -i /path/to/private/key
```
### Common SSH Server Options
Changes are made in `/etc/ssh/sshd_config`.
Important settings:

| Setting                  | Description                                                              |
| ------------------------ | ------------------------------------------------------------------------ |
| `Port`                   | The port which the service will run. The default is `22`                 |
| `PermitRootLogin`        | Disabled by default. It's a good idea to keep it that way                |
| `PubkeyAuthentication`   | Allows the use of keys as listed above                                   |
| `PasswordAuthentication` | Enabled by default, but it's a good idea to disable once keys are set up |
| `X11Forwarding`          | Allows/Denies forwarding of graphical applications                       |
| `AllowUsers`             | Allows the admin to set who may login via ssh                            |
### Copying files securely
You can use `scp` which is essentially a file transfer option which uses `ssh` under the hood.
### Syncing files securely
You can use `rsync` which will use `ssh` under the hood to sync files or directories.
## Time

| Command       | Description                                                 |
| ------------- | ----------------------------------------------------------- |
| `hwclock`     | Used to set hardware time. It can also be used to sync time |
| `timedatectl` | Used to manage time and time zone config                    |
### Managing Time
```bash
root@rhcsa-server-01:~# timedatectl --help
timedatectl [OPTIONS...] COMMAND ...

Query or change system time and date settings.

Commands:
  status                   Show current time settings
  show                     Show properties of systemd-timedated
  set-time TIME            Set system time
  set-timezone ZONE        Set system time zone
  list-timezones           Show known time zones
  set-local-rtc BOOL       Control whether RTC is in local time
  set-ntp BOOL             Enable or disable network time synchronization

systemd-timesyncd Commands:
  timesync-status          Show status of systemd-timesyncd
  show-timesync            Show properties of systemd-timesyncd
  ntp-servers INTERFACE SERVER…
                           Set the interface specific NTP servers
  revert INTERFACE         Revert the interface specific NTP servers
```
### Managing NTP
The default NTP service for RHEL is `chronyd`.
The configuration is in `/etc/chrony.conf`.
```bash
root@rhcsa-server-01:~# cat /etc/chrony.conf 
# Use public servers from the pool.ntp.org project.
# Please consider joining the pool (https://www.pool.ntp.org/join.html).
pool 2.rhel.pool.ntp.org iburst

# Use NTP servers from DHCP.
sourcedir /run/chrony-dhcp

# Record the rate at which the system clock gains/losses time.
driftfile /var/lib/chrony/drift

# Allow the system clock to be stepped in the first three updates
# if its offset is larger than 1 second.
makestep 1.0 3

# Enable kernel synchronization of the real-time clock (RTC).
rtcsync
# etc...
```
## SELinux
### Why SELinux is used
Default Linux security has a few flaws by default which SELinux attempts to solve.
The basic principle is that if something isn't explicitly allowed, then it will be denied.
- Unknown services will need additional configuration with SELinux for them to work.
### SELinux Labels
SELinux controls access using **labels (contexts)**.

Format:
```bash
user:role:type:level
           ^
########################
		This Part
########################
ls -laZ
-rw-------. 1 bradley bradley unconfined_u:object_r:user_home_t:s0     8721 Apr 16 19:30 .bash_history

```
**Only the `type` matters for RHCSA** (ends in `_t`)

You can view these labels with `Z` in a few commands.
```bash
ps auxZ | grep http
```
and 
```bash
ls -laZ
```

Common Context Examples

|Item|Type Context|
|---|---|
|Apache process|`httpd_t`|
|Web content|`httpd_sys_content_t`|
|Temp files|`tmp_t`|
|Web ports|`http_port_t`|
|MariaDB process|`mysqld_t`|
|MariaDB data|`mysqld_db_t`|
**tldr**
- Processes run in a type
- Files have a type
- SELinux policy defines what can talk to what (even if permissions are 777)
```bash
process_type → file_type → allowed?
```
**Example: Web Server (Apache)**
- **Process**: `httpd_t`
- **Web files**: `httpd_sys_content_t`
```bash
# ALLOWED
httpd_t → httpd_sys_content_t
```
```bash
# NOT ALLOWED
httpd_t → tmp_t
```
### Managing SELinux Modes
It's either `enabled` or `disabled`.

When `enabled`.
- `Enforcing`
- `Permissive`

You can manage the default in `/etc/sysconfig/selinux`.

The command `getenforce` will show the current state.
The command `setenforce` toggles between `Enforcing` and `Permissive`.
### Managing labels
You can repair the context of many default files with `restorecon`, but this will not work for completely new directories.
You can manually set the context labels with `semanage fcontext -a` (new context label) or  `-m` (modify existing) which will write to the policy, but not the file-system. You must then use `restorecon` to enforce the change.

> [!NOTE]
> Using `man semanage-fcontext` is a good idea to check for examples
### Managing SELinux Port Access
Network ports are also provided with an SELinux context label.
The ssh config file has a good reference.
```bash
# If you want to change the port on a SELinux system, you have to tell
# SELinux about this change.
# semanage port -a -t ssh_port_t -p tcp #PORTNUMBER
```
### Using boolean settings to modify SELinux

| Command                         | Description                     |
| ------------------------------- | ------------------------------- |
| `semanage boolean -l`           | Get an overview of all booleans |
| `setsebool -P <boolean> on\off` | Set the booleans                |
| `getsebool -a`                  | Get the current settings        |
### Troubleshooting SELinux
SELinux uses `auditd` to write log messages to the audit log.
- These may be difficult to interpret
You can use `sealert` to try to get more meaningful messages from the logs.
## Firewalls
You can use `firewall-cmd` to make services available. You can add the `--permanent` to make changes persistently, but they don't apply to the runtime.
A `firewalld` service is an `XML` file that contains info about ports and other features that must be allowed.
Default services are in `/usr/lib/firewalld/services`.
Custom services can be created in `/etc/firewalld/services`.
- An easy way to create a custom service is to copy an existing service file from `/usr/lib/firewalld/services` to `/etc/firewalld/services` and modify it.

| Command                                       | Description                                                         |
| --------------------------------------------- | ------------------------------------------------------------------- |
| `systemctl status firewalld`                  | Show whether the firewall is currently running.                     |
| `firewall-cmd --list-all`                     | List everything added or enabled.                                   |
| `firewall-cmd --get-services`                 | List all predefined service names.                                  |
| `firewall-cmd --info-service=ssh`             | Get info on the predefined service for ssh                          |
| `firewall-cmd --add-service=http`             | Enable http service in default zone.                                |
| `firewall-cmd --add-service=http --permanent` | Enable http service in default zone persistently.                   |
| `firewall-cmd --add-port 82/tcp --permanent`  | Opens port 82 (requires reloading the firewall after making change) |
| `firewall-cmd --reload`                       | Reload the firewall                                                 |

## Remote File-Systems and AutoFS
### Configuring a NFS server

> [!NOTE]
> Not a task that is required for exam itself, but good to know and useful when you are setting up home labs

Start off with installing **nfs-utils**
```bash
dnf install nfs-utils
```
Create some directories to share
```bash
mkdir -pv /nfsdata /home/ldapuser{1..9}
```
Create a share (for a lab, the permissions can be a little loose)
```bash
echo "/nfsdata *(rw,no_root_squash)" >> /etc/exports
```
```bash
echo "/home/ldap *(rw,no_root_squash)" >> /etc/exports
```
Verify the contents
```bash
root@rhcsa-server-02:/repo# cat /etc/exports
/nfsdata *(rw,no_root_squash)
/home/ldap *(rw,no_root_squash)
```
Enable the service
```bash
systemctl enable --now nfs-server.service
```
Allow the services through the firewall
```bash
root@rhcsa-server-02:/repo# firewall-cmd --add-service=rpc-bind --permanent 
success
root@rhcsa-server-02:/repo# firewall-cmd --add-service=mountd --permanent 
success
root@rhcsa-server-02:/repo# firewall-cmd --add-service=nfs --permanent 
success
```
Reload the firewall and verify they are present in the rules
```bash
root@rhcsa-server-02:/repo# firewall-cmd --reload 
success
root@rhcsa-server-02:/repo# firewall-cmd --list-all
public (default, active)
  target: default
  ingress-priority: 0
  egress-priority: 0
  icmp-block-inversion: no
  interfaces: enp1s0
  sources: 
  services: cockpit dhcpv6-client mountd nfs rpc-bind ssh
  ports: 
  protocols: 
  forward: yes
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
```
### Mounting NFS Shares
First, make sure **nfs-utils** are installed on the client.
```bash
dnf install nfs-utils
```
To discover what's available, you can use the following command.
```bash
showmount -e <server-ip>
```

```bash
root@rhcsa-server-01:/mnt# showmount -e 192.168.100.44
Export list for 192.168.100.44:
/home/ldap *
/nfsdata   *
```

You can mount the share temporarily with:
```bash
mount <server-ip>:/<share-name> /mnt
```
You can also mount via `fstab`:
```bash
root@rhcsa-server-01:/mnt# cat /etc/fstab 
#
# /etc/fstab
# Created by anaconda on Mon Mar 16 01:26:19 2026
#
# Accessible filesystems, by reference, are maintained under '/dev/disk/'.
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info.
#
# After editing this file, run 'systemctl daemon-reload' to update systemd
# units generated from this file.
#
UUID=47b2eee4-5f8d-42d6-9746-01a3681f5c0f /                       xfs     defaults        0 0
UUID=90c4ef48-c9a8-461b-81d5-9089dbb3b7ea /boot                   xfs     defaults        0 0
UUID=99e1d6b4-4ae6-47ba-87ca-2c59073eed00 none                    swap    defaults        0 0
UUID=2f726ee7-9cfd-4432-8236-fde65dbab62c /mounts/lvdb            xfs     defaults        0 0
192.168.100.44:/nfsdata  		  /mnt  		  nfs     defaults       0 0
```
### Configuring Automount
Start off with making sure `autofs` is installed
```bash
dnf install autofs
```
Start and enable the service.
```bash
systemctl enable --now autofs
```
Check the `/etc/auto.master` file
```bash
root@rhcsa-server-01:/etc# cat /etc/auto.master
#
# Sample auto.master file
# This is a 'master' automounter map and it has the following format:
# mount-point [map-type[,format]:]map [options]
# For details of the format look at auto.master(5).
#
/misc	/etc/auto.misc
#
```
It points to `/etc/auto.misc`
```bash
root@rhcsa-server-01:/etc# cat /etc/auto.misc 
#
# This is an automounter map and it has the following format
# key [ -mount-options-separated-by-comma ] location
# Details may be found in the autofs(5) manpage

cd		-fstype=iso9660,ro,nosuid,nodev	:/dev/cdrom

# the following entries are samples to pique your imagination
#linux		-ro,soft		ftp.example.org:/pub/linux
#boot		-fstype=ext2		:/dev/hda1
#floppy		-fstype=auto		:/dev/fd0
#floppy		-fstype=ext2		:/dev/fd0
#e2floppy	-fstype=ext2		:/dev/fd0
#jaz		-fstype=ext2		:/dev/sdc1
#removable	-fstype=ext2		:/dev/hdd
```
The above line containing `#linux		-ro,soft		ftp.example.org:/pub/linux` is an NFS example.
We can create a new autofs mount for nfs by editing `/etc/auto.master` and adding the following.
```bash
#
# Sample auto.master file
# This is a 'master' automounter map and it has the following format:
# mount-point [map-type[,format]:]map [options]
# For details of the format look at auto.master(5).
#
/misc   /etc/auto.misc
/nfsdata /etc/auto.nfsdata
#
```
And then creating `/etc/auto.nfsdata`.
```bash
root@rhcsa-server-01:/etc# touch /etc/auto.nfsdata
root@rhcsa-server-01:/etc# ls -la | grep auto
-rw-r--r--.   1 root                 root                  16240 May 11  2025 autofs.conf
-rw-------.   1 root                 root                    232 May 11  2025 autofs_ldap_auth.conf
-rw-r--r--.   1 root                 root                   1316 Apr 21 15:10 auto.master
drwxr-xr-x.   2 root                 root                      6 May 11  2025 auto.master.d
-rw-r--r--.   1 root                 root                    519 May 11  2025 auto.misc
-rwxr-xr-x.   1 root                 root                    901 May 11  2025 auto.net
-rw-r--r--.   1 root                 root                      0 Apr 21 15:12 auto.nfsdata
-rwxr-xr-x.   1 root                 root                   2087 May 11  2025 auto.smb
```
And using the `auto.misc` as a reference, edit the new config.
`server2` in this example is just the local mount name.
```bash
root@rhcsa-server-01:/etc# cat auto.nfsdata 
server2		-rw		192.168.100.44:/nfsdata
```
Then, restart the service and verify that it is present
```bash
root@rhcsa-server-01:/etc# systemctl restart autofs.service 
root@rhcsa-server-01:/nfsdata# cd server2/
root@rhcsa-server-01:/nfsdata/server2# ls -la
total 0
drwxr-xr-x. 3 root root 174 Apr 21 14:08 .
drwxr-xr-x. 4 root root   0 Apr 21 15:37 ..
drwxr-xr-x. 2 root root  24 Apr 21 15:34 testdir
-rw-r--r--. 1 root root   0 Apr 21 14:08 testfile1
-rw-r--r--. 1 root root   0 Apr 21 14:08 testfile2
-rw-r--r--. 1 root root   0 Apr 21 14:08 testfile3
-rw-r--r--. 1 root root   0 Apr 21 14:08 testfile4
-rw-r--r--. 1 root root   0 Apr 21 14:08 testfile5
-rw-r--r--. 1 root root   0 Apr 21 14:08 testfile6
-rw-r--r--. 1 root root   0 Apr 21 14:08 testfile7
-rw-r--r--. 1 root root   0 Apr 21 14:08 testfile8
-rw-r--r--. 1 root root   0 Apr 21 14:08 testfile9
```
You can also verify with `mount`
```bash
root@rhcsa-server-01:/# mount | grep nfs
/etc/auto.nfsdata on /nfsdata type autofs (rw,relatime,fd=9,pgrp=9496,timeout=300,minproto=5,maxproto=5,indirect,pipe_ino=42259)
192.168.100.44:/nfsdata on /nfsdata/testdir type nfs4 (rw,relatime,vers=4.2,rsize=1048576,wsize=1048576,namlen=255,hard,fatal_neterrors=none,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=192.168.100.192,local_lock=none,addr=192.168.100.44)
192.168.100.44:/nfsdata on /nfsdata/server2 type nfs4 (rw,relatime,vers=4.2,rsize=1048576,wsize=1048576,namlen=255,hard,fatal_neterrors=none,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=192.168.100.192,local_lock=none,addr=192.168.100.44)
```
