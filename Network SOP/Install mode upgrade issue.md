# Install Mode Cisco Switch Upgrade Issue
##### During the cisco 9200 install mode switch firmware upgarde activity we have identify the new issue `[1] Switch 1 FAILED: /mnt/sd3/user requires 462703 KB of free space, but only 408260 KB is available)`

```bash
NET17658-LAN-FARNBOROUGH.CE3#install add file flash:cat9k_lite_iosxe.17.09.06a.SPA.bin activate commit
install_add_activate_commit: START Fri Dec 13 05:25:16 GMT 2024
install_add: Adding IMG
 [1] Switch 1 FAILED: /mnt/sd3/user requires 462703 KB of free space, but only 408260 KB is available
FAILED: add_activate_commit /mnt/sd3/user/cat9k_lite_iosxe.17.09.06a.SPA.bin Fri Dec 13 05:25:16 GMT 2024
NET17658-LAN-FARNBOROUGH.CE3#
```

##### Issue was idenified in `#dir flash:` after copied new IOS image into flash we run this command `#clear install state` this was caused to extract the new IOS

```bash
NET17658-LAN-FARNBOROUGH.CE3#dir flash:
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
##### Removed new IOS SPA.pkg files by using below given commands in flash memory then it allowed us to upgarde the switch
```bash

NET17658-LAN-FARNBOROUGH.CE3#delete /force /recursive flash:cat9k_lite_iosxe.17.09.06a.SPA.conf
NET17658-LAN-FARNBOROUGH.CE3#delete /force /recursive flash:cat9k_lite-rpboot.17.09.06a.SPA.pkg
NET17658-LAN-FARNBOROUGH.CE3#delete /force /recursive flash:cat9k_lite-webui.17.09.06a.SPA.pkg
NET17658-LAN-FARNBOROUGH.CE3#delete /force /recursive flash:cat9k_lite-srdriver.17.09.06a.SPA.pkg
NET17658-LAN-FARNBOROUGH.CE3#delete /force /recursive flash:cat9k_lite-rpbase.17.09.06a.SPA.pkg
```
##### Once you delete new version files from flash: check if switch is running with current packages.conf

```bash
NET17658-LAN-FARNBOROUGH.CE3#more flash:packages.conf
#! /usr/binos/bin/packages_conf.sh

sha1sum: 3ba1c29a0bf3cb2fbbf818c8ca6203c2b0c2f60d

sha256sum: 7ae8a8699c927a269ecb7777a35621961e9f5792e93d642cc59cc6ed3331a942

# sha1sum above - used to verify that this file is not corrupted.
#
# package.conf: provisioned software file for build 2024-01-30_15.39
#
# NOTE: Editing this file by hand is not recommended. It is generated
#       as part of the build process, and is subject to boot-time
#       consistency checks. Automatically-produced package files are
#       guaranteed to pass those checks. Manually-maintained ones are
#       not. Because "nfs" and "mount" directives are processed first,
#       regardless of their position in the file, the recommended
#       approach is to keep a separate file containing JUST your
#       personal "nfs" and "mount" directives, and to append it to the
#       automatically-generated file.
#
#     Note further that when SHA-1 checksum verification is enabled,
#     you will NOT be able to alter this file without updating the
#     SHA-1 sum.

#
# This file can contain three types of entries:
#

#
# NFS directives (optional)
#     notes:    NFS directives are processed before all others (mount, iso).
#               Multiple NFS directives may appear so long as they do not
#               conflict -- that is, specify the same source or mountpoint.
#     syntax:   nfs <IP ADDRESS>:<REMOTE_PATH> <LOCAL_MOUNTPOINT>
#     example:  nfs 127.0.0.1:/auto/some/nfs/path /auto/some/nfs/path
#

#
# mount directives (optional)
#     notes:    mount directives are processed after 'nfs' and before 'iso'.
#               One mount directive may appear for each F/S/B/P tuple
#     syntax:   mount FRU SLOT BAY   PACKAGE_NAME   NFS_PATH
#     example:  mount rp 0 0 rp_base /auto/some/nfs/path/abs_soft/rp_base.ppc
#
#               The specified NFS_PATH must
#               reference the NFS mounts created earlier.
#
#               Mount directives cause the package-specific mount link to
#               be set to the specified path instead of to the mountpoint
#               in sw for the corresponding ISO.
#

