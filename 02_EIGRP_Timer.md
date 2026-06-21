# 02. EIGRP Timer 조정

## 📌 요구사항
- R1, R2, R3 Frame-Relay 구간에서 Hello-interval 5초, Hold-time 15초
- R1, R3 Frame-Relay 구간에서 Hello-interval 5초, Hold-time 15초

---

## 🔧 R1

```cisco
interface serial 1/0.123
 ip hello-interval eigrp 100 5
 ip hold-time eigrp 100 15
!
interface serial 1/1
 ip hello-interval eigrp 100 5
 ip hold-time eigrp 100 15
!
```

## 🔧 R2

```cisco
interface serial 1/0.123 
 no ip split-horizon eigrp 100
 ip hello-interval eigrp 100 5
 ip hold-time eigrp 100 15
!
```

## 🔧 R3

```cisco
interface serial 1/0.123
 ip hello-interval eigrp 100 5
 ip hold-time eigrp 100 15
!
interface serial 1/1
 ip hello-interval eigrp 100 5
 ip hold-time eigrp 100 15
!
```

---

## ✅ 정보 확인

```cisco
show ip eigrp neighbor
show ip eigrp interfaces detail
```
