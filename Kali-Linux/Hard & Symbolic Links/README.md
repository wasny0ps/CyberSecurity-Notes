
# Hard & Symbolic Link

In Linux systems; There are two types of links, symbolic and hard. If we explain them in order;
Links created with symbolic link; it acts as a shortcut of the files and its task is to redirect only to the relevant file.
Links created with a solid link are a copy of the file. Even if the original file is deleted, the solid link content continues to be preserved.
Before moving on to the use of connection types, we need to learn a little more information, which is the concept of inodes.

## inode

**The inode is a database that describes the file/directory attributes such as metadata and the physical location on the hard drive.** They are essentially the numerical equivalent of a full address. With an inode, the OS can retrieve information about the file such as permission privileges and the physical location of the data on the hard drive to access the file. Should a file be moved from one folder to another, the file will be moved to a different location on the hard drive and its inode value will change with it automatically. This will be important for hard links.





## Hard Link

A hard link is a direct reference to a file via its inode. You can also only hardlink files and not directories. By using a hardlink, you can change the original file’s contents or location and the hardlink will still point to the original file because its inode is still pointing to that file. There is no referencing to the original file. In addition, hardlinks can only refer to files within the same volume otherwise symbolic links will be needed. To make a hard link of a file, you will require the ln command and refer to the source file before naming what the hard link will be named.

```shell
┌──(root㉿kali)-[/home/kali/Documents/github]
└─# nano hello.txt
 ```
 ```shell
┌──(root㉿kali)-[/home/kali/Documents/github]
└─# ln hello.txt hard_link.txt
```
```shell
┌──(root㉿kali)-[/home/kali/Documents/github]
└─# ls    
hard_link.txt  hello.txt
```

## Symbolic  Link

Symbolic links are essentially shortcuts that reference to a file instead of its inode value. This method can be applied to directories and can reference across different hard disks/volumes. Since the symbolic link is referring to the original file and not its inode value, then replacing the original file into a different folder will break the symbolic link, or create a dangling link.

What will break a symbolic link is when the original file is moved to a different file or deleted. So symbolic links can be seen as a static link to the last known location of the original file. The link should work even if you replace the original file with a different file with the same name.

```shel
┌──(root㉿kali)-[/home/kali/Documents/github]
└─# ln -s hello.txt symbolic_link.txt
```
```shell
┌──(root㉿kali)-[/home/kali/Documents/github]
└─# ls                               
hard_link.txt  hello.txt  symbolic_link.txt

```

**_by wasny0ps**
