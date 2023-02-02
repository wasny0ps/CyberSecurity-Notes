
# Linux Directory Structure
In Linux/Unix operating system everything is a file even directories are files, files are files, and devices like mouse, keyboard, printer, etc are also files. Here we are going to see the Directory Structure in Linux.

### Types of files in the Linux system

- **General Files** : It is also called **ordinary files**. It may be an **image, video, program, or simple text files**. These types of files can be in ASCII or Binary format. It is the **most commonly used file** in the Linux system.
* **Directory Files** : These types of files are a warehouse for other file types. It may be a directory file within a subdirectory.
+ **Device Files** : In a Windows-like operating system, devices like CD-ROM, and hard drives are represented as drive letters like F: G: H whereas in the Linux system device are **represented as files**. As for example, /dev/sda1, /dev/sda2 and so on.

In the Linux/Unix operating system files are stored in a tree-like structure starting with the **root directory** as shown in the below diagram. **The base of the Linux/Unix file system hierarchy begins at the root** and everything starts with the root directory. 

|Directory|Content|
|:---:|:---|
|**/bin**|Binary or executable programs.|
|**/etc**|System configuration files.|
|**/home**|It is the default current directory.|
|**/opt**|Optional or third-party software.|
|**/tmp**|Temporary space, typically cleared on reboot.|
|**/usr**|User related programs.|
|**/var**|Log files.|
|**/boot**|It contains all the boot-related information files and folders such as conf, grub.|
|**/dev**|It is the location of the device files such as dev/sda1.|
|**/lib**|It contains kernel modules and a shared library.|
|**/lost+found**|It is used to find recovered bits of corrupted files.|
|**/media**|It contains subdirectories where removal media devices inserted.|
|**/mnt**|It contains temporary mount directories for mounting the file system.|
|**/proc**|It is a virtual and pseudo-file system to contains info about the running processes with a specific process ID or PID.|
|**/run**|It stores volatile runtime data.|
|**/sbin**|Binary executable programs for an administrator.|
|**/srv**|It contains server-specific and server-related files.|
|**/sys**|It is a virtual filesystem for modern Linux distributions to store and allows modification of the devices connected to the system.|



