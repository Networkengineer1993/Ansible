# Linux basics
### Inportant commands
```bash
cd
pwd
ls
cp
mv
rm
echo
cat
less
grep
mkdir
touch
chmod
man + help
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
root@DESKTOP-V421QNR:~/cisco_commands# nano switch_commands.txt
root@DESKTOP-V421QNR:~/cisco_commands# ls
switch_commands.txt
```

## Inside `switch_commands.txt` if you want to add some information

```bash
root@DESKTOP-V421QNR:~/cisco_commands# nano switch_commands.txt
root@DESKTOP-V421QNR:~/cisco_commands# (Once you enter data to save this follow below commands)

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