#
# iso directives (mandatory)
#     notes:    iso directives are processed last: any package for which
#               a 'mount' directive does not appear will be mounted.
#               One iso directive may appear for each F/S/B/P tuple.
#     syntax:   iso FRU SLOT BAY   PACKAGE_NAME  PACKAGE_FILE.bin
#     example:  iso rp 0 0 rp_base rp_base.ppc.bin
#
#               PACKAGE_FILE.bin is a path relative to the packages.conf
#               file.  Although it supports sub-directories for development
#               purposes, in deployment the files will always be managed
#               as in the same directory as packages.conf so as to
#               guarantee that name collisions cannot occur.
#
# Note that the RP 0/1 distinction is a convenience for development
# and testing as it allows us to have a packages.conf describe a
# SW load that varies depending on whether the RP finds itself in
# slot 0 or 1.
#
# The ISSU process *must* update *both* RP slots simultaneously so that
# the RP will behave predictably whichever slot it finds itself on [e.g.,
# if package X is upgraded, and the RP is ejected and put into either
# slot of a new chassis, we expect to see the upgraded X without regard
# to slot].

boot   rp 0 0   rp_boot cat9k_lite-rpboot.17.09.05.SPA.pkg
iso   rp 0 0   rp_base cat9k_lite-rpbase.17.09.05.SPA.pkg
iso   rp 0 0   rp_daemons cat9k_lite-rpbase.17.09.05.SPA.pkg
iso   rp 0 0   rp_iosd cat9k_lite-rpbase.17.09.05.SPA.pkg
iso   rp 0 0   rp_security cat9k_lite-rpbase.17.09.05.SPA.pkg
iso   rp 0 0   rp_webui cat9k_lite-webui.17.09.05.SPA.pkg
iso   rp 0 0   srdriver cat9k_lite-srdriver.17.09.05.SPA.pkg
iso   fp 0 0   fp cat9k_lite-rpbase.17.09.05.SPA.pkg
iso   cc 0 0   cc cat9k_lite-rpbase.17.09.05.SPA.pkg
iso   cc 0 0   cc_srdriver cat9k_lite-srdriver.17.09.05.SPA.pkg

# -start- superpackage .pkginfo
#
# pkginfo: Name: rp_super
# pkginfo: BuildTime: 2024-01-30_15.39
# pkginfo: ReleaseDate: Tue-30-Jan-24-23:00
# pkginfo: .BuildArch: arm32
# pkginfo: BootArchitecture: arm32
# pkginfo: .BootArch: arm32
# pkginfo: RouteProcessor: quake
# pkginfo: Platform: CAT9K_LITE
# pkginfo: User: mcpre
# pkginfo: PackageName: universalk9
# pkginfo: Build: 17.09.05
# pkginfo: .SupportedBoards: quake
# pkginfo: .InstallModel: 
# pkginfo: .PackageRole: rp_super
# pkginfo: .RestartRole: rp_super
# pkginfo: .UnifiedPlatformList: quake1,quake2
# pkginfo: CardTypes: 
# pkginfo: .CardTypes: 
# pkginfo: .BuildPath: /nobackup/mcpre/s2c-build-ws/binos/linkfarm/quake_universalk9-stage/img/rp_super_universalk9
# pkginfo: .Version: 17.09.05.0.6450.1706657994..Cupertino
# pkginfo: .InstallVersion: 1.0.0
# pkginfo: .InstallCapCommitSupport: yes
# pkginfo: .PKGUID: 87e497acfdf1f96849b540e8d6c2b19bf647c220
# -end- superpackage .pkginfo


NET17658-LAN-FARNBOROUGH.CE3#
```

##### Upgrade command

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
##### Post device reboot cisco switch successfully upgrade with new standard version

```bash
NET17658-LAN-FARNBOROUGH.CE3#show version | i Soft
Cisco IOS XE Software, Version 17.09.06a
Cisco IOS Software [Cupertino], Catalyst L3 Switch Software (CAT9K_LITE_IOSXE), Version 17.9.6a, RELEASE SOFTWARE (fc1)
NET17658-LAN-FARNBOROUGH.CE3#
```
##### For more information follow this cisco link
[Link-1](https://community.cisco.com/t5/switching/cisco-catalyst-9200-down-grade-problem/td-p/4955340)
[Link-2](https://community.cisco.com/t5/switching/cisco-9200-ios-out-of-space/td-p/4161212)

