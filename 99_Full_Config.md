# 99. Full Initial Config (R1 ~ R5)

## 🔧 R1

```cisco
en
conf t
!
no ip domain-lookup
!
enable secret cisco
!
hostname R1
!
line con 0
 logging sync
 exec-timeout 0 0
 password cisco
 login
!
line vty 0 4
 password cisco
 login
!
interface loopback 0 
 ip add 13.13.1.1 255.255.255.0
!
interface fastethernet 0/0
 ip add 150.1.13.1 255.255.255.0
 no shutdown
!
interface fastethernet 0/1
 ip add 13.13.11.1 255.255.255.0
 no shutdown
!
interface serial 1/0
 no shutdown
 encapsulation frame-relay
 no frame-relay inverse-arp
!
interface serial 1/0.123 multipoint
 ip add 13.13.9.1 255.255.255.0
 frame-relay map ip 13.13.9.2 102 broadcast
 frame-relay map ip 13.13.9.3 102 broadcast
!
interface serial 1/1
 ip add 13.13.10.1 255.255.255.0
 encapsulation frame-relay
 no frame-relay inverse-arp
 frame-relay map ip 13.13.10.3 113 broadcast
 no shutdown
 end
!
```

## 🔧 R2

```cisco
en
conf t
!
no ip domain-lookup
!
enable secret cisco
!
hostname R2
!
line con 0
 logging sync
 exec-timeout 0 0
 password cisco
 login
!
line vty 0 4
 password cisco
 login
!
interface loopback 0 
 ip add 13.13.2.2 255.255.255.0
!
interface fastethernet 0/1
 ip add 13.13.12.2 255.255.255.0
 no shutdown
!
interface serial 1/0
 encapsulation frame-relay
 no frame-relay inverse-arp
 no shutdown
!
interface serial 1/0.123 multipoint
 ip add 13.13.9.2 255.255.255.0
 frame-relay map ip 13.13.9.1 201 broadcast
 frame-relay map ip 13.13.9.3 203 broadcast
 end
!
```

## 🔧 R3

```cisco
en
conf t
!
no ip domain-lookup
!
enable secret cisco
!
hostname R3
!
line con 0
 logging sync
 exec-timeout 0 0
 password cisco
 login
!
line vty 0 4
 password cisco
 login
!
interface loopback 0 
 ip add 13.13.3.3 255.255.255.0
!
interface fastethernet 0/0
 ip add 150.3.13.3 255.255.255.0
 no shutdown
!
interface fastethernet 0/1
 ip add 13.13.13.3 255.255.255.0
 no shutdown
!
interface serial 1/0
 no shutdown
 encapsulation frame-relay
 no frame-relay inverse-arp
!
interface serial 1/0.123 multipoint
 ip add 13.13.9.3 255.255.255.0
 frame-relay map ip 13.13.9.2 302 broadcast
 frame-relay map ip 13.13.9.1 302 broadcast
!
interface serial 1/1
 ip add 13.13.10.3 255.255.255.0
 encapsulation frame-relay
 no frame-relay inverse-arp
 frame-relay map ip 13.13.10.1 311 broadcast
 no shutdown
 end
!
```

## 🔧 R4

