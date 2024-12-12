# Aggrigator Decommission Process

## Information Defore decommission

```bash
NET17097-ELBURYMOOR.CE1
WO: 17137771
Circuit ID: V1C36243
VLANs: Vl632, Vl633
PE Device: UK-LN5-GW1
```
## Utilization Report
![image](https://github.com/user-attachments/assets/a6f11886-572a-4bc6-ba3b-ee64d0ccab4c)

![image](https://github.com/user-attachments/assets/ba069af5-dfbb-479f-9c24-c79759b645e2)

## Router Information

```bash
UK-LN5-GW1#sh int desc | in NET17097-ELBURYMOOR.CE1
Vl632      up       up       WORCESTERSHIRE HEALT:NET17097-ELBURYMOOR.CE1:Virtual-1:PRIMARY:LES100:V1C36243,V1C36243,TTB0848529:MGMNT
Vl633      up       up       WORCESTERSHIRE HEALT:NET17097-ELBURYMOOR.CE1:Virtual-1:PRIMARY:LES100:V1C36243,V1C36243,TTB0848529:VPN
```

## Router Configuration VPN

```bash
UK-LN5-GW1#sh run int Vl633
Building configuration...

Current configuration : 222 bytes
!
interface Vlan633
 description WORCESTERSHIRE HEALT:NET17097-ELBURYMOOR.CE1:Virtual-1:PRIMARY:LES100:V1C36243,V1C36243,TTB0848529:VPN
 ip vrf forwarding IPSA_13009:2060538
 ip address 10.162.223.205 255.255.255.252
end

UK-LN5-GW1#sh run vrf IPSA_13009:2060538
Building configuration...

Current configuration : 1052 bytes
ip vrf IPSA_13009:2060538
 rd 13009:2060538
 route-target export 13009:1832952
 route-target import 13009:1832952
!
!
interface Vlan633
 description WORCESTERSHIRE HEALT:NET17097-ELBURYMOOR.CE1:Virtual-1:PRIMARY:LES100:V1C36243,V1C36243,TTB0848529:VPN
 ip vrf forwarding IPSA_13009:2060538
 ip address 10.162.223.205 255.255.255.252
!
router bgp 13009
 !
 address-family ipv4 vrf IPSA_13009:2060538
  no synchronization
  neighbor 10.162.223.206 remote-as 64874
  neighbor 10.162.223.206 description WORCESTERSHIRE HEALT:NET17097-ELBURYMOOR.CE1::PRIMARY:LES100:2028:V1C36243:V1C36243:TTB0848529:VPN
  neighbor 10.162.223.206 update-source Vlan633
  neighbor 10.162.223.206 activate
  neighbor 10.162.223.206 send-community
  neighbor 10.162.223.206 default-originate route-map PRIMARY
  neighbor 10.162.223.206 as-override
  neighbor 10.162.223.206 soft-reconfiguration inbound
  neighbor 10.162.223.206 prefix-list DEFAULT out
  neighbor 10.162.223.206 route-map PREFHIGH in
  neighbor 10.162.223.206 route-map PRIMARY out
 exit-address-family
!
end

UK-LN5-GW1#sh ip bgp vpnv4 vrf IPSA_13009:2060538 summ | in 10.162.223.206
10.162.223.206  4        64874       0       0        1    0    0 2w0d     Active

UK-LN5-GW1#sh arp vrf IPSA_13009:2060538
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.162.223.205          -   001b.0de6.2a80  ARPA   Vlan633
Internet  10.162.223.206          0   Incomplete      ARPA
UK-LN5-GW1#
```
## L2 Infromation

```bash
UK-LN5-GW1#sh vlan id 632

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
632  NET17097-ELBURYMOOR.CE1_MGMNT    active    Gi9/11 efp_id 632

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
632  enet  100632     1500  -      -      -        -    -        0      0

Remote SPAN VLAN
----------------
Disabled

Primary Secondary Type              Ports
------- --------- ----------------- ------------------------------------------

UK-LN5-GW1#sh vlan id 633

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
633  NET17097-ELBURYMOOR.CE1_VPN      active    Gi9/11 efp_id 633

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
633  enet  100633     1500  -      -      -        -    -        0      0

Remote SPAN VLAN
----------------
Disabled

Primary Secondary Type              Ports
------- --------- ----------------- ------------------------------------------

UK-LN5-GW1#
```

## Service Interface Configuration

```bash
UK-LN5-GW1#sh run partition int Gi9/11 | b 632
 service instance 632 ethernet
  description WORCESTERSHIRE HEALT:NET17097-ELBURYMOOR.CE1:Virtual-1:PRIMARY:LES100:V1C36243,V1C36243,TTB0848529:MGMNT
  encapsulation dot1q 2028 second-dot1q 2
  rewrite ingress tag pop 2 symmetric
  group 4119
  bridge-domain 632
 !
 service instance 633 ethernet
  description WORCESTERSHIRE HEALT:NET17097-ELBURYMOOR.CE1:Virtual-1:PRIMARY:LES100:V1C36243,V1C36243,TTB0848529:VPN
  encapsulation dot1q 2028 second-dot1q 200
  rewrite ingress tag pop 2 symmetric
  group 4119
  bridge-domain 633
 !

UK-LN5-GW1#sh run service-group 4119
Building configuration...

Current configuration:
service-group 4119
 description NET17097-ELBURYMOOR.CE1_30M_SHAPE
 service-policy output 30M_SHAPE
end

UK-LN5-GW1#sh service-group 4119 detail
Service Group 4119:
  Description: NET17097-ELBURYMOOR.CE1_30M_SHAPE
  Number of members:                      2
      Service Instance                    2
  State:                                  Up
  Features configured:                    QoS
  Input service policy:
  Output service policy:                  30M_SHAPE
  Number of Interfaces:                   1
  Interface:                              GigabitEthernet9/11
    Number of members:                    2
    Service Instance ID:
       632
       633
```

## Main Decommission Configuration

```bash
UK-LN5-GW1#
Int Gi9/11
no service instance 632
no service instance 633
exit

no service-group 4119
no int Vl632
no int Vl633

vlan 632
no name
vlan 633
no name
end
```

## IPSA Screenshots

![image](https://github.com/user-attachments/assets/75fd41c3-a6e0-4903-93b9-614e64d085dd)


