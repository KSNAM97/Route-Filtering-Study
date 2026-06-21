# 03. Prefix-list

## 📖 개념

- ACL과 함께 가장 많이 사용되는 Filtering 기능
- ACL로 구성할 수 없는 형식으로 Prefix-list를 사용하여 Filtering 가능
- 네트워크의 범위 지정 시에만 사용 가능 (방화벽 구성 불가)
- 재분배 시 `null = 0` 제외 송신할 때 사용

---

## 📐 기본 형식

```cisco
ip prefix-list [Name] seq [Permit/Deny] [A.B.C.D]/subnetmask [ge X] [le Y]
```

- `le Y` : subnetmask가 X부터 Y까지 모두 포함
- `ge X le Z` : X 네트워크 범위 내에서 X부터 Z까지의 subnetmask 포함
- ⚠️ 설정한 Prefix-list로 지정한 나머지 모든 트래픽은 **차단**

---

## 📝 예제

### EX1) 특정 네트워크만 허용

```cisco
! 172.16.0.0/24, 172.16.3.0/24 만 수신
ip prefix-list NET172 seq 5  permit 172.16.0.0/24
ip prefix-list NET172 seq 10 permit 172.16.3.0/24
```

### EX2) 172.16.0.0/16 허용

```cisco
ip prefix-list NET172 seq 15 permit 172.16.0.0/16
```

### EX2-2) 172.16.0.0 ~ 172.16.255.255 허용

```cisco
ip prefix-list NET172 seq 20 permit 172.16.0.0/16 le 32
```

### EX3) 10.0.0.0/8 관련

```cisco
! 10.0.0.0/8 만 허용
ip prefix-list NET10 seq 5 permit 10.0.0.0/8

! 10.0.0.0 ~ 10.255.255.255 모두 허용
ip prefix-list NET10 seq 10 permit 10.0.0.0/8 le 32
```

### EX4) 172.16.0.0 ~ 172.16.7.255 허용

```cisco
ip prefix-list NET172 permit 172.16.0.0/21 le 32
```

### EX5) 100.100.8.0 ~ 100.100.15.255 차단

```cisco
ip prefix-list NET100 deny   100.100.8.0/21 le 32
ip prefix-list NET100 permit 0.0.0.0/0 le 32
```

### EX6) 182.160.0.0/16 중 subnet /24만 허용

```cisco
ip prefix-list NET182 permit 182.160.0.0/16 ge 24 le 24
```

### EX7) 100.0.0.0/8 중 /25 ~ /27 허용

```cisco
ip prefix-list NET100 permit 100.0.0.0/8 ge 25 le 27
```

### EX8) 사설 주소 차단, 그 외 모두 허용

```cisco
ip prefix-list EDITORIAL deny   10.0.0.0/8 le 32
ip prefix-list EDITORIAL deny   172.16.0.0/12 le 32
ip prefix-list EDITORIAL deny   192.168.0.0/16 le 32
ip prefix-list EDITORIAL permit 0.0.0.0/0 le 32
```

---

## 🧪 실습 환경 구성

### R4

```cisco
interface loopback 172
 shutdown
!
```

### R5

```cisco
interface loopback 172
 ip address 172.16.1.1   255.255.255.224
 ip address 172.16.2.2   255.255.255.224 secondary
 ip address 172.16.3.3   255.255.255.224 secondary
 ip address 172.16.4.4   255.255.255.192 secondary
 ip address 172.16.5.5   255.255.255.192 secondary
 ip address 172.16.6.6   255.255.255.192 secondary
 ip address 172.16.7.7   255.255.255.128 secondary
 ip address 172.16.8.8   255.255.255.128 secondary
 ip address 172.16.9.9   255.255.255.128 secondary
 ip address 172.16.10.10 255.255.255.0   secondary
 ip address 172.16.11.11 255.255.255.0   secondary
 ip address 172.16.12.12 255.255.255.0   secondary
!
router eigrp 100
 network 172.16.0.0 0.0.15.255
!
```

---

## 🧪 실습 문제

### 문제 1) 특정 네트워크 4개만 수신

> R3은 R5로부터 수신하는 EIGRP 네트워크 정보 중  
> 172.16.1.0/27, 172.16.4.0/26, 172.16.7.0/25, 172.16.10.0/24 만 수신

```cisco
! R3
ip prefix-list NET172 permit 172.16.1.0/27
ip prefix-list NET172 permit 172.16.4.0/26
ip prefix-list NET172 permit 172.16.7.0/25
ip prefix-list NET172 permit 172.16.10.0/24
!
router eigrp 100
 distribute-list prefix NET172 in fa0/0
!
```

### 문제 2) /25 ~ /27 만 수신

> R3은 R5로부터 수신하는 172.16.0.0/16 중 /25 ~ /27 만 수신

```cisco
! R3
ip prefix-list NET172 permit 172.16.0.0/16 ge 25 le 27
!
router eigrp 100
 distribute-list prefix NET172 in fa0/0
!
```

### 문제 3) /25 ~ /27 만 수신 + 172.16 외 나머지 모두 수신

```cisco
! R3
ip prefix-list NET172 permit 172.16.0.0/16 ge 25 le 27
ip prefix-list NET172 deny   172.16.0.0/16 le 32
ip prefix-list NET172 permit 0.0.0.0/0 le 32
!
router eigrp 100
 distribute-list prefix NET172 in fa0/0
!
```

---

## ✅ 정보 확인

```cisco
show ip route eigrp | include 172
show ip prefix-list
```