```cisco
en
conf t
!
no ip domain-lookup
!
enable secret cisco
!
hostname R4
!
line con 0
 logging sync
 exec-timeout 0 0
 password cisco
 login
!
line vty 0 4
 password cisco
 login
!
interface loopback 0 
 ip add 13.13.4.4 255.255.255.0
!
interface loopback 150
 ip address 150.100.1.4 255.255.255.0
!
interface loopback 10
 ip address 10.1.0.4  255.255.255.0
 ip address 10.1.1.4  255.255.255.0 secondary
 ip address 10.1.2.4  255.255.255.0 secondary
 ip address 10.1.3.4  255.255.255.0 secondary
!
interface loopback 172
 ip address 172.16.4.4  255.255.255.0
 ip address 172.16.5.4  255.255.255.0 secondary
 ip address 172.16.6.4  255.255.255.0 secondary
 ip address 172.16.7.4  255.255.255.0 secondary
!
interface loopback 192
 ip address 192.168.8.4  255.255.255.0
 ip address 192.168.9.4  255.255.255.0 secondary
 ip address 192.168.10.4  255.255.255.0 secondary
 ip address 192.168.11.4  255.255.255.0 secondary
!
interface loopback 199
 ip address 199.171.1.4 255.255.255.0
 ip address 199.171.2.4 255.255.255.0 secondary
 ip address 199.171.3.4 255.255.255.0 secondary
 ip address 199.171.4.4 255.255.255.0 secondary
 ip address 199.171.5.4 255.255.255.0 secondary
 ip address 199.171.6.4 255.255.255.0 secondary
 ip address 199.171.7.4 255.255.255.0 secondary
 ip address 199.171.8.4 255.255.255.0 secondary
 ip address 199.171.9.4 255.255.255.0 secondary
 ip address 199.171.10.4 255.255.255.0 secondary
 ip address 199.171.11.4 255.255.255.0 secondary
 ip address 199.171.12.4 255.255.255.0 secondary
 ip address 199.171.13.4 255.255.255.0 secondary
 ip address 199.171.14.4 255.255.255.0 secondary
 ip address 199.171.15.4 255.255.255.0 secondary
 ip address 199.171.16.4 255.255.255.0 secondary
!
interface fastethernet 0/0
 ip address 150.1.13.4 255.255.255.0
 no shutdown
!
interface fastethernet 0/1
 ip add 13.13.14.4 255.255.255.0
 no shutdown
!
router eigrp 100
 no auto-summary
 network 10.1.0.0     0.0.3.255
 network 172.16.4.0   0.0.3.255
 network 192.168.8.0  0.0.3.255
 network 199.171.0.0  0.0.15.255
 network 199.171.16.0 0.0.0.255
 end
!
```

## 🔧 R5

```cisco
en
conf t
!
no ip domain-lookup
!
enable secret cisco
!
hostname R5
!
line con 0
 logging sync
 exec-timeout 0 0
 password cisco
 login
!
line vty 0 4
 password cisco
 login
!
interface loopback 0 
 ip add 13.13.5.5 255.255.255.0
!
interface loopback 4
 ip address 4.1.1.4 255.255.255.0
!
interface loopback 128
 ip address 128.28.2.4 255.255.255.0 secondary
 ip address 128.128.1.4 255.255.255.0
!
interface loopback 198
 ip address 198.198.22.4 255.255.255.0 secondary
 ip address 198.198.21.4 255.255.255.0 secondary
 ip address 198.198.5.4  255.255.255.0 secondary
 ip address 198.198.4.4  255.255.255.0 secondary
 ip address 198.198.1.4  255.255.255.0 secondary
 ip address 198.2.1.4    255.255.255.0 secondary
 ip address 198.2.3.4    255.255.255.0 secondary
 ip address 198.2.5.4    255.255.255.0 secondary
 ip address 198.1.1.5    255.255.255.252
!
interface fastethernet 0/0
 ip add 150.3.13.5 255.255.255.0
 no shutdown
!
interface fastethernet 0/1
 ip add 13.13.15.5 255.255.255.0
 no shutdown
!
router eigrp 100
 no auto-summary
 network 4.1.1.0       0.0.0.255
 network 128.28.2.0    0.0.0.255
 network 128.128.1.0   0.0.0.255
 network 198.1.1.0     0.0.0.255
 network 198.2.1.0     0.0.0.255
 network 198.2.3.0     0.0.0.255
 network 198.2.5.0     0.0.0.255
 network 198.198.1.0   0.0.0.255
 network 198.198.4.0   0.0.0.255
 network 198.198.5.0   0.0.0.255
 network 198.198.21.0  0.0.0.255
 network 198.198.22.0  0.0.0.255
 end
!
```
