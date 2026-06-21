# 05. Distribute-list

## 📖 개념

- Routing Protocol(RIP, EIGRP, OSPF)을 사용하여 네트워크 정보 업데이트 시  
  List를 구성하여 지정된 네트워크 정보에 대해서만 송/수신 가능
- 특정 네트워크만 송/수신 또는 특정 네트워크 제외하고 송/수신
- 하나의 Routing Protocol Process에서 Interface당 **in/out 각 1번씩**만 적용 가능

---

## 🔧 설정 방법

1. ACL / Prefix-list / Route-map 으로 범위 지정
2. Routing Protocol Process에서 `distribute-list` 로 적용

---

## 📝 EX1) 특정 네트워크만 수신

> R1은 R4로부터 수신하는 EIGRP 중  
> 199.171.8.0/24 ~ 199.171.15.0/24 만 수신

### R1 (수신 측)

```cisco
access-list 1 permit 199.171.8.0  0.0.7.255
!
router eigrp 100
 distribute-list 1 in fastethernet 0/0
!
```

### OR — R4 (송신 측)

```cisco
access-list 1 permit 199.171.8.0  0.0.7.255
!
router eigrp 100
 distribute-list 1 out fastethernet 0/0
!
```

### 삭제

```cisco
no access-list 1
!
router eigrp 100
 no distribute-list 1 in fastethernet 0/0
!
```

---

## 📝 EX2) 특정 네트워크 제외하고 수신

> R3은 R5와 연결된 fa0/0으로 수신 EIGRP 중  
> 198.198.x.0/24 제외 나머지 모두 수신

### R3

```cisco
access-list 2 deny   198.198.0.0  0.0.255.255
access-list 2 permit any
!
router eigrp 100
 distribute-list 2 in fastethernet 0/0
!
```

### OR — R5

```cisco
access-list 2 deny   198.198.0.0  0.0.255.255
access-list 2 permit any
!
router eigrp 100
 distribute-list 2 out fastethernet 0/0
!
```

---

## 📝 EX3) 사설 네트워크 모두 차단

> R3은 R1으로부터 수신 EIGRP 중 모든 사설 네트워크 제외

### R1

```cisco
access-list 3 deny   10.0.0.0     0.255.255.255
access-list 3 deny   172.16.0.0   0.15.255.255
access-list 3 deny   192.168.0.0  0.0.255.255
access-list 3 permit any
!
router eigrp 100
 distribute-list 3 out s1/1
 distribute-list 3 out s1/0.123
!
```

---

## 📝 EX4) Prefix-list / Route-map 활용

> R3은 R5로부터 13.13.5.0/24, 13.13.15.0/24, 198.198.x.0/24 만 수신

### Method 1) ACL

```cisco
access-list 1 permit 13.13.5.0  0.0.0.255
access-list 1 permit 13.13.15.0 0.0.0.255
access-list 1 permit 198.198.0.0 0.0.255.255
!
router eigrp 100
 distribute-list 1 in fastethernet 0/0
!
```

### Method 2) Prefix-list

```cisco
ip prefix-list R3<---R5_DISTRIBUTE_LIST permit 13.13.5.0/24
ip prefix-list R3<---R5_DISTRIBUTE_LIST permit 13.13.15.0/24
ip prefix-list R3<---R5_DISTRIBUTE_LIST permit 198.198.0.0/16 le 32
!
router eigrp 100
 distribute-list prefix R3<---R5_DISTRIBUTE_LIST in fastethernet 0/0
!
```

### Method 3) Route-map + ACL

```cisco
access-list 1 permit 13.13.5.0   0.0.0.255
access-list 1 permit 13.13.15.0  0.0.0.255
access-list 1 permit 198.198.0.0 0.0.255.255
!
route-map EIGRP_DISTRIBUTE permit 10
 match ip address 1
!
router eigrp 100
 distribute-list route-map EIGRP_DISTRIBUTE in fastethernet 0/0
!
```

### Method 4) Route-map + Prefix-list

```cisco
ip prefix-list EIGRP permit 13.13.5.0/24
ip prefix-list EIGRP permit 13.13.15.0/24
ip prefix-list EIGRP permit 198.198.0.0/16 le 32
!
route-map EIGRP_DISTRIBUTE permit 10
 match ip address prefix EIGRP
!
router eigrp 100
 distribute-list route-map EIGRP_DISTRIBUTE in fastethernet 0/0
!
```
