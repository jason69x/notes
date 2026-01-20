
###### What Is the Shell?
the *shell* is a program that takes keyboard commands and passes them to the operating system to carry out.

*terminal emulators* give us access to the shell. eg- konsole.

*virtual consoles/terminals*, `ctrl-alt-F1:6` `alt-F1:6` `alt-F7`
kernel managed terminals (tty)

terminal emulators launch shell as a child process.

display server controls one of tty. runs on top of it & switch it to graphics mode.
###### Navigation
`pwd` - print working directory
`cd` - change directory
`ls` - list directory content

linux organizes its files in a *heirarchical directory structure*.

unlike windows,which has a separate file system tree for each storage device, linux always have a single file system tree.

`.` - refers to working directory, same inode as the directory
`..` - refers to parent of working directory , same inode as parent

each file,directory,devices etc. has a inode, directory inode points to data blocks that contains directory entries (filename -> inode)
inode table in disk.

these are ***special directory entries***. they are ***hard links***.
created automatically by the filesystem when a directory is created.
every directory has them.
a directory is essentially a table : `filename -> inode`

we can't create hardlinks to directories coz they can create a cycle etc. cmds like `cp -r` needs a tree, if there's a cycle then can loop forever ` A -> B -> A_hard -> A -> B -> A_hard ...`
travel tools can detect sym-link following.

kernel resolves relative paths using inodes.

`cd -`  - prev working directory not parent.
`cd ~eren` - home dir. of user eren

###### exploring the system
`command -options arguments`

`file` - determine file type
`less` - view file contents
`ls -l` - long 
`ls -lt` - sort by modification time

![[ls.png|400]]

`drwxrwxrwx`
first character indicates file type, `d` for directory `-` for regular file. next 3 characters are access rights for the file's owner, next 3 for members of file's group, next 3 for everyone else.
`1` 
file's number of hard links.
`root` 
username of file's owner
`root`
name of the group that owns the file
`4096`
size of file in bytes
`Dec 3 10:21`
date and time of file's last modification
`contest`
name of the file

**Symbolic links**

![[sym_link.png]]

first letter of the listing is `l`

*symbolic link* - soft link/sym link. 

###### Manipulating Files and Directories
![[wildcards.png|500]]

![[wildclasses.png|500]]

`mkdir dir1 dir2`
`cp item1 item2` - if item2 exists its overwritten else created
`cp items... dir` - copies items to a directory
`-a` - archive all details as it is
`-r` - recursively copy directories
`-u` - update
`-v` - verbose

`mv` - move
`rm` - remove

`ln file link` - hard link
`ln -s file/dir link` - soft link

all files have a name part (inode) and a data part (data blocks).

*hard links* cannot refer a dir and files outside the filesystem. file data exists until all links to data are deleted. creating additional name part that refers to same data part.

*sym links* are files that points to referenced file. is sym links are modified, ref. file also gets modified. if we delete sym link only the link is deleted. if ref. is del then it points to nothing. broken.

each hard link refers to a specific inode containing pointers to file's content.

when we are creating sym links, we are creating a text description of where the target file is relative to the symbolic link.

###### Working with Commands
a command can be :

- executable program
- shell builtin
- shell function
- alias

`type cmd` - display type of cmd
`which cmd` - display an executable's location, only works for executable
`whatis ls` - display one line info 
`man section term`

`cmd1; cmd2; cmd3;`

`alias name='string'`
`unalias name`

###### Redirection
when you type `ls` , shell calls `fork()` to create a child process which then calls `execve()` to load `ls`. after work `ls` calls `exit()`.

`ls` send its result to `stdout` and status messages to `stderr`. both of these special files are linked to the screen and not saved on the disk.
they are streams, they write to terminal by default.

`> hell`
`{ cmd1; cmd2; cmd3; } > log`

`ls -l /bin/usr &> log` - redirect both stdout & stderr

`/dev/null` - null device, discards all data written to it.

`cat` - copies files to stdout

wildcards always expand in sorted order.

`stdin` is default attached to keyboard

`cat > text`

`cmd1 | cmd2`

`sort` `uniq` `uniq -d`

`wc -l -c -b`

`grep pattern file` 
`-i` - ignore case
`-v` - print lines that do not match
`-l` - file names that has pattern
`-w` - match whole words

`head -n -5` - all but last 5 lines
`tail -n +5` - all but first 5 lines
`tail -f log` - monitor files in real time

`tee` - reads from stdin it to stdout and one or more files.

`EOF` is a signal by os not data or character.

pathname expansion - `*` `~`

in fish - `math 5^2 * 3`

`echo Front-{A,B,C}-Back`
`mkdir Number_{1..5}` `{01..15}` `{z..a}`

parameter expansion -

`echo $USER`

command substitution -

`echo $(ls)` `ls $(which cp)`

delete hidden files and not `.` `..`
`rm -rf temp/* temp/.[!.]* temp/..?*`


###### Permissions
`id` - uid,gid,groups

`/etc/passwd` - user info
`/etc/group` - group info

![[linux_perm.png|500]]

symbolic links have dummy permissions

*chmod*

change mode (permissions)

only owner or superuser can use `chmod`

u can use octal or symbol method
`u` - user `g` - group `o` - others `a` - all 
`u+x` `u-x` `+x //same as a+x` `go=rw`

`umask` - set default permissions

![[umask.png|500]]

everywhere a 1 appears in the binary value of the mask, an attribute is unset.

`chown` - change file owner and group

`passwd [user]`

home directory for a user is defined in `passwd` file.

x11 and wayland are display server protocols. they are middle layer between applications and hardware.

kernel resolves relative path step by step.

##### 10 - Processes
when a system starts up, kernel initiates few processes and a program *init* . init starts *systemd* which starts all system services.

*daemon* - programs that sit in background and do their thing without direct user interaction.

init has *pid* 1 . pid's are assinged in ascending order.

*TTY* - teletype, refers to the controlling terminal of a process.
*PTY* - pseudo terminal slave, 

`ps` - snapshot of processes
- `a` all `u` verbose `x` running 

`top` - view processes dynamically, sorted by cpu activity

`top &` `fg %1` `jobs` `bg %1` 

`Ctrl+z TSTP` - stop and put in bg  `Ctrl+c INT`

`nice -n 10 cpu-hog` `-20 to 19`  niceness $\to$ 
`renice`

`nohup firefox`



shell maintains a body of information during our shell session called *environment*.

`printenv` `set` `echo $HOME`

