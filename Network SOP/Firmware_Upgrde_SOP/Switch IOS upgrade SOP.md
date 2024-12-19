# Cisco switch IOS upgrade SOP

#### Pre-checks before upgrade cisco switch firmware
```bash
Terminal length 0
show vtp status
show run
show ver | i Software|Process|CISC
show processes cpu sorted | i minutes
show processes memory sorted | i Total
show flash:
show run | se user
show interfaces status
show ip int brief
show cdp nei
show switch
show switch detail
show switch stack-ports
show switch stack-ring speed
show mac address-table dynamic
```

# Step-1 
#### Once you login into install mode switch primary enter this comamnds to remove unwanted files from cisco switch flash
```bash
#Install remove inactive – Will remove any unused files (Install mode switches)
#Software clean flash: - Will remove any unused files (Bundle mode switches)
```

# Step-2
#### Run the "service internal (Global mode)" and "clear install state (User mode)" commands to free up the booting loops in cisco install mode switches

```bash
Switch(config)#service internal
Switch(config)#exit
Switch#clear install state

Note - This commands required switch reboot
```

# Step-3
#### Once switch is back online check the flash available space
```bash
#sh flash: | in bytes
#dir flash:
```

# Step-4
#### Copy the new IOS to switch flash:

```bash
copy ftp://anonymous:anonymous@10.254.224.125/ios_images/cisco/cat9k_iosxe.17.09.05.SPA.bin flash:

NET17658-LAN-FARNBOROUGH.CE7#sh flash: | in SPA
160  473808711 Dec 13 2024 04:56:47.0000000000 +00:00 cat9k_lite_iosxe.17.09.06a.SPA.bin

Note - If the switch has two or more member switches them copy the new IOS from master switch to member switches

copy flash:cat9k_lite_iosxe.17.06.05.SPA.bin flash-2: 
```

# Step-5
#### Once new IOS copied to flash run the MD5 key checks

```bash
#ver /md5 cat9k_lite_iosxe.17.09.06a.SPA.bin
...........................................................................
................................................Done!
verify /md5 (flash:cat9k_lite_iosxe.17.09.06a.SPA.bin) = 2f12e313fced39906499474d0c379e79
```

# Step-6
#### Set the boot set up

```bash
#install add file flash:cat9k_lite_iosxe.17.09.06a.SPA.bin activate commit

Note - Before you set the boot set up make sure you have checked boot variable, if boot variable is not configured switch will be stuck in rommon mode

NET17658-LAN-FARNBOROUGH.CE7#sh boot
---------------------------
Switch 1
---------------------------
Current Boot Variables:
BOOT variable does not exist

Boot Variables on next reload:
BOOT variable = flash:packages.conf;
Manual Boot = no
Enable Break = no
Boot Mode = DEVICE
iPXE Timeout = 0
NET17658-LAN-FARNBOROUGH.CE7#
```

# Step-7
#### Once switch is back online verify the version

```bash
NET17658-LAN-FARNBOROUGH.CE7#sh ver | in Sof
Cisco IOS XE Software, Version 17.09.06a
Cisco IOS Software [Cupertino], Catalyst L3 Switch Software (CAT9K_LITE_IOSXE), Version 17.9.6a, RELEASE SOFTWARE (fc1)
````
# Step-8
#### Post checks atfer IOS upgradation

```bash
Terminal length 0
show vtp status
show run
show ver | i Software|Process|CISC
show processes cpu sorted | i minutes
show processes memory sorted | i Total
show flash:
show run | se user
show interfaces status
show ip int brief
show cdp nei
show switch
show switch detail
show switch stack-ports
show switch stack-ring speed
show mac address-table dynamic
````

