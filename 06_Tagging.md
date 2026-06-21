# 06. Tagging

## 📖 개념

- RIP, EIGRP, OSPF 등을 사용하여 라우팅 되는 네트워크 정보에 Tag 값을 부여
- Tag 값을 사용하여 트래픽을 Filtering 하는 기능

---

## 📝 EX4-1) Tag 부여

> R4는 Local 네트워크 중 199.171.8.0/24 ~ 199.171.15.0/24 에  
> EIGRP 라우팅 업데이트 시 TAG 100 Tagging 후 송신

```cisco
! R4
access-list 41 permit 199.171.8.0  0.0.7.255
!
route-map R4_TAG100 permit 10
 match ip address 41
 set tag 100
!
route-map R4_TAG100 permit 20
!
router eigrp 100
 distribute-list route-map R4_TAG100 out fastethernet 0/0
!
```

### ✅ 확인

```cisco
R1# show ip eigrp topology | include tag 
P 199.171.12.0/24, 1 successors, FD is 409600, tag is 100
P 199.171.13.0/24, 1 successors, FD is 409600, tag is 100
...
```

---

## 📝 EX4-2) Tag 기반 차단

> R5는 199.171.8.0/24 ~ 199.171.15.0/24 를 수신하지 않아야 함  
> (단, ACL / Prefix-list 사용 불가)

```cisco
! R5
route-map TAG100_DENY deny 10
 match tag 100
!
route-map TAG100_DENY permit 20
!
router eigrp 100
 distribute-list route-map TAG100_DENY in fastethernet 0/0
!
```

---

## 📝 EX5-1) 사설망 차단 + Tag 분류 (R1)

> - R1은 R4로부터 모든 사설망 제외하고 수신
> - 199.171.0.0/24 ~ 199.171.7.0/24 → TAG 150
> - 나머지 → TAG 100

```cisco
! R1
ip prefix-list PRIVATE permit 10.0.0.0/8 le 32
ip prefix-list PRIVATE permit 172.16.0.0/12 le 32
ip prefix-list PRIVATE permit 192.168.0.0/16 le 32
!
access-list 41 permit 199.171.0.0  0.0.7.255
!
route-map R4_FROM deny 5
 match ip address prefix-list PRIVATE
!
route-map R4_FROM permit 10
 match ip address 41
 set tag 150
!
route-map R4_FROM permit 20
 set tag 100
!
router eigrp 100
 distribute-list route-map R4_FROM in fastethernet 0/0
!
```

---

## 📝 EX5-2) 198.198.x.0 차단 + Tag 분류 (R3)

> - R3은 R5로부터 198.198.x.0/24 제외하고 수신
> - 13.13.5.0/24, 13.13.15.0/24, 4.1.1.0/24, 128.128.1.0/24 → TAG 200
> - 나머지 → TAG 250

```cisco
! R3
ip prefix-list NET198 permit 198.198.0.0/16 le 32
!
access-list 35 permit 13.13.5.0    0.0.0.255
access-list 35 permit 13.13.15.0   0.0.0.255
access-list 35 permit 4.1.1.0      0.0.0.255
access-list 35 permit 128.128.1.0  0.0.0.255
!
route-map R3_FROM deny 5
 match ip address prefix-list NET198
!
route-map R3_FROM permit 10
 match ip address 35
 set tag 200
!
route-map R3_FROM permit 20
 set tag 250
!
router eigrp 100
 distribute-list route-map R3_FROM in fastethernet 0/0
!
```

---

## 📝 EX5-3) Tag 기반 종합 필터링 (R2)

> - R4: 199.171.0.0/24 ~ 199.171.7.0/24 = TAG 150 / 나머지 TAG 100
> - R5: 13.13.5.0, 13.13.15.0, 4.1.1.0, 128.128.1.0 = TAG 250 / 나머지 TAG 200
> - R2는 TAG 150, 250 을 차단

### Method 1) 별도 Line (AND처럼 별개 항목)

```cisco
! R2
route-map EIGRP_FIL deny 10
 match tag 150
!
route-map EIGRP_FIL deny 20
 match tag 250
!
router eigrp 100
 distribute-list route-map EIGRP_FIL in s1/0.123
!
```

### Method 2) 동일 Line (OR)

```cisco
! R2
route-map EIGRP_FIL deny 10
 match tag 150 250
!
route-map EIGRP_FIL permit 20
!
router eigrp 100
 distribute-list route-map EIGRP_FIL in s1/0.123
!
```
