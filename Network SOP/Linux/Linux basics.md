# Linux basics
### Inportant commands
```bash
cat /etc/os-release (or)  lsb_release -a  (To see the current version of linux)
cd    (to change the fdirectory)
pwd
ls    (to see the inside created directory)
cp    (copy)
mv    (used for move & rename the file)
rm    (remove)
echo  (comment)
less 
grep  (filter)
mkdir (create a Directory)
touch (create a file)
nano  (edit the files)
cat   (to see the configuration inside that file)
chmod (to give permissions) (chmod +x)
vim   (text editor)
./    (Used for exicute the files)
#!/bin/bash  (The line #!/bin/bash is called a shebang or hashbang. It specifies the interpreter that should be used to execute the script.)
rmdir (used for remove directory)
man + help
```

## Linux version details

```bash
root@DESKTOP-V421QNR:~# cat /etc/os-release
PRETTY_NAME="Ubuntu 24.04.1 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.1 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
root@DESKTOP-V421QNR:~#
```
[or]

```bash
root@DESKTOP-V421QNR:~# lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.1 LTS
Release:        24.04
Codename:       noble
root@DESKTOP-V421QNR:~#
```

## Create a new folder in Linux

```bash
root@DESKTOP-V421QNR:~#mkdir cisco_commands
root@DESKTOP-V421QNR:~# ls
cisco_commands
```
## Create folder inside `cisco_commands`

```bash
root@DESKTOP-V421QNR:~# cd cisco_commands/
root@DESKTOP-V421QNR:~/cisco_commands# touch switch_commands.txt
root@DESKTOP-V421QNR:~/cisco_commands# ls
switch_commands.txt
```

## Inside `switch_commands.txt` if you want to add some information

```bash
root@DESKTOP-V421QNR:~/cisco_commands# nano switch_commands.txt
root@DESKTOP-V421QNR:~/cisco_commands# (Once you enter data to save this follow below commands)
Once you finish, enter esc button :wq hit the enter button to save it
or
Once you finish, press Ctrl + O to save the file.
Press Enter to confirm the filename.
Press Ctrl + X to exit the editor.
```
## To see the added data to `switch_commands.txt`

```bash
root@DESKTOP-V421QNR:~/cisco_commands# cat switch_commands.txt
terminal length 0
sh run
sh switch details
sh ver | in Sof
dir flash:
```

## If you to exit from folder

```bash
root@DESKTOP-V421QNR:~/cisco_commands# cd ..
root@DESKTOP-V421QNR:~#
```

## If you want to copy `.txt` file from one folder to another folder

```bash
root@DESKTOP-V421QNR:~# cp cisco_commands/switch_commands.txt router_commands
root@DESKTOP-V421QNR:~#
root@DESKTOP-V421QNR:~# ls
cisco_commands  router_commands

root@DESKTOP-V421QNR:~/router_commands# cat switch_commands.txt
terminal length 0
sh run
sh switch details
sh ver | in Sof
dir flash:
root@DESKTOP-V421QNR:~/router_commands#
```

## If you want to remove the `Folder or txt` files

```bash
root@DESKTOP-V421QNR:~/router_commands# rm switch_commands.txt
root@DESKTOP-V421QNR:~/router_commands#

root@DESKTOP-V421QNR:~/router_commands# ls
root@DESKTOP-V421QNR:~/router_commands#
```


