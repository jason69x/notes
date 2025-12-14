
###### What Is the Shell?
the *shell* is a program that takes keyboard commands and passes them to the operating system to carry out.

*terminal emulators* give us access to the shell. eg- konsole.

*virtual consoles/terminals*, `ctrl-alt-F1:6` `alt-F1:6` `alt-F7`


###### Navigation
`pwd` - print working directory
`cd` - change directory
`ls` - list directory content

linux organizes its files in a *heirarchical directory structure*.

unline windows,which has a separate file system tree for each storage device, linux always have a single file system tree.

`.` - refers to working directory, same inode as the directory
`..` - refers to parent of working directory , same inode as parent

these are ***special directory entries***. they are ***hard links***.
created automatically by the filesystem when a directory is created.
every directory has them.
a directory is essentially a table : `filename -> inode`

we can't create hardlinks to directories coz they can create a cycle etc.

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


