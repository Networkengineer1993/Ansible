# Install Mode Cisco Switch Upgrade Issue
During the cisco 9200 install mode switch firmware upgarde activity we have identify the new issue "[1] Switch 1 FAILED: /mnt/sd3/user requires 462703 KB of free space, but only 408260 KB is available"

```bash
NET17658-LAN-FARNBOROUGH.CE3#install add file flash:cat9k_lite_iosxe.17.09.06a.SPA.bin activate commit
install_add_activate_commit: START Fri Dec 13 05:25:16 GMT 2024
install_add: Adding IMG
 [1] Switch 1 FAILED: /mnt/sd3/user requires 462703 KB of free space, but only 408260 KB is available
FAILED: add_activate_commit /mnt/sd3/user/cat9k_lite_iosxe.17.09.06a.SPA.bin Fri Dec 13 05:25:16 GMT 2024
NET17658-LAN-FARNBOROUGH.CE3#
```

Issue was idenified in #dir flash:

```bash
NET17658-LAN-FARNBOROUGH.CE3#dir 
Directory of flash:/

40510   -rw-          2097152  Dec 13 2024 05:29:28 +00:00  nvram_config_bkup
40529   -rw-             4918  Dec 13 2024 05:17:47 +00:00  cat9k_lite_iosxe.17.09.06a.SPA.conf
72874   -rw-         44284647  Dec 13 2024 05:17:43 +00:00  cat9k_lite-rpboot.17.09.06a.SPA.pkg
72873   -rw-         14876672  Dec 13 2024 05:16:04 +00:00  cat9k_lite-webui.17.09.06a.SPA.pkg
72872   -rw-          5984256  Dec 13 2024 05:16:03 +00:00  cat9k_lite-srdriver.17.09.06a.SPA.pkg
72871   -rw-        409161728  Dec 13 2024 05:16:03 +00:00  cat9k_lite-rpbase.17.09.06a.SPA.pkg
72865   drwx             4096  Dec 13 2024 05:13:34 +00:00  .images
40518   -rw-        473808711  Dec 13 2024 04:47:57 +00:00  cat9k_lite_iosxe.17.09.06a.SPA.bin
40485   -rw-             2133  Jun 27 2024 05:15:37 +01:00  boothelper.log.old
40548   -rw-             1924  Jun 27 2024 05:12:22 +01:00  ISRGRootX1CrossSignDST.ca
40536   drwx             4096  Jun 27 2024 05:12:13 +01:00  EDScisco
97156   drwx             4096  Jun 27 2024 05:12:13 +01:00  tech_support
80962   drwx             4096  Jun 27 2024 05:10:04 +01:00  .rollback_timer
40560   -rw-             4908  Jun 27 2024 05:08:57 +01:00  packages.conf
40552   -rw-             4908  Jun 27 2024 05:01:21 +01:00  cat9k_lite_iosxe.17.09.05.SPA.conf
40558   -rw-         44267403  Jun 27 2024 05:01:21 +01:00  cat9k_lite-rpboot.17.09.05.SPA.pkg
40556   -rw-         14876672  Jun 27 2024 04:59:49 +01:00  cat9k_lite-webui.17.09.05.SPA.pkg
40554   -rw-          5980160  Jun 27 2024 04:59:48 +01:00  cat9k_lite-srdriver.17.09.05.SPA.pkg
40553   -rw-        409178112  Jun 27 2024 04:59:47 +01:00  cat9k_lite-rpbase.17.09.05.SPA.pkg
40544   drwx             4096  Mar 21 2023 05:33:08 +00:00  pd_info

1956839424 bytes total (355348480 bytes free)
NET17658-LAN-FARNBOROUGH.CE3#
```
Removed unwated files from flash memory then it allowed us to upgarde the switch
```bash

NET17658-LAN-FARNBOROUGH.CE3#delete /force /recursive flash:cat9k_lite_iosxe.17.09.06a.SPA.conf
NET17658-LAN-FARNBOROUGH.CE3#delete /force /recursive flash:cat9k_lite-rpboot.17.09.06a.SPA.pkg
NET17658-LAN-FARNBOROUGH.CE3#delete /force /recursive flash:cat9k_lite-webui.17.09.06a.SPA.pkg
NET17658-LAN-FARNBOROUGH.CE3#delete /force /recursive flash:cat9k_lite-srdriver.17.09.06a.SPA.pkg
NET17658-LAN-FARNBOROUGH.CE3#delete /force /recursive flash:cat9k_lite-rpbase.17.09.06a.SPA.pkg
```

Upgrade command

```bash
NET17658-LAN-FARNBOROUGH.CE3#install add file flash:cat9k_lite_iosxe.17.09.06a.SPA.bin activate commit
install_add_activate_commit: START Fri Dec 13 05:45:36 GMT 2024
install_add: Adding IMG
--- Starting initial file syncing ---
Copying flash:cat9k_lite_iosxe.17.09.06a.SPA.bin from Switch 1 to Switch 1
Info: Finished copying to the selected Switch
Finished initial file syncing

--- Starting Add ---
Performing Add on all members
 [1] Finished Add package(s) on Switch 1
Checking status of Add on [1]
Add: Passed on [1]
Finished Add

Image added. Version: 17.09.06a.0.12

install_activate: Activating IMG
Following packages shall be activated:
/flash/cat9k_lite-rpbase.17.09.06a.SPA.pkg
/flash/cat9k_lite-rpboot.17.09.06a.SPA.pkg
/flash/cat9k_lite-srdriver.17.09.06a.SPA.pkg
/flash/cat9k_lite-webui.17.09.06a.SPA.pkg
Following packages shall be activated:
/flash/cat9k_lite-rpbase.17.09.06a.SPA.pkg
/flash/cat9k_lite-rpboot.17.09.06a.SPA.pkg
/flash/cat9k_lite-srdriver.17.09.06a.SPA.pkg
/flash/cat9k_lite-webui.17.09.06a.SPA.pkg

This operation may require a reload of the system. Do you want to proceed? [y/n]y

--- Starting Activate ---
Performing Activate on all members
 [1] Activate package(s) on Switch 1
 [1] Finished Activate on Switch 1
Checking status of Activate on [1]
Activate: Passed on [1]
Finished Activate

--- Starting Commit ---
Performing Commit on all members
 [1] Commit package(s) on Switch 1
 [1] Finished Commit on Switch 1
Checking status of Commit on [1]
Commit: Passed on [1]
Finished Commit operation

SUCCESS: install_add_activate_commit Fri Dec 13 05:54:19 GMT 2024
NET17658-LAN-FARNBOROUGH.CE3#client_loop: send disconnect: Broken pipe
ell1-jumpbox:
```
Post device reboot cisco switch successfully upgrade with new standard version

```bash
NET17658-LAN-FARNBOROUGH.CE3#show version | i Soft
Cisco IOS XE Software, Version 17.09.06a
Cisco IOS Software [Cupertino], Catalyst L3 Switch Software (CAT9K_LITE_IOSXE), Version 17.9.6a, RELEASE SOFTWARE (fc1)
NET17658-LAN-FARNBOROUGH.CE3#
```

