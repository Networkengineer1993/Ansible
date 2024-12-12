# Aggrigator Circuit Decommission Process

## Information Defore decommission

```bash
NET18737-CHESHIRE.CE1
WO: 12561275
Circuit ID: CAL0325606
VLANs: Te0/0/0/21.22942, Te0/0/0/21.2294300
PE Device: UK-LN10-AGG2
```
## Utilization Report
![image](https://github.com/user-attachments/assets/d63397b1-b35f-4a7c-8b68-d43471be2ee7)


## Router Information

```bash
RP/0/RSP0/CPU0:UK-LN10-AGG2#sh int desc | in NET18737-CHESHIRE.CE1
Mon Dec  2 07:36:23.696 GMT
Te0/0/0/21.22942   up      up     OPTEGRA UK LTD:NET18737-CHESHIRE.CE1:Virgin:PRIMARY:LES1000:CAL0325606,1163087-1996722,CAL0325606:MGMNT
Te0/0/0/21.2294300 up      up     OPTEGRA UK LTD:NET18737-CHESHIRE.CE1:Virgin:PRIMARY:LES1000:CAL0325606,1163087-1996722,CAL0325606:VPN
```

## Router Configuration MGMT

```bash
RP/0/RSP0/CPU0:UK-LN10-AGG2#sh run int Te0/0/0/21.22942
Mon Dec  2 07:36:30.365 GMT
interface TenGigE0/0/0/21.22942
 description OPTEGRA UK LTD:NET18737-CHESHIRE.CE1:Virgin:PRIMARY:LES1000:CAL0325606,1163087-1996722,CAL0325606:MGMNT
 service-policy output 100M_SHAPE_10M_VOIP shared-policy-instance NET18737-CHESHIRE.CE1
 vrf MGMNT_CONSOLIDATION_1
 ipv4 address 10.162.173.249 255.255.255.252
 encapsulation dot1q 2294 second-dot1q 2
!

RP/0/RSP0/CPU0:UK-LN10-AGG2#sh run vrf MGMNT_CONSOLIDATION_1
Mon Dec  2 07:37:57.067 GMT
vrf MGMNT_CONSOLIDATION_1
 address-family ipv4 unicast
  import route-target
   13009:120
  !
  export route-policy MGMNT-EX
 !
!

RP/0/RSP0/CPU0:UK-LN10-AGG2#sh run router bgp 13009 vrf MGMNT_CONSOLIDATION_1 neighbor 10.162.173.250
Mon Dec  2 07:38:10.389 GMT
router bgp 13009
 vrf MGMNT_CONSOLIDATION_1
  neighbor 10.162.173.250
   remote-as 64933
   description OPTEGRA UK LTD:NET18737-CHESHIRE.CE1:Virgin:PRIMARY:LES1000:CAL0325606,1163087-1996722,CAL0325606:MGMNT
   update-source TenGigE0/0/0/21.22942
   address-family ipv4 unicast
    send-community-ebgp
    route-policy PREFHIGH in
    route-policy DROP_ALL_ROUTES out
    as-override
    default-originate route-policy PRIMARY
   !
  !
 !
!

RP/0/RSP0/CPU0:UK-LN10-AGG2#sh bgp vrf MGMNT_CONSOLIDATION_1 summary | in 10.162.173.250
Mon Dec  2 07:38:22.174 GMT
10.162.173.250    0 64933 6879381 4686304        0    0    0    16w2d Active

RP/0/RSP0/CPU0:UK-LN10-AGG2#sh arp vrf MGMNT_CONSOLIDATION_1 | in 10.162.173.250
Mon Dec  2 07:38:31.859 GMT
RP/0/RSP0/CPU0:UK-LN10-AGG2#
```

## Router Configuration VPN
```bash
RP/0/RSP0/CPU0:UK-LN10-AGG2#sh run int Te0/0/0/21.2294300
Mon Dec  2 07:39:09.021 GMT
interface TenGigE0/0/0/21.2294300
 description OPTEGRA UK LTD:NET18737-CHESHIRE.CE1:Virgin:PRIMARY:LES1000:CAL0325606,1163087-1996722,CAL0325606:VPN
 service-policy output 100M_SHAPE_10M_VOIP shared-policy-instance NET18737-CHESHIRE.CE1
 vrf IPSA_13009:2261526
 ipv4 address 10.162.173.253 255.255.255.252
 encapsulation dot1q 2294 second-dot1q 300
!

RP/0/RSP0/CPU0:UK-LN10-AGG2#sh run vrf IPSA_13009:2261526
Mon Dec  2 07:39:15.598 GMT
vrf IPSA_13009:2261526
 address-family ipv4 unicast
  import route-target
   13009:1891302
  !
  export route-target
   13009:1891302
  !
 !
 address-family ipv6 unicast
  import route-target
   13009:1891302
  !
  export route-target
   13009:1891302
  !
 !
!

RP/0/RSP0/CPU0:UK-LN10-AGG2#sh run router bgp 13009 vrf IPSA_13009:2261526 neighbor 10.162.173.254
Mon Dec  2 07:39:27.796 GMT
router bgp 13009
 vrf IPSA_13009:2261526
  neighbor 10.162.173.254
   remote-as 64933
   description OPTEGRA UK LTD:NET18737-CHESHIRE.CE1:Virgin:PRIMARY:LES1000:CAL0325606,1163087-1996722,CAL0325606:VPN
   update-source TenGigE0/0/0/21.2294300
   address-family ipv4 unicast
    send-community-ebgp
    route-policy PREFHIGH in
    route-policy DROP_ALL_ROUTES out
    as-override
    default-originate route-policy PRIMARY
   !
  !
 !
!

RP/0/RSP0/CPU0:UK-LN10-AGG2#sh bgp vrf IPSA_13009:2261526 summary | in 10.162.173.254
Mon Dec  2 07:39:39.253 GMT
10.162.173.254    0 64933 6871335 4678295        0    0    0    16w2d Idle

RP/0/RSP0/CPU0:UK-LN10-AGG2#sh arp vrf IPSA_13009:2261526
Mon Dec  2 07:39:45.742 GMT

-------------------------------------------------------------------------------
0/0/CPU0
-------------------------------------------------------------------------------
Address         Age        Hardware Addr   State      Type  Interface
10.162.173.253  -          683b.78c4.b495  Interface  ARPA  TenGigE0/0/0/21.2294300
RP/0/RSP0/CPU0:UK-LN10-AGG2#
```
## Main Decommission Configuration

```bash
UK-LN10-AGG2#
no int Te0/0/0/21.22942
no int Te0/0/0/21.2294300
```

## IPSA Screenshots

![image](https://github.com/user-attachments/assets/2c11f575-93ed-4dfa-a80e-b2656378d476)



