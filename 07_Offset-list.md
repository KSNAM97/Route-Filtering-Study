# 07. Offset-list

## 📖 개념

- Routing Table에 등록된 특정 네트워크의 Metric 값을 조정하여 **최적 경로 변경**
- Offset-list는 **Metric의 증가만** 가능 (감소 불가)
- 하나의 Routing Protocol Process에서 Interface당 **in/out 각 1번씩**만 적용

---

## 🔧 설정 방법

1. ACL로 Metric 변경할 네트워크 범위 지정
2. Routing Protocol Process에서 해당 네트워크의 Metric 조정

---

## 📝 EX1) 경로 변경

> R4의 13.13.14.0/24 → R5의 13.13.15.0/24 통신 시 **R2 경유**

### R1

```cisco
access-list 13 permit 13.13.15.0  0.0.0.255
!
router eigrp 100
 offset-list 13 in 700000 s1/1
!
```

### R3

```cisco
access-list 13 permit 13.13.15.0  0.0.0.255
!
router eigrp 100
 offset-list 13 out 700000 s1/1
!
```

---

## ✅ 정보 확인

```cisco
R1# show ip route eigrp | include 13.13.15.0
D       13.13.15.0 [90/2686976] via 13.13.9.2, 00:00:30, Serial1/0.123

R4# traceroute 13.13.15.5 source 13.13.14.4
  1 150.1.13.1
  2 13.13.9.2     ! R2 경유 ✅
  3 13.13.9.3
  4 150.3.13.5
```
